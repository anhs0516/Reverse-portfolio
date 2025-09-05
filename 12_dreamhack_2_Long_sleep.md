### 문제

<img width="1203" height="318" alt="image" src="https://github.com/user-attachments/assets/748f63e0-5f93-497e-8168-8ecb319b302f" />


출처 : https://dreamhack.io/wargame/challenges/635






### 분석

압출을 풀면 prob라는 파일이 생기네요 

이 파일을 PEview를 통해 보면 ELF 파일 형식을 가지고 있다는 것을 알 수 있습니다.

<img width="1446" height="259" alt="image" src="https://github.com/user-attachments/assets/0cd5f98a-2b32-45c8-bd3c-11301680c079" />

이 문제 또한 IDA와 GDB를 통해 문제를 풀어가야겠네요

우선 문제를 읽어보면 sleep 후에 결과값이 나오는 것 같은데 sleep 시간이 길게 잡혀있어 결과값을 볼 수 없게 되어있나봐요 

IDA 먼저 살펴보겠습니다.

* IDA 문자열

<img width="824" height="371" alt="image" src="https://github.com/user-attachments/assets/1d1784f3-1645-400e-84f5-3c7b3a40b1a6" />

* IDA GraphView

call sub_1411 이후 "Here's your flag: DH{" 문자열이 보이는 걸로 봐서 sub_1411이 중요해보입니다.

<img width="592" height="962" alt="image" src="https://github.com/user-attachments/assets/3589933d-e888-4543-8852-859f18f5dd00" />

* source code
  sub_1411 함수를 잘 봐야할 것이고 반복문을 보면 DH{32글자}  겠네요
  

<img width="429" height="251" alt="image" src="https://github.com/user-attachments/assets/2ddb5721-76b5-41d6-822e-bebf888e8acc" />


보여지는 함수에 대한 분석
  
* sub_1411

해당 함수 안에 다양한 함수들을 또 불러오네염...

<img width="447" height="236" alt="image" src="https://github.com/user-attachments/assets/57bf9bad-4be4-436e-ad03-45d8accd2ac8" />

* sub_19F5
  
<img width="360" height="267" alt="image" src="https://github.com/user-attachments/assets/00b2aa31-117e-43b2-afd5-5ae037afdcba" />

* sub_1A73

<img width="630" height="307" alt="image" src="https://github.com/user-attachments/assets/2472271a-6e6d-486d-9d15-e3965acd7ef3" />

* sub_1B14

<img width="616" height="864" alt="image" src="https://github.com/user-attachments/assets/4443f6f6-88b6-4e84-9e58-b1f2c6a6b2fc" />

보다보니 sub_14D2에 nano sleep이 걸려 있는걸 확인


<img width="338" height="644" alt="image" src="https://github.com/user-attachments/assets/4b7aebac-a517-4eef-bca0-9ac0a7e5eb74" />

이 sleep을 건너뛰면 어떨까 생각하여 문제풀이 진행해보겠습니다.


### 문제풀이

1. nano sleep 통해 sleep하는 것을 건너뛰기

1536 syscall nanosleep을 nop으로 변경 혹은 시간받아오는 부분을 0으로 시도해보겠습니당

<img width="1121" height="471" alt="image" src="https://github.com/user-attachments/assets/f5991b9c-696c-4cf0-8362-5dad18d994fa" />

GDB 실행 결과

integrity check 무결성체크에서 걸리는 것 같네요 여기도 건너뛰거나 다른 방법을 찾아야할 것 같습니다.

<img width="1214" height="64" alt="image" src="https://github.com/user-attachments/assets/f1a2afa3-2d3a-482a-a4e3-f0b24e8fb9b0" />

아래 결과값이 없어서 무결성체크에서 걸리는 것 같아요 

<img width="805" height="387" alt="image" src="https://github.com/user-attachments/assets/85d0cc48-7723-49fd-b671-78b2d1782d31" />







### 결과


<img width="928" height="110" alt="image" src="https://github.com/user-attachments/assets/9fe97ad1-361a-451c-bf39-f434c03ad9e3" />




### 느낀점
