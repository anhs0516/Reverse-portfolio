### 문제

<img width="828" height="270" alt="image" src="https://github.com/user-attachments/assets/e850bce3-899a-4227-814e-5b1b64711c67" />



### 문제풀이

이번 문제도 ELF 파일이네요

IDA로 파일을 확인해보고 리눅스 GDB를 이용해서 프로그램 실행하여 flag 값을 찾아보도록 하겠습니다. 

우선 문자열들을 확인하는 Shift + F12를 통해 보겠습니다.

<img width="591" height="313" alt="image" src="https://github.com/user-attachments/assets/097f44f0-48be-465c-aec2-ef849d547dde" />

flag 관련된 내용들이 보이네요 해당 문자열들을 가진 곳을 찾아가 자세히 보도록 하겠습니다.

<img width="1045" height="609" alt="image" src="https://github.com/user-attachments/assets/ac637612-f454-4bff-a245-3666ccbd73b3" />

해당 문자열들이 main에 존재하네요 
그래프로 볼때는 아래와 같습니다.

<img width="473" height="445" alt="image" src="https://github.com/user-attachments/assets/5a984688-bdd5-40b5-8f91-d3d9fdbed5ab" />

소스코드로 보면 아래로 보입니다. 뭔가 저번 문제와 비슷하지 않을까 생각드네요
* 참고 6번문제해결 내용을 확인해보세요

<img width="457" height="109" alt="image" src="https://github.com/user-attachments/assets/2c95a683-aeab-4235-bc56-063bfce46e41" />


