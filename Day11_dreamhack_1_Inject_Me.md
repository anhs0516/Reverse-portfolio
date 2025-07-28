문제

<img width="831" height="273" alt="image" src="https://github.com/user-attachments/assets/afb04401-d22e-4f9c-8966-b6fd38dc8516" />




문제해결시도

우선 x64dbg로 prob_rev.dll 파일을 열었다.
<img width="1576" height="830" alt="image" src="https://github.com/user-attachments/assets/c4aa1bdf-d2ee-410a-b5f3-e7d01c2a6a56" />

문자열을 찾아보니 dreamhack.exe 와 flag 문자가 보였다. 이를 따라가보자
<img width="1512" height="110" alt="image" src="https://github.com/user-attachments/assets/9b8baad1-744e-4e93-bd7a-ef71c84f79d9" />

eax 값들을 비교하여 zero flag 값에 따라 점프하거나 점프하지 않는게 보여서 이를 바꿔 강제로 Flag값을 띄우는 쪽으로 가는 방향으로 시도하려고 한다.
<img width="1421" height="307" alt="image" src="https://github.com/user-attachments/assets/9ac33b5e-52be-443b-a059-b7eeac8e4e06" />

아래와 같이 변경

<img width="1369" height="163" alt="image" src="https://github.com/user-attachments/assets/6baa5c58-919c-4ac8-8a5e-3181bb5b7c7e" />

우선 Flag쪽으로 강제로 점프했더니 아래의 결과값이 보였다.

<img width="220" height="183" alt="image" src="https://github.com/user-attachments/assets/e93a61ca-08b9-4178-8428-0e5f36dc0052" />

정답이 아닌듯함... 결국 dreamhack.exe의 파일이름과 연관된 것을 찾아 문제출제자의 의도에 맞게 변경해서 실행해야 하는 것으로 보임

조금 내려가보니 GetModuleFileNameA, PathFindFileNameA가 확인된다 해당 라이브러리들이 연관되어 있을 것으로 보이는데 처음보는 것으로 알아보도록 하자
<img width="1408" height="174" alt="image" src="https://github.com/user-attachments/assets/428efee7-16bc-42a9-a713-4bb2ae0b2c8e" />

*** GetModuleFileNameA
- 현재 실행중인 실행파일의 경로 얻기

```code
DWORD GetModuleFileNameA(
  [in, optional] HMODULE hModule, // 경로가 요청되는 로드된 모듈에 대한 핸들 , NULL의 경우 GetModuleFileName 현재 프로세스의 실팽 파일 경로를 검색함
  [out]          LPSTR   lpFilename, // 모듈의 정규화된 경로를 수신하는 버퍼에 대한 포인터
  [in]           DWORD   nSize // lpFilename 버퍼의 크기
);

```

*** PathFindFileNameA
```code
LPCSTR PathFindFileNameA(
  [in] LPCSTR pszPath // 검색할 경로가 포함된 최대 길이 MAX_PATH null로 끝나는 문자열에 대한 포인터
);
```


