
## 문제
<img width="1216" height="313" alt="image" src="https://github.com/user-attachments/assets/ec4fa81e-ea1a-4b57-bc21-7431ab456ebe" />

출처 : https://dreamhack.io/wargame/challenges/91




## 분석
해당 문제의 압축파일을 풀어보니 3개 파일이 나왔습니다. 

<img width="1617" height="138" alt="image" src="https://github.com/user-attachments/assets/b61c26a2-a830-48e3-80f7-05929fa13189" />

* out.txt파일

암호화된 flag 값을 복호화하는 문제로 아래 두 값들이 사용될 것으로 보입니다.

<img width="688" height="198" alt="image" src="https://github.com/user-attachments/assets/e3b9dc90-8570-4856-b781-f5d92da50cfd" />

* out.bin 파일
  
해당 파일은 bin 파일이네요

이 파일은 이미 암호화가 되어 있나봐요 특정 단어가 한눈에 보이지 않네요

<img width="1329" height="49" alt="image" src="https://github.com/user-attachments/assets/529bdf73-19a1-4c46-8196-e4aff3e3d67e" />


* public 파일

해당 파일은 ELF 매직넘버가 보이는걸로 봐서 ELF 파일인 것 같습니다 이 파일을 먼저 분석해봐야할 것 같아요 
<img width="1763" height="572" alt="image" src="https://github.com/user-attachments/assets/641e26ca-21c8-43d6-ac1a-39ad0a56b81d" />


IDA로 열어 문자열들을 보니 아래와 같습니다.

<img width="464" height="451" alt="image" src="https://github.com/user-attachments/assets/fdbbfdc5-6df9-44ae-a550-4e58c79490a2" />

이번 문제는 그래프도 매우 길고 코드도 기네요... 어려운 문제같아요 오랜만에 하니 처음보는거같네요 ㅠ

Main 함수부터 차근차근 코드분석 해보겠습니다.

* Main 함수

```main

v46 = __readfsqword(0x28u);
  v10 = 0LL;
### n.txt. 파일을 열음
  stream = fopen("n.txt", "r");
### v5, v6 값을 받아옴
  __isoc99_fscanf(stream, "%llu %llu", &v5, &v6);
  fclose(stream);
  while ( v10 <= 0xFCFCFCFC )
  {
### sub_12FE 를 통해 v10 값 생성
    v8 = sub_12FE(v5);
    v9 = sub_12FE(v8 + 128);
    v10 = v9 * v8;
  }
### n1 = v10 출력 이는 v5 값을 바탕으로 생성됨
  printf("n1 = %llu\n", v10);
  v11 = v6;
  v13 = (v8 - 1) * (v9 - 1);
  while ( v11 < v13 && sub_1249(v11, v13) != 1 )
    ++v11;
### n2 = v11 출력 이는 v6 값을 바탕으로 생성됨 
  printf("n2 = %llu\n", v11);
  *(_QWORD *)s = 0LL;

  v15 ~ v45 = 0LL;
### flag.txt 파일 읽어 s 값을 가져옴 이는 길이가 3이 아니여야 함
  stream = fopen("flag.txt", "rb");
  __isoc99_fscanf(stream, "%255s", s);
  fclose(stream);
  if ( (strlen(s) & 3) != 0 )
  {
    puts("invalid length!");
    exit(-1);
  }
### out.bin에 저장하는데 sub_1289를 통해 얻은 값을 저장함
  stream = fopen("out.bin", "wb");
  for ( i = 0; i < strlen(s) >> 2; ++i )
  {
    ptr = sub_1289(*(unsigned int *)&s[4 * i], v11, v10);
    fwrite(&ptr, 8uLL, 1uLL, stream);
  }
  fclose(stream);
  return 0LL;
}

```

* sub_1289

