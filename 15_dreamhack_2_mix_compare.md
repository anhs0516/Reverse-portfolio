

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

0~15까지는 아래 조건들을 따르고

<img width="336" height="424" alt="image" src="https://github.com/user-attachments/assets/a024b94d-3d8a-45b0-b5af-2bbe378acac8" />

16~25까지는 아래 조건

<img width="374" height="179" alt="image" src="https://github.com/user-attachments/assets/5b524e1f-0570-4ab6-bb0c-862c5a2f830d" />

26~35 

<img width="355" height="172" alt="image" src="https://github.com/user-attachments/assets/e9d89308-8021-42d7-a1eb-0c35af7e30d2" />

36~45

<img width="356" height="170" alt="image" src="https://github.com/user-attachments/assets/af871009-a120-401c-83df-75685244a1a9" />

46~55

<img width="346" height="175" alt="image" src="https://github.com/user-attachments/assets/73181a59-609b-4e5f-bc84-e546b8fda5b3" />

56~63

<img width="400" height="174" alt="image" src="https://github.com/user-attachments/assets/69330dce-187b-45bb-bee8-69d196a7cff0" />


입력값이 무엇인지를 찾아야하는데 고민입니다...

우선 파이썬에 각 글자마다 적용되는 조건부터 적어보겠습니다.

# 문제풀이

역연산을 하기 전에 입력되어야하는 값들인 리스트를 추출해야 합니다. 

result.txt파일을 만들어 이 데이터를 읽게하고 조건들을 역연산하여 64글자의 입력값을 찾아내보겠습니다.

result.txt에는 512글자가 들어가있고 이를 hex 4바이트 (ex 00 00 00 39)마다 한 글자가 되어 총 64글자가 나오게 될 것으로 보입니다.

우선 조건들을 보았을 때, result 혹은 dword_**** 가 보입니다.

<img width="333" height="401" alt="image" src="https://github.com/user-attachments/assets/d88234dd-f159-47e3-8de3-5e81da777f7d" />


이 조건들이 이루어지면 값이 어떻게 되는지 나타내는 것으로 우리는 이 데이터를 역연산하면 flag 값들이 나올 것으로 판단됩니다.

<img width="883" height="846" alt="image" src="https://github.com/user-attachments/assets/8ecd25e3-67a9-4893-a119-96af53e8c3af" />



```code
def intFormat(strings):
    return int.from_bytes(bytes.fromhex(strings), "little")
    


with open("D:\\2025\\Reversing_C\\dreamhack_reverse\\2_mix_compare\\result.txt", "r") as f:
    arr = [0 for i in range(64)]
    data = f.read().strip()
    
    # #1~15
    # for i in range(len(data)//8):
    #    print((intFormat(data[i*8:(i+1)*8]) - 1) & 0xff)
    
    for i in range(len(data)//8):
        if i == 0:
            arr[i] = (intFormat(data[i*8:(i+1)*8])- 9) & 0xff
        elif i==1:
            arr[i] = ~intFormat(data[i*8:(i+1)*8]) & 0xff
        elif i==2:
            arr[i] = (intFormat(data[i*8:(i+1)*8]) + 4) & 0xff
        elif i==3:
            arr[i] = (intFormat(data[i*8:(i+1)*8]) // 2) & 0xff
        elif i==4:
            arr[i] = (intFormat(data[i*8:(i+1)*8]) -  34) & 0xff
        elif i==5:
            arr[i] = (intFormat(data[i*8:(i+1)*8]) - 40 ) & 0xff
        elif i==6:
            arr[i] = (intFormat(data[i*8:(i+1)*8]) + 40 ) & 0xff
        elif i==7:  
            arr[i] = (intFormat(data[i*8:(i+1)*8]) // 3) & 0xff
        elif i==8:
            arr[i] = ~intFormat(data[i*8:(i+1)*8]) & 0xff
        elif i==9:
            arr[i] = (intFormat(data[i*8:(i+1)*8]) // 2) & 0xff
        elif i==10:
            arr[i] = (intFormat(data[i*8:(i+1)*8]) // 4) & 0xff
        elif i==11:
            arr[i] = (intFormat(data[i*8:(i+1)*8]) // 4) & 0xff            
        elif i==12:
            arr[i] = (19 - intFormat(data[i*8:(i+1)*8])) & 0xff
        elif i==13:
            arr[i] = (intFormat(data[i*8:(i+1)*8]) - 17) & 0xff
        elif i==14:
            arr[i] = (intFormat(data[i*8:(i+1)*8]) - 30) & 0xff
        elif i==15:
            arr[i] = intFormat(data[i*8:(i+1)*8]) & 0xff
        elif i < 26:
            arr[i] = ~(intFormat(data[i*8:(i+1)*8]) - i) & 0xff
        elif i < 36:
            arr[i] = (intFormat(data[i*8:(i+1)*8]) - i) & 0xff
        elif i < 46:
            arr[i] = (intFormat(data[i*8:(i+1)*8]) + i) & 0xff
        elif i < 56:
            arr[i] = (intFormat(data[i*8:(i+1)*8]) // i) & 0xff
        elif i < 65:
            arr[i] = (intFormat(data[i*8:(i+1)*8]) + i - 100) & 0xff
            
            
for i in arr:
    print(chr(i), end ="")
```

데이터를 찾은 후 위 역연산을 실행하게 되면 아래와 같이 64글자의 flag 값을 얻을 수 있습니다.

<img width="597" height="22" alt="image" src="https://github.com/user-attachments/assets/f5cef837-0dc5-49e0-807e-d1b447ab64ab" />


# 정리

# 느낀점
