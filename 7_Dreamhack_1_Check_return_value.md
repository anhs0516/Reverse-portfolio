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

-----------------------------------
위 코드와 그래프들을 봤을 때에 제가 낸 결론은 flag 값을 출력하기 전에 다른 값으로 변경하는 것이 문제라고 생각합니다.

문제해결방법으로는 지난번에 했던 것과 비슷한 유형으로 flag가 출력되기전에 다른값으로 바뀌어서 출력되는부분이 존재하여 flag값이 보이지 않는 것이 문제라고 생각 

완전 지난번처럼 하려했더니 지난번과는 달리 call sub_40152B에서 어떠한 작업을 수행하여 아래 방법으로는 문제해결이 되지 않음
<img width="1039" height="594" alt="image" src="https://github.com/user-attachments/assets/b38ba681-787c-4ab9-b4b9-dcd126acc56c" />

call sub_40152B까지 진행 후 그다음 출력하는 내용이 다른것으로 바뀌지 않도록 설정

<img width="1137" height="591" alt="image" src="https://github.com/user-attachments/assets/e3a82402-7843-40da-ad2f-8fd486bfe686" />

흐음 문제가 바로 해결되진 않네요 

다시 차근차근 짚고 넘어가기 위해 sub_40152B 내용을 한 번 살펴보겠습니다.

<img width="1028" height="903" alt="image" src="https://github.com/user-attachments/assets/e4e47fda-d798-46a3-a377-050818ce9f5b" />
