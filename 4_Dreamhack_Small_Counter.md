
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


disassemble을 시도한 내용은 아래와 같습니다.
dbg를 처음 만져봐서 잘 모르겠네요...

<img width="1176" height="1627" alt="image" src="https://github.com/user-attachments/assets/2afe06c5-96bf-4b47-aa84-c3168d3008b7" />


nice로 넘어가기 위한 cmp와 jump 관련 부분을 찾아보니 main+278과 연관되어 있어 보입니다.
