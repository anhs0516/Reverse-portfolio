
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

main 함수 중간에 flag.txt을 열어 값을 저장하는데 v10과 v10값을 encode 함수를 통해 변환 후 out.bin에 저장

문제 해결을 위해선 암호화되어 나오는 값인 out.txt에 주어진 n1과 n2 값을 역으로 거슬러 올라가 즉, 이를 복호화하여 flag 값을 찾아내는게 문제푸는 방법인 것 같습니다.



## 정리




## 느낀점
