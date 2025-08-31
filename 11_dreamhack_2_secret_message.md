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

```
답) a2 : a b c c \x01 d d \x00 e e \x01
```


위 답을 다시 abcccddeee로 나오게끔 하면 원본 이미지파일을 찾을 수 있을 것으로 예상됩니다.


파이썬 코드로 한번 작성해보겠습니다.

``` code
def decoding(enc, raw):
    result = bytearray()
    with open(enc, 'rb') as a1, open(raw, 'wb') as a2:
        data = a1.read() # 파일 전체 읽기
        i = 0
        prev = None

        while i < len(data): # data 길이만큼 반복
            curr = data[i:i+1]  # 현재 바이트 (bytes 타입, 길이 1) data[i]로하면 A를 ASCII 65로 가져옴
            result += curr  # 항상 현재 바이트는 저장

            if prev == curr: # 이전과 같은 바이트가 2번나오면 
                i += 1 # 인덱스
                if i < len(data):
                    repeat_count = data[i] # 반복횟수 
                    print(f"반복 횟수: {repeat_count}")
                    result += prev * repeat_count  # 이전 바이트 숫자만큼 반복
            else:
                prev = curr
            i += 1

        print(result)
        print(len(result))
        a2.write(result)

decoding('secretMessage.enc', 'secretMessage.raw')




```

입력값 : python imageviewer.py secretMessage.raw

### 결과값

문제풀이 시 다운로드 받은 imageviewer.py 파일을 실행시키면 디코딩되어 생긴 "secretMessage.raw" 파일이 이미지 파일로 열리게 됩니다.

<img width="1761" height="943" alt="image" src="https://github.com/user-attachments/assets/acb5eb03-49f6-4569-b849-6fc6673af59c" />

그럼 문제해결이 됩니다  끝~

### 느낀점

1. 우선 디코딩과정에서 시간이 꽤 소요되었습니다. 아직 많이 부족하다는 뜻 같습니다. 디코딩하는 실력을 향상시켜야할 것 같아요.. 코딩을 이해하는 시간과 이를 역연산으로 만드는 코드 작성 이 두 가지 실력을 키워야할 것 같습니다.
2. 1번과 연관된 내용인데 잘못된 방향으로 끝까지 가다보니 시간이 많이 소요됐습니다. 제가 처음 디코딩한대로 하면 877 byte 밖에 안됐어요 이것또한 하나의 힌트가 될 수 있었는데 이 점을 놓쳤습니다.
   imageviewer.py 코드를 보면 Image.frombytes(mode, size, data)로 구성되어 1비트 흑백모드, 가로 500px, 세로 50px 이므로 전체 이미지크기는 500 * 50 = 25,000 픽셀, 필요한 바이트 수는 25,000 / 8bit (1byte)해서 3,125 바이트가 나왔어야 합니다. 877바이트가 나온거에서 잘못된 거라고 인지하고 다시 디코딩을 시도했어야 했는데 주어진 힌트 사용에 부족한 점이 많았네요..
   
<img width="488" height="41" alt="image" src="https://github.com/user-attachments/assets/022620ed-4c8a-4a10-a58f-0da07f8a0d20" />


