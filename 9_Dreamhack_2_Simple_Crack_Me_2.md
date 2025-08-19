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

처음 32자리를 비교하는거는 알게 되었고 아래 각 함수마다 연산식이 있을겁니다 확인해보겠습니다.

* 4011EF
<img width="391" height="243" alt="image" src="https://github.com/user-attachments/assets/638b365c-b5fc-4d00-a12a-7ed21e392590" />

XOR 

* 401263
<img width="517" height="212" alt="image" src="https://github.com/user-attachments/assets/e3ecf7c6-b57d-40c1-8e1e-37cfefd71029" />

ADD

* 4012B0

<img width="421" height="208" alt="image" src="https://github.com/user-attachments/assets/267289a5-7347-4947-bd9a-ade624d3afaa" />

SUB

를 뜻합니다.

이 위에 식들을 표현하자면 아래와 같습니다 



이후 collect를 띄우기 전 비교 대상인 if 문을 확인하여 어떤 문자열 값과 비교하는지 확인해보겠습니다.

<img width="409" height="511" alt="image" src="https://github.com/user-attachments/assets/d3ffb329-8e22-4817-894d-9aba5d28769e" />

if문에서 "s2"를 더블 클릭하면 402008 주소값이 나오는 것을 확인할 수 있습니다.

<img width="1078" height="372" alt="image" src="https://github.com/user-attachments/assets/b6edf8cf-5a82-439e-89b9-69d7fb4f2d5d" />

402008 주소를 따라가보면 아래와 같은 데이터들이 나오네요 이를 토대로 역연산을 진행하면 flag 값을 유추할 수 있을 것 같습니다.

<img width="878" height="502" alt="image" src="https://github.com/user-attachments/assets/81f74717-336b-4459-a08c-4252cb30fcc0" />


역연산이 이루어지면 ADD는 SUB로, SUB는 ADD로 진행되기 때문에 진행흐름은 아래와 같습니다.

* 연산 (역연산 전 기존 흐름)

XOR

ADD

SUB

XOR

SUB

ADD

XOR


* 역연산 (flag값을 유추하기 위한 역연산)

XOR

SUB

ADD

XOR

ADD

SUB

XOR

위에 순서대로 진행됩니다.

이를 역연산하기 위한 파이썬 코드를 작성해보겠습니다.

```code
data = [ 0xf8, 0xe0, 0xe6, 0x9e, 0x7f, 0x32, 0x68, 0x31, 0x05, 0xdc, 0xa1, 0xaa, 0xaa, 0x09, 0xb3, 0xd8, 0x41, 0xf0, 0x36, 0x8c, 0xce, 0xc7, 0xac, 0x66, 0x91, 0x4c, 0x32, 0xff, 0x05, 0xe0, 0xd9, 0x91]
xor1 = [ 0xde, 0xad, 0xbe, 0xef]
xor2 = [ 0xef, 0xbe, 0xad, 0xde]
xor3 = [ 0x11, 0x33, 0x55, 0x77, 0x99, 0xbb, 0xdd]

def XORParam(lists, key):
    for i in range(0x20):
        lists[i] ^= key[i % len(key)]

def ADDParam(lists, key):
    for i in range(0x20):
        lists[i] = (lists[i] + key) & 0xFF  # +/- 연산 후 1바이트 범위(0~255)로 유지하려고 & 0xff를 사용 C에서 unsigned char x 와 동일
        
def SUBParam(lists, key):
    for i in range(0x20):
        lists[i] = (lists[i] - key) & 0xFF # +/- 연산 후 1바이트 범위(0~255)로 유지하려고 & 0xff를 사용 C에서 unsigned char x 와 동일

# 역연산으로 기존 진행흐름에서 반대로 진행하며 ADD -> SUB , SUB -> ADD로 진행 단, XOR 는 거꾸로해도 XOR
lists = data
XORParam(lists, xor3)
SUBParam(lists, 0xf3)
ADDParam(lists, 0x4d)
XORParam(lists, xor2)
ADDParam(lists, 0x5a)
SUBParam(lists, 0x1f)
XORParam(lists, xor1)

assert len(lists) == 0x20
print('DH{%s}' %bytes(lists).decode()) # bytes(lists)는 0~255 범위의 정수만 허용


```

<img width="338" height="30" alt="image" src="https://github.com/user-attachments/assets/d34689f0-3773-449d-8b6c-7ddb1280e6ca" />



--------------------------

### 느낀점

문제 해결에 있어서 플래그값이 출력되는지 안되는지부터 확인할 것 
IDA Source Code를 보며 역연산 과정을 정확히 분석하여 파이썬 코드로 짜는데에 익숙해져 시간 단축이 필요
