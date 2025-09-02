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


* sub_1411

해당 함수 안에 다양한 함수들을 또 불러오네염...

<img width="447" height="236" alt="image" src="https://github.com/user-attachments/assets/57bf9bad-4be4-436e-ad03-45d8accd2ac8" />


* 각 함수 분석



### 문제풀이




### 결과




### 느낀점
