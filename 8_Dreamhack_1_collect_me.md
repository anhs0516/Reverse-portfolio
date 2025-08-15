### 문제

<img width="810" height="287" alt="image" src="https://github.com/user-attachments/assets/cdadbec7-1056-4b94-a95a-737f96520024" />

### 문제풀이

역시 이번에도 ELF 파일입니다.... GDB 사용법을 더 익히면 뭔가 더 쉽게 문제들을 풀 수 있을 것 같은데 아직 많이 부족합니다..

문제를 이해해보겠습니다. 

제가 생각한 포인트 

1. 함수 928개가 있다

2. 각 함수당 char 지역변수 1개

3. 이를 순차적으로 추출하여 문자열을 만들면 문제 해결이라고 하네요..

우선 IDA에서 Ctrl + F12를 통해 문자열검색을 했습니다.

눈에 띄는 것은 Program Description과 To Do 이 근처들이 눈에 띕니다

<img width="653" height="237" alt="image" src="https://github.com/user-attachments/assets/e9d1bec1-2d49-4845-83ea-1c2cfc22e810" />


IDA 좌측편엔 928개의 함수들이 보입니다..

<img width="491" height="748" alt="image" src="https://github.com/user-attachments/assets/5fc3457c-d63c-4b0c-9df0-0e34bf4dfa23" />

일부 함수를 소스코드로 보니 비어있네요?

<img width="144" height="78" alt="image" src="https://github.com/user-attachments/assets/b9e05181-6154-46f9-9c1a-01bc0f30fb42" />

메인 함수를 그래프형식으로 보게되면 아래와 같습니다.

<img width="545" height="728" alt="image" src="https://github.com/user-attachments/assets/2bfe57b6-550e-43dd-ab18-7514ebd1d66b" />


이를 소스코드로 보면 각 문자열들을 출력하는거말고는 특별히 보이지는 않는 것 같습니다. 각 함수의 지역변수 1개씩을 찾아야하는데 이를 어떻게 찾아야할지부터 확인해봐야겠네요

<img width="532" height="204" alt="image" src="https://github.com/user-attachments/assets/064f5c10-8a87-4cfe-85b5-756977db4f22" />

함수들을 Hex-view를 통해 보았으나, IDA와 GDB만으로는 노가다가 필요해보입니다.

<img width="620" height="979" alt="image" src="https://github.com/user-attachments/assets/6f701e7e-2438-4f3a-ae4c-0a47bf10702b" />

Dreamhack 같은 페이지 내 참조하라고 되어있으니 해당 방법을 우선 익혀보고 진행하도록 하겠습니다.

<img width="1209" height="150" alt="image" src="https://github.com/user-attachments/assets/ce81d555-1ec0-42f5-8191-da2418034d91" />


### Ghidra 

기드라란? 미국 국가 안보국(NSA)에서 만들어 오픈 소스로 공개한 역어셈블리어 프레임워크입니다

자세한 내용은 notes쪽에 정리하고 돌아오도록 하겠습니다.
