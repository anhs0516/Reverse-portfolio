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



### 코드 분석

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



### 느낀점



