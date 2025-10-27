### 문제

<img width="816" height="224" alt="image" src="https://github.com/user-attachments/assets/6d98346f-4e70-4fce-8500-bb221e4b816d" />

출처 : https://dreamhack.io/wargame/challenges/932

### 분석


* PEview

<img width="2161" height="499" alt="image" src="https://github.com/user-attachments/assets/cfe874b7-72da-4df6-9c7d-2eb7473cd598" />

PEView로 보았을 때 ELF 형식으로 리눅스에서 실행해보겠습니다.

<img width="785" height="94" alt="image" src="https://github.com/user-attachments/assets/a2e26e48-a96a-4678-b0bf-6bc31725f07c" />

여기서 sleep이 아주 길게 되어있는것 같아요 ㅠ 일단 sleep을 1초로 변경하던지 없애는 방향으로 가봐야겠네요


문제를 읽어보았을 때, 정답은 68자리나 되는 것 같습니다. 나머지는 바로 유추되는게 없어서 IDA를 통해 문제를 보겠습니다.


* Main

<img width="618" height="622" alt="image" src="https://github.com/user-attachments/assets/79390821-e5d5-49db-bad5-3f3dde7278f4" />


Sleep도 보이고, if문 여러개가 보입니다. if문에 따라 DH Flag 값이 28자와 36자를 얻을 수 있는것으로 보입니다. 문제풀이로 가볼게요



### 문제 풀이

1. Sleep 우회하기

우선 Sleep 우회방법으로는 두 가지가 있을 것으로 보입니다..

첫째, IDA에서 Sleep 부분을 nop으로 패치하는 방법

둘째, GDB에서 실행 중 register 값을 변환하여 시간 조절하여 sleep을 빠져나오는 방법

매번 실행하면서 진행하여야 하니 첫번째 방법으로 IDA에서 변환해주겠습니다.

아래의 Sleep 부분을 nop으로 변환해보겠습니다.

변환 전

<img width="950" height="621" alt="image" src="https://github.com/user-attachments/assets/6344aeca-e676-4d1b-9256-fffaa38ce625" />

변환 후

<img width="973" height="641" alt="image" src="https://github.com/user-attachments/assets/8f2b5972-427f-4edb-998c-bf7947c038ae" />

IDA에서 Sleep 을 모두 nop으로 변환하여 바로 아래처럼 값을 입력하는 부분이 보이는것을 확인할 수 있습니다..

<img width="776" height="97" alt="image" src="https://github.com/user-attachments/assets/3623bcea-2692-4887-ba8e-6c00402ad783" />

   
2. if 문 두개를 거쳐 flag 값 얻어내기 

num에 알맞은 숫자를 넣으면 플래그가 보이는 것으로 보이는데 

1,2 를 넣었을 때는 flag 값이 하나도 보이지 않았습니다.

모두를 넣을 순 없으니 어느 부분이 실행될 때에 register에 어떠한 숫자와 일치해야 flag를 보여주는지 찾아봐야할 것 같아요

아래는 처음 if문에 접근하는 곳입니다.

여기에 접근할 때 내가 어느 값을 입력했고, 어느 값이 입력되었을 때 if문 통과하는지 확인할 수 있을 것 같아요

<img width="548" height="168" alt="image" src="https://github.com/user-attachments/assets/7641e64b-e4e7-4df7-854d-1a73e6b8b54d" />

이제 main에서부터 몇 번째 자리가 if문인지 GDB를 통해서 확인해 보겠습니다.

```
disassemble main

```

main 이미지

main+202에서 if 문을 지나는 것으로 보입니다. 여기에 breakpoint를 걸어보겠습니다.

```
b *main+202
```

<img width="700" height="99" alt="image" src="https://github.com/user-attachments/assets/65199a23-558a-4bcf-b0c2-ae32fa6a67ec" />



아래가 if문을 지날 때 당시 register들을 확인해보았습니다. 

rax는 제가 입력한 3이 출력되고 rcx에는 if문을 통과하기 위한 값인 5가 입력되어 있는것으로 보이네요

<img width="708" height="712" alt="image" src="https://github.com/user-attachments/assets/fead2b7b-8135-4f8a-be35-88eaa87c6bdd" />

그렇다면 5를 입력하게 되면 첫번째 if문을 지나 flag 28자리가 보일 것으로 보입니다. 확인해보겠습니다.

### 정리


### 느낀점
