

# 문제

<img width="815" height="205" alt="image" src="https://github.com/user-attachments/assets/55957734-eaf7-4e25-8d8f-bdb090fb779e" />


문제 주소 : https://dreamhack.io/wargame/challenges/961

# 분석

우선 PEView를 통해 ELF 파일임을 확인했습니다.

<img width="1449" height="284" alt="image" src="https://github.com/user-attachments/assets/dd90c541-4aaa-4c12-8734-657e2860a697" />

IDA에서 Graph View를 통해 보았을 때, 어떠한 값을 입력하고 조건문에 따라 flag, good, try again이 보일 것 같네여

<img width="877" height="954" alt="image" src="https://github.com/user-attachments/assets/5f6a2d0e-9f21-451c-b2a3-caa99a1798f1" />

Code로는 아래와 같습니다.

<img width="489" height="351" alt="image" src="https://github.com/user-attachments/assets/73cff908-bc19-4b49-bd5a-47ca8effec3e" />

조건문이 실행되기 전 비교 대상이 0x40 즉, "64가 맞냐?" 비교가 있습니다.

이거는 64자리가 맞느냐는 내용으로 보입니다.

<img width="712" height="49" alt="image" src="https://github.com/user-attachments/assets/7074642f-5842-47c3-bdbc-ba5576920971" />

Input에 5라는 숫자를 하나 적으니 rax 값에 1이 들어간걸 확인할 수 있었습니다.

<img width="543" height="334" alt="image" src="https://github.com/user-attachments/assets/53b62b0a-1118-4c19-b03f-3441ef158d69" />

그리고 한 자리만을 넣으니 아래와 같이 "Try Again" 이라는 결과값이 나왔습니다.

<img width="160" height="33" alt="image" src="https://github.com/user-attachments/assets/29e64f20-01d5-4ab8-b85a-901cf68f9034" />

그래서 64자리 숫자를 채워보았더니 "good"이라는 결과값을 찾을 수 있었습니다.

<img width="644" height="839" alt="image" src="https://github.com/user-attachments/assets/6ac87e10-abde-47e2-821e-5f3beb4d4052" />

여기서 이제 어떠한 값을 일치시켜 good이 아닌 Nice로 가게끔 해야하는데 위 그래프를 보시면 check 함수가 보입니다..

check 함수를 자세히 확인해보면 아래와 같은데 와..... 엄청나게 많이 값을 바꾸고 그러고 있네요...... 이걸 하나씩 봐야한다니 앞이 깜깜한데 실력증진을 위해 하나씩 해나아가보겠습니다..

<img width="366" height="927" alt="image" src="https://github.com/user-attachments/assets/fb3449f8-c8d4-4416-8198-b84cbfe21a43" />


# 문제풀이

# 정리

# 느낀점
