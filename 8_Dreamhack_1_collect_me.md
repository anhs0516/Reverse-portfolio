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

파이썬을 pwn 이용하여 해당 func의 1byte씩 가져와보도록 하겠습니다.

``` code

# pip install pwn

from pwn import *
import re

elf = ELF('./collect_me') # 파일 불러오기
s = b""  # byte 형식
for i in range(928):
    s += elf.read(elf.sym[f'func_{i}']+11, 1) # s += elf.read(elf.sym['func_'+str(i)]+11, 1) func_0~ 927까지의 주소 내 11byte 떨어진 위치에서 1바이트 읽어와서 문자열 붙이기
s = s.decode() # 문자열 형식으로 변환
print(s)
print(re.search(r"DH\{.*?\}", s).group()) 

```
func의 지역변수는 11byte 떨어진 위치에서 1byte가 있었습니다. 이를 모두 불러오면 아래와 같은 단어들로 이루어져 정답을 찾을 수 있네요

점점 생각할 시간이 많아져 오래 걸리고 어려워지고 있습니다...ㅋㅋ..

<img width="1834" height="112" alt="image" src="https://github.com/user-attachments/assets/d1a7273f-7249-4eda-a02d-dfb607e0d673" />

