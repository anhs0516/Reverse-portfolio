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

우선 sub_40152B에서 실행되는 call들은 아래 내용이에요

각 내용들에서 jle, ja, jb로 설정되어 있어서 flag 값들을 못만들고 있나 생각이 들어 모두 jmp로 항상 넘어가도록 설정해보았습니다.

<img width="126" height="304" alt="image" src="https://github.com/user-attachments/assets/ca9311b2-a8a1-429a-b4c7-b4a5165caea8" />

* 4012B7
  <img width="1018" height="595" alt="image" src="https://github.com/user-attachments/assets/dedc6609-e153-486d-8840-fbd5d857eb7c" />
* 40126A
<img width="1029" height="627" alt="image" src="https://github.com/user-attachments/assets/38ce6693-d515-4b5b-9157-c1aeb411065b" />

* 401301
<img width="983" height="862" alt="image" src="https://github.com/user-attachments/assets/4dcec8bc-58cd-4c14-9194-319e11d3e652" />

* 401388
<img width="1113" height="938" alt="image" src="https://github.com/user-attachments/assets/a79c43ed-3bf8-4c6a-aff7-ff22c4c8ffdd" />

* 401444
<img width="922" height="861" alt="image" src="https://github.com/user-attachments/assets/e1500e76-e578-451e-81a3-dc9edfa01bf9" />



이를 패치 후 한번 gdb로 실행해볼게요

<img width="739" height="125" alt="image" src="https://github.com/user-attachments/assets/ce6ec646-4f68-48f2-a586-4b93db4c76a9" />

flag 값들을 출력하지 못하고 있습니다. 문자열을 만드는게 실패했는지 잘 모르겠네요... 아직 많이 부족합니다.. 어셈블리어를 정확히 해석하는 능력도 아직 부족하네요..

<img width="795" height="275" alt="image" src="https://github.com/user-attachments/assets/395e3c35-e02b-49bb-934b-433b727b3d89" />

이제까지 제가 생각한 방법은
1. sub_40152B를 실행하지 않고 넘기는 저번방식으로 진행

※ 그러나, 이번에는 sub_40152B 함수가 비어있는게 아닌 실제로 flag 값들을 만드는 내용으로 판단되어 이 함수는 실행되어야만 하는 부분입니다.

3. 그럼 sub_40152B 실행 후 즉시 값을 출력하는 곳으로 jmp 하자

※ sub_40152B에서 실행한 값을 rax값에 넣지 않고 바로 출력하려고하니 당연히 문제해결이 안되는게 맞습니다... 생각이 짧았습니당..

4. 제가 생각한 결론은 그럼 sub_40152B 실행 후 "I have return the flag" 이 값으로 rax에 덮어쓰는 부분만 없애자라고 생각하여 해당 부분을 nop으로 변경하였습니다.

진행은 아래와 같은 순서로 진행하였습니다.

<img width="424" height="434" alt="image" src="https://github.com/user-attachments/assets/bbeba108-da5e-4100-bf05-6f0f313b493f" />

위 내용을 아래의 값으로 patch 진행

<img width="955" height="578" alt="image" src="https://github.com/user-attachments/assets/68ec7ca7-6c29-4837-8cc0-bebf4968c25f" />

최종 문제해결입니다.. 

<img width="770" height="246" alt="image" src="https://github.com/user-attachments/assets/4a2ea952-cfbf-4169-b679-1362a8161ed8" />

--------------------------------

### 느낀점

아직 많이 부족하다..

Linux GDB도 잘 다루지 못하고 GDB로도 해결할 수 있어 보이는데 GDB로는 해결하지 못했따...

GDB strapped 되어있어 strip 하는법도 찾아봐야 할 것 같음

그리고 다른 사람 문제해결법도 확인해봐야할 듯 합니다 
