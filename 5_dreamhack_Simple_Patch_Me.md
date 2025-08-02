

### 문제
<img width="1264" height="288" alt="image" src="https://github.com/user-attachments/assets/5eb5d0a0-d8ac-4c38-a6ad-fba333ea82e7" />


### 문제풀이
실제시간 365일이 흘러가면 플래그가 나오는것으로 보아 간단하게는... 시간을 건드리면 해결될 것 같네요

이 파일도 ELF 파일로 IDA에서 확인 후 리눅스를 통해 열어보겠습니다.


IDA에서 문자열 검색을 통해 확인해본 값으로 
1 시간,  1일 passed 뒤 1년이 지나면 Great 및 flag 정보가 나오는 걸로 확인되는 문자열이 확인됩니다.

<img width="679" height="288" alt="image" src="https://github.com/user-attachments/assets/61ccd857-3e36-4a24-a067-46896b3488a1" />

main 내 Graph로 볼 때 아래처럼 이루어져있네요 

생각해볼 것
1. cmp 비교값을 줄이기
2. jmp 를 변경하기

gdb를 확인할 때 위 2가지를 한 번 시도해보겠습니다.

<img width="1129" height="953" alt="image" src="https://github.com/user-attachments/assets/1307513d-32a7-402e-afdf-e432f8f7e2bf" />

우선 해당 code를 보겠습니다.

<img width="559" height="510" alt="image" src="https://github.com/user-attachments/assets/a7668437-2486-43a8-9a47-9c0ffd991e52" />

sub_401196 부분에서 flag 값 나오는 것이 보여 sub_401196 도 확인해보겠습니다.

<img width="474" height="415" alt="image" src="https://github.com/user-attachments/assets/26adfafd-3f0b-4a5d-b7be-856cd421c396" />
