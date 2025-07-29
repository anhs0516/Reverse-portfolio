### 문제

<img width="831" height="273" alt="image" src="https://github.com/user-attachments/assets/afb04401-d22e-4f9c-8966-b6fd38dc8516" />




### 문제해결시도

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

### GetModuleFileNameA

- 현재 실행중인 실행파일의 경로를 구해주는 API 함수, 반환해주는 값은 경로의 길이, 인자는 3개

```code
DWORD GetModuleFileNameA(
  [in, optional] HMODULE hModule, // 경로가 요청되는 로드된 모듈에 대한 핸들 , NULL의 경우 GetModuleFileName 현재 프로세스의 실팽 파일 경로를 검색함
  [out]          LPSTR   lpFilename, // 모듈의 정규화된 경로를 수신하는 버퍼에 대한 포인터
  [in]           DWORD   nSize // lpFilename 버퍼의 크기
);

```
```
ex) GetModuleFileName(NULL, Path, 256);
: Path에 현재 실행되는 프로그램의 FullPath가 넘어옴(예시 : D://reverse//dreamhack//dreamhack.exe)

```

### PathFindFileNameA
- 파일 경로에서 파일 이름을 가져오는 API 함수, 반환해주는 값은 파일 이름, 인자는 1개

```code
LPCSTR PathFindFileNameA(
  [in] LPCSTR pszPath // 검색할 경로가 포함된 최대 길이 MAX_PATH null로 끝나는 문자열에 대한 포인터
);
```
```
ex) PathFindFileName(D://reverse//dreamhack//dreamhack.exe);
: "dreamhack.exe" 반환
```


- strncmp에서 dreamhack.exe 파일이름과 비교하는게 핵심으로 보이며, strncmp는 strcmp와 다르게 "지정한 크기만큼 비교"를 하는 것
이를 IDA로 확인해보자
<img width="2041" height="222" alt="image" src="https://github.com/user-attachments/assets/0d4fafef-c6bf-44a0-8ae1-3efbe7eed737" />

MessageBoxA에 flag 텍스트 내용에 덧붙여 내용을 반환하는 것을 보았을 때, 불러오는 파일명에 따라 내용들이 바뀌는 것을 알 수 있다.
현재 파일명으로 불러왔을 때는 알 수없는 문자들이 확인되어 이를 strncmp 지정한 크기만큼 비교하는 것을 보았을 때, dreamhack.exe로 만들어 dll 파일을 불러오면 flag값이 정상적으로 바뀔 것으로 예상된다.
<img width="509" height="654" alt="image" src="https://github.com/user-attachments/assets/001f906a-a60c-4ce2-b119-4d59d4f66f40" />



C에서 LoadLibraryA를 통해 prob_rev.dll 파일을 로드시켜보았다.

<img width="730" height="283" alt="image" src="https://github.com/user-attachments/assets/328e7445-6de4-41a0-8f4a-bbc5b5169bbf" />


flag값이 한눈에 보기 쉽게 나오는 것을 보았을 때 정답을 찾았다.. 이는 잘라놓겠습니다
* 주의사항으로는 64비트로 빌드해야 정답이 나온다 32비트로하면 프로그램 실행되자마자 꺼지는 것처럼 보여요
<img width="178" height="154" alt="image" src="https://github.com/user-attachments/assets/ab9c3c21-dfbb-4fcd-bd53-8a1153a8147e" />

