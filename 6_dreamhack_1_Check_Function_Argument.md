
### 문제

<img width="748" height="279" alt="image" src="https://github.com/user-attachments/assets/908bf364-c76e-43d5-806a-360a63ffabab" />


### 문제풀이

이번 문제도 ELF파일이라 IDA랑 리눅스에서 GDB 등을 이용해봐야겠네요 

문제를 이해해봐야겠는데 플래그가 담긴 함수의 주소를 찾고 이를 출력하면 되는 문제로 판단됩니다.

IDA로 먼저 흐름을 확인해보겠습니다.

flag 관련 특정 문자열이나 main쪽을 보고 정답이라 판단되는 곳이 있는지 확인해볼게요

<img width="666" height="276" alt="image" src="https://github.com/user-attachments/assets/3536280c-7a1d-4633-b960-4973ac3ecb7d" />

"can you see that" 이란 문자열이 뭔가 정답과 연관이 되어 있을 것 같아보여서 살펴보겠습니다.

<img width="509" height="438" alt="image" src="https://github.com/user-attachments/assets/ca16ed42-187b-4ad1-8ae1-f5349c0533ca" />

여기에 소스코드는 아래와 같습니다

<img width="459" height="112" alt="image" src="https://github.com/user-attachments/assets/87e3e22b-b2e9-456d-bae4-ba4b587efc9a" />

---------------------------
내가 생각한 문제 해결 방법
1. return 0이 아니라 인자 하나를 return 하게되면??
2. 아래 그림을 보았을 때, call sub_4015E2 값이 아무것도 없다. 이를 출력하는 것으로 변경?

<img width="517" height="322" alt="image" src="https://github.com/user-attachments/assets/b5f233c8-734a-4657-85f6-a38a1fbf6a27" />

<img width="200" height="66" alt="image" src="https://github.com/user-attachments/assets/d472600a-3b69-40b3-91bd-aaa5ef69795b" />

3. "Can you see that?" 이 부분을 건너 뛰고 바로 출력하는 곳으로 가서 각 register에 들어간 값이 flag값으로 변경되어 출력되게 하기?
 
<img width="754" height="570" alt="image" src="https://github.com/user-attachments/assets/de033c22-cd00-4724-b35f-ace3b0aba4f1" />

---------------------------

우선 1번 방법은 잘모르겠습니다. 생각은 했는데 실행방법이 어려운 것 같아요

그래서 2번을 먼저 시도해보았습니다. 이 값을 call _puts으로도 변경해보았지만 실행되지 않네요

3번시도
call sub_4015E2 -> jmp 0x401621로 중간 부분을 실행하지 않고 바로 이동하여 register 값을 flag로 생각한 값을 출력하게끔 시도

<img width="1546" height="642" alt="image" src="https://github.com/user-attachments/assets/270032c5-d431-4a40-8905-56150ab6c9dc" />

patch하는 방법이 잘못되었나 생각해서 여러개 시도해보겠습니다. 아래는 여러번 시도한 내용입니다.

<img width="1570" height="664" alt="image" src="https://github.com/user-attachments/assets/71276814-a811-4437-96fc-f8a636b2518b" />

patch가 하나도 안됨..
<img width="1006" height="444" alt="image" src="https://github.com/user-attachments/assets/3a7c2c9d-857a-4177-b661-346482a5c7df" />

바꼈는데 GDB 오류발생
<img width="1064" height="436" alt="image" src="https://github.com/user-attachments/assets/65abb895-fdab-45fd-bde2-48ff7d8356a2" />

음.. 잘안바뀌네요
<img width="1044" height="573" alt="image" src="https://github.com/user-attachments/assets/6c39ab65-7937-47af-b62b-4af8702dfe1f" />



오오오 드디어 patch를 정상적으로 해냈습니다. 제가 접근한 방향이 맞았네요

<img width="736" height="276" alt="image" src="https://github.com/user-attachments/assets/bf742ae8-3349-43fc-9bc0-a4e475f3a3a6" />

------------------------------------------------------------------
### 찐 문제해결

우선 제가 의도한 바는 메인코드를 보았을 때, 아래와 같이 실행하는 것인데 4040D0주소에 flag값을 지정하였는데 이를 출력하지 않았다는게 문제입니다.

이 부분을 출력하려고 해서 이부분을 출력함수인 _puts을 시도하려고 하였으나 이게 아니여도 "Can you see that" 부분에서 똑같은 레지스터들을 덮어쓰더라구요

그래서 저는 rax, rdi 덮어쓰여지기 전에 기존에 "Can you see that"을 출력하는 쪽으로 넘어가 flag를 출력하게 변경하면 문제가 해결될거라 생각했습니다.
<img width="900" height="532" alt="image" src="https://github.com/user-attachments/assets/334628d0-6708-43c6-86d8-764f00a1e530" />

아직 IDA patch가 익숙치 않아 많이 틀렸었네요..

소스코드는 아래와 같이 수정되어 flag 값을 갖고 바로 출력하도록 patch했습니다.

<img width="985" height="516" alt="image" src="https://github.com/user-attachments/assets/b655400e-b1aa-499b-97df-428e474311a3" />

최종 결과값은 아래와 같습니다.

<img width="736" height="276" alt="image" src="https://github.com/user-attachments/assets/bf742ae8-3349-43fc-9bc0-a4e475f3a3a6" />
