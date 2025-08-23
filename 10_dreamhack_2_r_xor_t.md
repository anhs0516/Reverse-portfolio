### 문제

<img width="1049" height="298" alt="image" src="https://github.com/user-attachments/assets/d1aaf639-e4f9-44c5-8cc6-141bafc6aaf6" />


### 문제풀이

ELF 파일로 IDA로 먼저 본 후 GDB 실행시켜보겠습니다. 

IDA에서 main에 대해 Graph는 아래와 같습니다.

<img width="751" height="803" alt="image" src="https://github.com/user-attachments/assets/3938aedd-e5f3-409b-a3c4-f296aaf9f10f" />

<img width="853" height="960" alt="image" src="https://github.com/user-attachments/assets/72bf25f7-a009-4e7d-8e08-5e8cbf3c586a" />

<img width="852" height="443" alt="image" src="https://github.com/user-attachments/assets/b056eea8-cbda-4db5-a6c5-73eb9c4de559" />


Source Code 로 보면 아래처럼 보이고 

먼저 하나씩 살펴보면 입력값을 입력 후 64글자를 비교하는 걸로 봐서 이번엔 64자리나 입력해봐야 되나봐요 ㅠ 

그리고 3개의 반복문을 지나 문자열 비교하면 flag 값을 보여주나봅니다.

<img width="766" height="405" alt="image" src="https://github.com/user-attachments/assets/83bade8b-ce46-42c2-b13e-2dba77918a7f" />

이번 문제는 flag 값을 결과에 출력하지만 그렇다고 JMP를 이용하여 바로 넘어간다하면 Input 내용이 보여지기에 내가 입력한대로만 나올 것으로 보입니다.

64자 입력값과 비교 이 두가지를 JMP를 이용해보았지만 제가 생각한대로 정답이 아닌 제가 입력한 값이 그대로 flag값으로 확인됩니다.

<img width="760" height="174" alt="image" src="https://github.com/user-attachments/assets/3bcb4d04-135f-4407-993d-acdbbe29ae7e" />

즉, for문을 지나 모두 연산이 되어야 정답이 나올 수 있기 때문에 각 for문에 대해 역연산이 필요해보여 파이썬을 이용하여 진행해보겠습니다.

이 문자열을 이용하여 역연산을 진행하면 될 것 같네요 

<img width="467" height="32" alt="image" src="https://github.com/user-attachments/assets/64e214ab-7a01-460e-91ba-772cd0c0ba50" />

위 문자열을 HEX 값으로 변환하면 아래와 같은 값이네요

<img width="645" height="98" alt="image" src="https://github.com/user-attachments/assets/911dfc6d-043b-458d-a09a-b6e87e45dd25" />

```

43 40 71 70 6c 3d 3d 42 70 70 6c 40 3c 3d 70 47
3c 3e 40 6c 3e 40 42 6c 73 70 3c 40 6c 40 41 41
72 71 6d 47 72 3d 42 40 41 3e 71 40 40 42 3d 47
45 73 6d 43 40 41 72 42 6d 41 47 6c 41 3d 40 71

```

하지만 이렇게 매번 변환해서 사용하기엔 힘들 수 있으니 파이썬 코드를 작성할 때 바이트 문자열로 변환하여 사용하도록 하겠습니다.
```
result2 = b"C@qpl==Bppl@<=pG<>@l>@Blsp<@l@AArqmGr=B@A>q@@B=GEsmC@ArBmAGlA=@q"
result2 = list(result2)


```


아래 코드는 수정중에 있습니다. 

``` code
result2 = [ 0x43, 0x40,  0x71,  0x70,  0x6c, 0x3d, 0x3d, 0x42, 0x70, 0x70, 0x6c, 0x40, 0x3c, 0x3d, 0x70, 0x47, 0x3c, 0x3e, 0x40, 0x6c, 0x3e, 0x40, 0x42, 0x6c, 0x73, 0x70, 0x3c, 0x40, 0x6c, 0x40 , 0x41 , 0x41 , 0x72 , 0x71 , 0x6d , 0x47 , 0x72 , 0x3d , 0x42 , 0x40 , 0x41 , 0x3e , 0x71 , 0x40 , 0x40 , 0x42 , 0x3d , 0x47, 0x45 , 0x73 , 0x6d , 0x43 , 0x40 , 0x41 , 0x72 , 0x42 , 0x6d , 0x41 , 0x47 , 0x6c , 0x41 , 0x3d , 0x40 , 0x71]

# XOR
def for3(lists):  
    for i in range(64):
         lists[i] ^= 3

# 뒤집기         
def for2(lists):
    for i in range(61):
        lists[i+1] = lists[63-(i+1)]
        
# -13 후 0x7F AND하여 0~127로 표현하기
def for1(lists):
    lists[0] = 0x40
    for i in range(64):
        lists[i] = (lists[i]-13) & 0x7f
    

# 역연산으로 기존 진행흐름에서 반대로 진행하며 ADD -> SUB , SUB -> ADD로 진행 단, XOR 는 거꾸로해도 XOR
lists = result2
print(len(lists))

for3(lists)
for2(lists)
for1(lists)

assert len(lists) == 64
print(len(lists))
print('DH{%s}' %bytes(lists).decode()) # bytes(lists)는 0~255 범위의 정수만 허용

```

