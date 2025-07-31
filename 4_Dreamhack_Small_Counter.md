
### 문제

<img width="824" height="242" alt="image" src="https://github.com/user-attachments/assets/5585d861-a8f9-472a-8020-4d48b04fc2b4" />

힌트가 있길래 눌러보았으나 페이지를 찾을 수 없음...

일단 한번 쭉보겠습니다...


### 문제풀이
이 문제는 ELF 파일로 , ELF(Executable and Linkable Format) 파일은 유닉스 계열 운영체제에서 사용되는 표준 바이너리 파일 형식입니다.
IDA에서는 열리기 때문에 우선 IDA에서 확인해보겠습니다.



Ctrl+F12 문자열찾기로 아래 문자들이 확인되네요

<img width="712" height="315" alt="image" src="https://github.com/user-attachments/assets/5196e0da-5322-46f8-8023-2d810948e85c" />

---counter--- , ---END---
뭔가 시작과 끝을 나타내는 것 같습니다


그래프를 확인해보니 counter로 시작하고 중간에 cmp 비교를 통해 NICE 단어가 보이면 flag 값을 확인하는 쪽으로 갈 것 같아 보입니다.

<img width="906" height="968" alt="image" src="https://github.com/user-attachments/assets/81f7fdbb-1bc0-4745-8d1f-fe75bd41717e" />


코드를 보면 아래와 같은데 잘 모르겠습니다. ELF를 실행해보아야 할 것 같아 기존에 깔려있던 제 linux로 gdb를 설치하여 실행해보겠습니다.

<img width="823" height="323" alt="image" src="https://github.com/user-attachments/assets/998d4254-cd6e-45a1-8b9d-a1abebf569d3" />

실행했을 땐 아래와 같이 보이네요

<img width="290" height="265" alt="image" src="https://github.com/user-attachments/assets/e2400446-9227-426b-9209-d4d5f7046022" />

```code
(gdb) disassemble main


```
disassemble을 시도한 내용은 아래와 같습니다.
dbg를 처음 만져봐서 잘 모르겠네요...

<img width="1176" height="1627" alt="image" src="https://github.com/user-attachments/assets/2afe06c5-96bf-4b47-aa84-c3168d3008b7" />


nice로 넘어가기 위한 cmp와 jump 관련 부분을 찾아보니 main+278과 연관되어 있어 보입니다.

처음 아래 부분의 값을 5로 set 해주면 될 것 같아서 진행해봤습니다.
<img width="634" height="50" alt="image" src="https://github.com/user-attachments/assets/f67733d1-81c5-4e6b-9292-5bb67615233c" />

IDA에서 확인한 NICE로 이동하여 IDA에서 확인되는 flag값처럼 보인 IM{***}가 확인됩니다. 뭔가 잘못됐습니다. DH{로 시작하는 값이 발견되어야 하는데 왜그럴까 한참을 생각했어요

<img width="725" height="446" alt="image" src="https://github.com/user-attachments/assets/1b3d468c-6c84-422c-93ea-8d9ba3b9e764" />

생각해보니 breakpoint(BP)를 이미 카운터 다 세고 난 *main+278에 해놨으니 그 이후 값을 변경해봤자 의미가 없는 부분이라고 생각했습니다. 이미 값은 다 정해져서 흘러간걸로 판단됩니다.

우리가 확인했던데로 rbp-4의 값을 바꿔야 할 것 같네요 
이 명령어도 잘 모르겠어서 register 보는 명령어를 찾아보았습니다.
* info registers : 모든 레지스터리 값 확인
  

<img width="616" height="610" alt="image" src="https://github.com/user-attachments/assets/afe52ce7-335b-4c84-9260-4f54b05dad86" />

여기서 rbp의 -4를 진행 후 다시 시도해보겠습니다.

0x7FFFFFFFE2BC(rbp-4)의 값을 5로 변경 후 다시 진행해보겠습니다.

<img width="459" height="161" alt="image" src="https://github.com/user-attachments/assets/927deeb1-de3c-4e22-8da8-4d27fe46818d" />

계산하지 않고 "-" 뺄셈이 적용되는가 해봤는데 안되네요..
우선 rbp-4에 5값으로 set 후 진행하니 제가 알던 flag처럼 생겼네요

아래 값으로 하면 문제해결이됩니다.

<img width="712" height="207" alt="image" src="https://github.com/user-attachments/assets/a1c00cf1-d65b-4588-ae68-5fd80fb4e41e" />

- gdb 문제해결
```code
gdb chall
(gdb)r    / run
(gdb)disassemble main
(gdb)br *main+278 / breakpoint 설정
(gdb)r / bp설정 후 run하여 main+278에 breakpoint걸림
(gdb)info registers / 모든 레지스트리 값을 확인하여 rbp 값 찾기
(gdb)set *0x7FFFFFFFE2BC=5    /rbp-4를 5값으로 변경
(gdb)c /command

```