```
__int64 __fastcall sub_1289(__int64 a1, unsigned __int64 a2, unsigned __int64 a3)
{
  __int64 v4; // [rsp+20h] [rbp-10h]
  unsigned __int64 v5; // [rsp+20h] [rbp-10h]
  unsigned __int64 i; // [rsp+28h] [rbp-8h]

  if ( !a3 )
    exit(-1);
  v4 = 1LL;
  for ( i = 0LL; i < a2; ++i )
  {
    v5 = a1 * v4;
    v4 = v5 % a3;
### 쉽게작성하면 위 내용은 아래와 같아요 아래 내용을 a2번 반복
v4 = a1 * v4 % a3;
  }
  return v4;
}

```


* sub_12FE

```
unsigned __int64 __fastcall sub_12FE(unsigned __int64 a1)
{
  unsigned __int64 i; // [rsp+10h] [rbp-8h]

  while ( 1 )
  {
    for ( i = 2LL; a1 >= i * i && a1 % i; ++i )
      ;
    if ( a1 < i * i )
      break;
    ++a1;
  }
  return a1;
}

``` 



## 문제풀이 

우선 outbin 내용에 뭐가 들어갔는지 읽어보겠습니다. 

암호화를 거쳐서 아래값들로 저장되었을 것 같습니다. 이 결과값을 복호화시키면 flag값이 보일 것 같습니다.

```code
with open("D:\\2025\\Reversing_C\\dreamhack_reverse\\2_public\\out.bin", "rb") as f:
    data = f.read()
    print(data)

결과값
b'@\xe1\xdc\xd4\x00\x00\x00\x00\xe2\xdf\x83\xa1\x00\x00\x00\x00\x06\xe3c\xc3\x00\x00\x00\x00h\xe2\xd2\xf9\x00\x00\x00\x00\t$\x1a\xc4\x00\x00\x00\x00\xfb\xc0\x9b*\x00\x00\x00\x00\xb5"N\x9a\x00\x00\x00\x00\x9a\xef8}\x00\x00\x00\x00\x9f\x92_\xb1\x00\x00\x00\x00\xef{\xd6\x9e\x00\x00\x00\x00\xe7\xea\xcd\x99\x00\x00\x00\x00'
```

우선 문제풀이 중에 도저히 잘 모르겠어서 Dreamhack의 댓글을 보았는데 "RSA 알면 ㄹㅇ 쉬움" 이라는 답변을 보고 아래 코드를 보니 (p-1)*(q-1)이 바로 보이네요 

이걸보니 RSA 암호화가 걸려있는 것 같아요 이제 문제의 흐름이 조금이나마 확인되네요

```
 while ( v10 <= 0xFCFCFCFC )
  {
### sub_12FE 를 통해 v10 값 생성
    v8 = sub_12FE(v5);
    v9 = sub_12FE(v8 + 128);
    v10 = v9 * v8;
  }
### n1 = v10 출력 이는 v5 값을 바탕으로 생성됨
  printf("n1 = %llu\n", v10);
  v11 = v6;
  v13 = (v8 - 1) * (v9 - 1);
  while ( v11 < v13 && sub_1249(v11, v13) != 1 )
    ++v11;
### n2 = v11 출력 이는 v6 값을 바탕으로 생성됨 
  printf("n2 = %llu\n", v11);
  *(_QWORD *)s = 0LL;

  v15 ~ v45 = 0LL;
```

문제풀이 방식
1. 소수 p, q 찾기
2. n1 = pq
3. 오일러 파이함수 phi 값 구하기 phi(n1) = (p-1)(q-1)
4. 1 < e < phi(n1) 을 만족하는 phi(n1)와 서로소인 e(n2)
5. ed mod phi(n1) = 1을 만족하는 d 찾기
6. 평문 x의 암호화는 y = x^e % n1
7. 암호문 y의 복호화는 x = y^d % n1

공개키는 N 과 e 값을 주기 때문에 아래로 예상해볼게요
n1 = 4271010253 (N)
n2 = 201326609 (e)

n1 값을 통해 두 개의 소수 값을 찾아봐야할 것으로 보입니다. 근데 값이 너무큰데요....ㅋㅋ
n1을 소인수분해하여 2개의 소수값을 찾아볼게요
``` 소인수분해 code
from sympy import factorint

print(factorint(4271010253))


```
위 결과값에 따라 소수 p, q 값을 확인했습니다
p = 
q = 



## 정리




## 느낀점
