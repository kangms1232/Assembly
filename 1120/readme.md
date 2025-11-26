# 11.20 수업 내용
***

## 서브루틴과 스택 프레임

- C/C++: 함수(function), Java: 메서드(method), MASM: 프로시저(procedure)로 부른다.  
- 호출자가 넘기는 값: 인수/인자(argument), 서브루틴이 받는 쪽 이름: 매개변수(parameter).  
- 스택 프레임(activation record): 인수, 복귀 주소, 저장된 EBP, 지역 변수, 보존할 레지스터들을 위해 스택에 확보한 영역.[1]
- 일반적인 프레임 구성(위에서 아래로): 전달된 인수들 → 복귀 주소 → 저장된 EBP → 지역 변수들 → 보존 레지스터들.[1]

***

## 스택 매개변수와 지역 변수

- 인수 전달을 레지스터 대신 스택으로 하면 push/pop이 호출 규약에 맞게 정리되고 코드가 더 깔끔해진다.  
- 값 인수: 그 값 자체를 push (예: `push val1`).  
- 참조 인수: 변수의 주소(OFFSET)를 push (예: `push OFFSET val1`), C에서 `X(&val1);` 과 동일한 개념. 배열 이름은 첫 원소 주소이므로 그대로 OFFSET 전달.  
- 프레임 기준 주소: `[EBP+4]` = 복귀 주소, `[EBP+8 + 4*x]` (x ≥ 0) = 인수들. 그래서  
  - `x_param EQU [ebp+8]`, `y_param EQU [ebp+12]` 처럼 상수로 이름 붙여 사용 가능.  
- 지역 변수는 EBP 아래에 위치하며 `[EBP-4]`, `[EBP-8]` … 형태로 접근한다.[1]

***

## ENTER/LEAVE, LOCAL, LEA

- 스택을 직접 만들면:  
  - `push ebp` → `mov ebp, esp` → `sub esp, num` 으로 프레임 생성,  
  - 끝날 때 `mov esp, ebp` → `pop ebp`.  
- ENTER/LEAVE: 위 코드를 한 번에 처리하는 명령.  
  - `ENTER localbytes, 0` 이 `push ebp / mov ebp,esp / sub esp,localbytes` 역할.  
  - `LEAVE` 가 `mov esp,ebp / pop ebp` 역할.[1]
- LOCAL 지시어: `LOCAL var1:DWORD, var2:BYTE` 처럼 이름 있는 지역 변수를 선언하고, 그만큼 프레임 내에 공간을 자동으로 확보한다.[2]
- LEA: 실행 시간에 “주소 계산 결과(오프셋)”를 레지스터에 넣는다.  
  - 예: `LEA esi, [ebp-8]` 가능 (OFFSET은 컴파일 타임 상수만 처리).

***

## 호출 규약, INVOKE, ADDR, PROC, PROTO

- stdcall 규약: 인수는 오른쪽에서 왼쪽 순으로 push, 스택 정리는 보통 피호출자(ret n).[3]
- `INVOKE`는  
  - 인수 push + call을 한 번에 처리하는 디렉티브:  
    - 예전 코드  
      - `push TYPE array`  
      - `push LENGTHOF array`  
      - `push OFFSET array`  
      - `call DumpArray`  
    - → `INVOKE DumpArray, OFFSET array, LENGTHOF array, TYPE array` 로 축약.[4]
- `ADDR` 연산자: INVOKE에서 포인터 인수를 보낼 때 주소를 자동 push하는 용도(LEA + push 느낌). 단, 상수 주소만 가능하고 INVOKE와 함께만 사용.[4]
- `PROC` 형식:  
  - `label PROC [attributes] [USES reglist] [parameter_list]`  
  - `distance(NEAR/FAR)`, `langtype(STDCALL 등)`, `visibility(PUBLIC/PRIVATE/EXPORT)`, `prologue` 속성 지정 가능.[2]
  - `PROC`에 매개변수와 stdcall을 쓰면, MASM이 자동으로  
    - 진입부: `push ebp / mov ebp,esp`  
    - 종료부: `ret (n*4)` 생성해 준다.[2]
  - 예:  
    - `AddTwo PROC, val1:DWORD, val2:DWORD`  
      - 내부에서 `mov eax, val1`, `add eax, val2` 처럼 매개변수 이름으로 접근 가능.  
- `PROTO` 지시어:  
  - 프로시저 프로토타입 선언.  
  - 반드시 해당 프로시저에 대한 INVOKE보다 앞에 위치해야 하며, 인수 개수/형을 체크해 준다.[1]
  - 구현(실제 PROC)이 INVOKE보다 먼저 있으면 그 PROC 정의 자체가 원형 역할을 한다.[2]

***

## 재귀, 프로시저 숨김, 외부 참조

- 재귀 서브루틴: 자기 자신을 직접 혹은 간접 호출하는 프로시저.  
  - 탈출 조건 없는 무한 재귀는 절대 ret까지 도달하지 않는다.  
  - 예: `CalcSum` 같이 `ecx`를 감소시키면서 0이 될 때까지 자기 자신을 호출하는 패턴.[5]
- 프로시저 숨김(캡슐화):  
  - 기본은 public; `mySub PROC PRIVATE` 또는 `OPTION PROC:PRIVATE` 로 모듈 내부에만 보이게 할 수 있다.  
  - 이때 외부에 보이고 싶은 것만 `PUBLIC mySub` 로 내보내서 이름 충돌 방지.[2]
- 외부 프로시저 호출:  
  - `EXTERN sub1@0:PROC` 처럼 선언하면, 실제 구현이 다른 모듈에 있어도 어셈블 타임 에러를 막고 링커가 나중에 연결한다.[2]
  - 이름 뒤 `@n` 은 인수가 차지하는 스택 바이트 수(예: 인수 2개 DWORD → @8).[2]
- 변수/심볼 외부 공유:  
  - 기본은 private. `PUBLIC count, SYM1` 으로 내보내고, 다른 모듈에서는 `EXTERN count:DWORD, SYM1:ABS` 등으로 가져온다.[2]
  - `EXTERNDEF` 는 PUBLIC + EXTERN을 합친 형태라 공통 헤더(inc)에 넣어 재사용하기 좋다.[2]

***

원래 적어둔 정리는 이미 내용이 잘 잡혀 있어서, 위처럼 “키워드별로 어디서 뭐가 자동으로 생성되는지, 시험 때 써먹을 그림” 위주로만 재배열했다. 필요하면 각각에 대해 실제 스택 그림(ESP/EBP, 인수/지역변수 위치)을 같이 그려서 또 정리해 줄 수 있다.

