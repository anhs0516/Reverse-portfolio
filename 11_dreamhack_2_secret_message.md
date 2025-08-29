### 문제

<img width="789" height="168" alt="image" src="https://github.com/user-attachments/assets/07dd625b-45cc-4e17-acae-d076a46cd4e7" />


링크 : https://dreamhack.io/wargame/challenges/235

### 파일 분석

이번 문제에서는 다른 기존 문제들과는 달리 파일 3개로 되어있네요

<img width="648" height="63" alt="image" src="https://github.com/user-attachments/assets/e89d43ec-cd46-4d74-ab95-4324e73cacc2" />


1. PEview로 봤을 때, ELF 파일

<img width="1442" height="258" alt="image" src="https://github.com/user-attachments/assets/3bf037ee-e915-4f6d-8669-6724083edd9f" />

2. 파이썬 코드로 작성되어 있는 파이썬 파일

<img width="982" height="473" alt="image" src="https://github.com/user-attachments/assets/637e008b-ea09-457b-92a1-bd0a7144ce8f" />


3.enc 확장자

ENC 파일은 특정 소프트웨어에서 생성한 인코딩 또는 암호화된 파일을 나타내는 확장자



### 간단한 코드 분석

문제와 연관지어 생각해보면, 어떠한 알고리즘으로 이미지 파일을 인코딩하여 enc 확장자로 만들었다.

복호화시켜 원본 이미지를 알아낸 후 이를 imageviewer.py 코드를 이용해서 이미지 보면 문제 해결될 것으로 보입니다.


아래 코드는 간단히 보면 바이너리(raw) 파일을 읽어서 가로 500, 세로 50 크기를 OS 기본 이미지 뷰어로 열어서 화면에 띄우는 것 같습니다. prob(ELF) 파일 분석과 enc 파일을 푸는게 핵심일 것 같아요 
```code

#imageviewer.py


import sys

# pip install pillow
from PIL import Image

if len(sys.argv) != 2:
    print(f"python {sys.argv[0]} <secretMessage.raw>")
else:
    with open(sys.argv[1], "rb") as f:
        output = f.read()
        Image.frombytes("1", (500, 50), output).show()

```



### 문제 풀이

* prob 문자열

<img width="635" height="309" alt="image" src="https://github.com/user-attachments/assets/337143b6-5f25-4e65-ae5d-68df71e0c4b6" />


* prob main 그래프 view

<img width="330" height="609" alt="image" src="https://github.com/user-attachments/assets/c2fab0cb-1018-4c71-9c65-889cb9364cf3" />


* prob main source code

<img width="513" height="239" alt="image" src="https://github.com/user-attachments/assets/8512fe32-2062-49aa-a1c6-610cba3227ee" />

위 code에서 fopen을 통해 secretMessage.raw와 secretMessage.enc 파일들을 열어 "sub_7FA" 함수에 들어가네요 

* sub_7FA

이 함수가 어떻게 흘러가는지 확인하는게 매우 중요할 것으로 보입니다.

<img width="437" height="856" alt="image" src="https://github.com/user-attachments/assets/31db9eaa-75e2-4c54-b88f-4b60ca77e648" />


여기까지 봤을 때 확인할 수 있는 것은 prob 파일은 인코딩해주는 프로그램, secretMessage.enc는 prob로 인코딩하여 생성된 파일, imageviewer.py는 원본 파일(secretMessage.raw)를 실행시켜 이미지를 보는 파이썬 코드인것으로 보입니다.

그렇다면 sub_7FA를 역연산하여 즉, secretMessage.enc 파일을 디코딩하여 secretMessage.raw 원본 이미지 파일을 구해야할 것으로 보입니다. 



그렇다면 가장 중요한 sub_7FA 코드분석을 해보겠습니다.
```
13줄 : a1 파일에서 하나의 문자(1byte) 읽어서 c라는 변수에 저장
14~15줄 : c가 -1(EOF : End Of File 더이상 읽을 데이터가 없는 상태)이면 반복문 종료
16줄 : c(읽은 글자)를 a2파일에 저장
1. 17줄~ : c(읽은 글자)가 v5랑 같다면 조건문 실행
    19줄 : v3는 0으로 초기화
    22줄 : a1 파일에서 다음 글자 가져와서 c에 저장 즉, c는 다음글자가 됨
    23~24줄 : 다음글자가 EOF면 break
    25~31줄 : c(다음글자)가 v5가 아니면? a2파일에 v3, c(다음글자) 저장, v5가 다음글자가 됨
    32~40줄 : v3가 1증가하고 이게 255이면?, a2파일에 255값 저장 후 v5 = -1
     (이 때 v3는 unsigned_int이므로 -가 없는 8byte로 2의8승인 0~255값, -1이면 255)
     (1이 먼저증가함 즉, 같은숫자가 나오게되면 v3가 증가함, count 값)
2. 40~49줄 : c(읽은글자)가 v5랑 다르다면 조건문 실행
    v5 = c(읽은글자)
    c(읽은글자)가 -1이면, a2파일에 v3값 저장


```

<img width="437" height="856" alt="image" src="https://github.com/user-attachments/assets/31db9eaa-75e2-4c54-b88f-4b60ca77e648" />


이렇게 코드 분석을 진행해보았을 때, 유추할 수 있는 것은 뒤에 반복되는 글자가 있으면 몇번 반복되는지에 대해 인코딩하는 형식일 것으로 판단됩니다.

그래서 예시를 가지고 한 번 어떻게 나뉘게 될지 적어보겠습니다.

```
예시)  원본 File 내용(a1) : abcccddeee
```

위 내용일 때 a2 파일은 어떻게 인코딩될까요?

a2 : abcc1dd0ee1


위 답을 다시 abcccddeee로 나오게끔 하면 원본 이미지파일을 찾을 수 있을 것으로 예상됩니다.


파이썬 코드로 한번 작성해보겠습니다.

``` code




```







### 느낀점



