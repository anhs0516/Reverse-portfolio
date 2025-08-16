### 문제

<img width="1185" height="553" alt="image" src="https://github.com/user-attachments/assets/ab94499f-117d-4a14-9f72-cab91e13673f" />


### 문제풀이

문제 내용을 읽어보면 입력값을 검증하여 맞으면 correct, 틀리면 wrong을 출력하는 것이니 

우선 문자열 위주로 봐야할 것 같습니다.

IDA를 이용하여 Ctrl+F12 문자열 검색하여 correct 존재를 확인할 수 있었습니다.

<img width="594" height="329" alt="image" src="https://github.com/user-attachments/assets/3358a62d-3267-4d25-ba61-a035a759ad56" />

문제 해결할 방법으로는 크게 2가지가 있어보입니다.
1. cmp 비교값을 건드려 넘기기
2. jmp 조건들을 수정하여 넘기기

크게 cmp와 jmp 들이 사용되는 부분들을 Graph view를 통해 보겠습니다. 

<img width="898" height="961" alt="image" src="https://github.com/user-attachments/assets/4478578c-5d70-44fd-aeba-64c8c206ebb0" />
<img width="862" height="389" alt="image" src="https://github.com/user-attachments/assets/e9b44f04-b1a0-4d90-97f0-680e2aded27c" />
