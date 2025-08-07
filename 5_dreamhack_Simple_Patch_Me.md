

### 문제
<img width="1264" height="288" alt="image" src="https://github.com/user-attachments/assets/5eb5d0a0-d8ac-4c38-a6ad-fba333ea82e7" />


### 문제풀이
실제시간 365일이 흘러가면 플래그가 나오는것으로 보아 간단하게는... 시간을 건드리면 해결될 것 같네요

이 파일도 ELF 파일로 IDA에서 확인 후 리눅스를 통해 열어보겠습니다.


IDA에서 문자열 검색을 통해 확인해본 값으로 
1 시간,  1일 passed 뒤 1년이 지나면 Great 및 flag 정보가 나오는 걸로 확인되는 문자열이 확인됩니다.

<img width="679" height="288" alt="image" src="https://github.com/user-attachments/assets/61ccd857-3e36-4a24-a067-46896b3488a1" />

main 내 Graph로 볼 때 아래처럼 이루어져있네요 

----------

#### 크게 봤을 때 문제풀이 방법 

1. jmp 를 변경하기
2. cmp 비교값을 줄이기

---------
gdb를 확인할 때 위 2가지를 한 번 시도해보겠습니다.

<img width="1129" height="953" alt="image" src="https://github.com/user-attachments/assets/1307513d-32a7-402e-afdf-e432f8f7e2bf" />

우선 해당 code를 보겠습니다.

"dword_40404c" 가 1일때 "1 hour passed"가 출력되는 걸로 봐서 해당 변수는 "hour" 시간을 나타내는 것 같습니다.

반복문을 살펴보았을 때, while(dword_40404c <= 0x2337)은 2337 hex값을 10진수로 변경해보았을 때 8760이고 이는 365일을 시간으로 나타낸 숫자입니다.
이로써 8760시간 보다 적게 됐을 때  지속적으로 반복문이 진행되겠네요

<img width="559" height="510" alt="image" src="https://github.com/user-attachments/assets/a7668437-2486-43a8-9a47-9c0ffd991e52" />

sub_401196 부분에서 flag 값 나오는 것이 보여 sub_401196 도 확인해보겠습니다.

<img width="474" height="415" alt="image" src="https://github.com/user-attachments/assets/26adfafd-3f0b-4a5d-b7be-856cd421c396" />

우선 gdb로 프로그램 실행을 시켜보고 main 함수 , disassemble을 시도해보려고 했으나 sleep에 의해 진행되지 않아 보이네요.. 

<img width="796" height="279" alt="image" src="https://github.com/user-attachments/assets/849c18bd-4228-4202-ad0c-dd7bb04b4262" />

--------------------------------------

#### 1번째 방법 jmp 변경

위에서 생각해보기로한 1번째 방법인 jmp를 변경해보겠습니다.

IDA code 에서 while 반복문이 진행될 때를 보니 8760시간보다 작거나 같으면 점프 즉, 이를 어셈블리어로 보면 jbe로 점프가 되는것을 확인했습니다.

JBE란 Jump If Below OR Equal로 뒤에 값이 크거나 같다라는 뜻입니다. (<=)

<img width="419" height="26" alt="image" src="https://github.com/user-attachments/assets/2118adad-549b-4b12-9fff-a8048e555b5e" />

code를 확인해보니 아예 while 반복문을 안들어가면 flag값을 print 하는것처럼 보이더라구요 

그럼 JBE가 뒤에값이 더 크거나 같을 때 반복문에 들어가는거니 이를 앞에 값이 더 클 때 반복문에 진입하게 하면 문제가 해결될 것으로 보입니다.

JA : Jump If Above , 앞에 값이 더 크면 점프

한번 패치를 통해 JBE 값을 JA로 변경해보겠습니다.

우선 IDA 패치 사용법을 몰랐어서 그거부터 찾아보았습니다. 

방법으로는 Alt + F2라고 하던데 한 번 적용해보겠습니다.


