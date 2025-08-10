
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
