
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

return 0이 아니라 인자 하나를 return 하게되면 문제가 해결될 수 있을 것 같다는 생각갖고 찾아볼게요
