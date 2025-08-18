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

크게 cmp와 jmp 들이 사용되는 부분들을 Graph view를 본 후 code 확인 및 GDB를 이용해서 프로그램 실행하여 어떤 식으로 흘러가는지 확인해보겠습니다.

* Graph View

<img width="898" height="961" alt="image" src="https://github.com/user-attachments/assets/4478578c-5d70-44fd-aeba-64c8c206ebb0" />
<img width="862" height="389" alt="image" src="https://github.com/user-attachments/assets/e9b44f04-b1a0-4d90-97f0-680e2aded27c" />

* Source Code

소스코드를 확인해보았을 때 글자길이 32자를 맞추어 if문을 들어가고 cmp 비교문을 맞추면 문제가 해결될 것으로 보입니다.

<img width="505" height="503" alt="image" src="https://github.com/user-attachments/assets/6c95d9c3-97d6-43d0-b895-e2d851f58ba1" />



* GDB를 이용한 프로그램 실행

프로그램 실행 후 입력값을 맞추면 flag 값이 보여질 것으로 보입니다.

<img width="749" height="129" alt="image" src="https://github.com/user-attachments/assets/5777fe2a-20f4-4e31-930b-d9c03b874044" />

위 source code에서 알아낸 방법으로 일단 32글자를 입력값에 입력해 보았습니다.

그랬더니 위와는 다른 결과값이 보여지는 걸 확인할 수 있습니다. IDA 패치를 진행해보겠습니다.

<img width="763" height="127" alt="image" src="https://github.com/user-attachments/assets/03e0290a-0de0-41db-9710-6fcb5d3e9145" />

패치 후 무작정 correct를 띄우게끔 했습니다만... 위에 그래프, 소스코드 등을 살펴보니 correct만 띄우는거지 이걸 띄운다고 해서 flag 값이 나오는게 아니였습니다. 

<img width="774" height="98" alt="image" src="https://github.com/user-attachments/assets/460b5f54-f64a-4fdf-b6c1-1dd82b556648" />

다시 말해 위에서 제가 말한 2가지 방법 모두가 잘못되었고 이는 32자릿수의 글자를 알아내야 하는 즉, 역연산을 해야하는 것 같습니다.

역연산을 위해 코드를 좀 더 자세히 보겠습니다.
