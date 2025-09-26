
## 문제
<img width="1216" height="313" alt="image" src="https://github.com/user-attachments/assets/ec4fa81e-ea1a-4b57-bc21-7431ab456ebe" />

출처 : https://dreamhack.io/wargame/challenges/91




## 분석
해당 문제의 압축파일을 풀어보니 3개 파일이 나왔습니다. 

<img width="1617" height="138" alt="image" src="https://github.com/user-attachments/assets/b61c26a2-a830-48e3-80f7-05929fa13189" />

* out.txt파일

암호화된 flag 값을 복호화하는 문제로 아래 두 값들이 사용될 것으로 보입니다.

<img width="1379" height="419" alt="image" src="https://github.com/user-attachments/assets/1268c200-d0a7-4717-80d0-478a106911d9" />

* out.bin 파일
  
해당 파일은 bin 파일이네요

<img width="1329" height="49" alt="image" src="https://github.com/user-attachments/assets/529bdf73-19a1-4c46-8196-e4aff3e3d67e" />


* public 파일

해당 파일은 ELF 매직넘버가 보이는걸로 봐서 ELF 파일인 것 같습니다 이 파일을 먼저 분석해봐야할 것 같아요 
<img width="1763" height="572" alt="image" src="https://github.com/user-attachments/assets/641e26ca-21c8-43d6-ac1a-39ad0a56b81d" />


IDA로 열어 문자열들을 보니 아래와 같습니다.

<img width="464" height="451" alt="image" src="https://github.com/user-attachments/assets/fdbbfdc5-6df9-44ae-a550-4e58c79490a2" />

이번 문제는 그래프도 매우 길고 코드도 기네요... 어려운 문제같아요 오랜만에 하니 처음보는거같네요 ㅠ



## 문제풀이 



## 정리




## 느낀점