<img width="2044" height="985" alt="image" src="https://github.com/user-attachments/assets/e4985498-23ba-4ee6-b32c-acfa52fcb48f" />

* 코드 변경 후 [Edit] - [Patch Program] - [Apply Patches to input files] 로 patch 해줘야합니다.

이를 진행 후 파일을 다시 gdb로 실행하였으나...

알아볼 수 없는 문자형태가 나와버렸습니다.. 어디가 문제가 된걸까요..

<img width="835" height="104" alt="image" src="https://github.com/user-attachments/assets/239a9b44-3d52-48d4-a6c1-f0e99a4afc4c" />

--------------------------

#### 2번째방법 cmp 관련 값 패치

시간을 다 보내지 않아서일까요... 이 방법이 안된다면 2번째 방법으로 넘어가보겠습니다. cmp 비교값을 0으로 만들어보고 혹은 비교대상인 eax 값을 0시간으로 비교하게끔 해보겠습니다.

우선 실행중인 시간과 비교하는 eax값에 0x2238 (=8760시간) 바로 넣기를 진행
<img width="1237" height="715" alt="image" src="https://github.com/user-attachments/assets/11abd14b-370e-4cd5-a4e2-abdcfa249651" />

바꿔보니... 그래프가 깨져버립니다... 이래서는 실행이 안되네욤..

<img width="1057" height="956" alt="image" src="https://github.com/user-attachments/assets/8e88f6d2-04e8-45d2-826b-8a1543052b05" />

cmp 관련 값들을 바꾸면 모두 그래프가 깨지더라구요... 제가 생각한 두 방법 다 문제가 발생합니다.

하 그럼 제가 바꿀 수 있는 부분은 뭐가 있을까요... 뭔가 시간을 다 보내면서 해야될 것 같은데

그렇게 몇시간 생각해본 후 sleep이 1시간씩 되어 있는데 이걸 바꿔봐야겠다는 생각이 들었습니다.

-------------------------

그럼 3번째 방법인 시간을 다 보내게 하는 sleep을 변경해보겠습니다...

#### 3번째 방법 sleep 시간 변경

우선 Alt + F2로 패치말고도 아래 그림으로도 할 수 있네요 그 이후 패치적용은 똑같이하면 됩니다.

<img width="552" height="623" alt="image" src="https://github.com/user-attachments/assets/ee5d3d8f-9b95-4dd5-882a-bf28ff1bbc21" />

sleep 함수를 call하기 전에 얼마나 sleep할 것인지에 대한 값을 0xe10(=3600) 3600초에서 0초로 지정하였습니다.

<img width="229" height="176" alt="image" src="https://github.com/user-attachments/assets/18c0ccef-6346-4218-b5e7-5bae5b37b3fa" />

<img width="219" height="200" alt="image" src="https://github.com/user-attachments/assets/00286787-5285-49c2-881c-9f911ba6c352" />

이를 gdb에서 실행시켜보니 jmp 랑 cmp를 바꿔 깨진문자가 나온것과는 다르게 정상적으로 문자가 보이네요 휴 다행히 문제해결했습니다.

<img width="818" height="640" alt="image" src="https://github.com/user-attachments/assets/bc6023c7-6c10-40e1-8fee-682c9128c363" />

--------------------------

#### 요약
실행 후 1년이 지나면 flag 값이 나오는 문제

내가 생각한 방법은 총 3가지
1. jmp 값을 1년이 넘었을 때(jbe) 가 아닌 1년이 되지 않았을 때(ja) flag가 보이게 진행 = 실패, flag는 보이나 문자가 깨짐
2. cmp 비교값을 변경, 내가 시작한 시간을 0시간이 아닌 8760시간으로 변경 혹은 비교대상인 eax 값을 0시간으로 변경 = 실패, IDA그래프가 깨짐 gdb실행이 안됨 size가 맞지 않아서 그런가... short 이런걸 넣었어야하나...
3. sleep 값을 3600초가 아닌 0초로 변경 = 성공, 시간을 모두 보내어 정상적인 flag값 보이는 것 확인, 문제 해결

