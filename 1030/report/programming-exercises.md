# 1. Draw Text Colors
 Write a program that displays the same string in four different colors, using a loop. Call the Set
TextColor procedure from the book’s link library. Any colors may be chosen, but you may find
 it easiest to change the foreground color.
***

### 문제 번역


동일한 문자열을 네 가지 다른 색상으로(한 줄에 하나씩) 반복문을 이용해 출력하는 프로그램을 작성하세요. 색상 설정에는 책의 SetTextColor 프로시저를 사용합니다. (아무 색상을 골라도 되지만 보통 전경색을 바꾸는 것이 가장 쉽습니다.)

***

## MASM, Irvine32 라이브러리 예시

```assembly
INCLUDE Irvine32.inc
.data
str1 BYTE "This line is displayed in color",0
.code
main PROC
    mov ecx, 4            ; 반복 4회
    mov eax, green         ; 시작 foreground 색상(예시: green)
L1:
    call SetTextColor      ; 색상 적용
    mov edx, OFFSET str1
    call WriteString       ; 문자열 출력
    call Crlf
    inc eax                ; 다음 foreground 색상으로 변경(또는 add eax,2 등 조정 가능)
    loop L1
    call WaitMsg           ; 종료 대기
    exit
main ENDP
END main
```

***

## 핵심 포인트
- `SetTextColor`는 EAX의 하위 4비트를 글자색(전경색), 상위 4비트를 배경색으로 해석합니다. 원하는 foreground 색상 값만 바꿔도 충분히 4줄 각기 다른 색상으로 출력 가능합니다.
- `WriteString`, `Crlf`, `WaitMsg`는 Irvine32 표준 라이브러리 함수입니다.
- 각 색상 값은 yellow, blue, green 등으로 심볼릭 상수로 사용할 수 있습니다.

더 많은 색상 및 배경색 조합이 필요하면 eax 값 계산을 조정하면 됩니다.

#  2. Linking Array Items

## 문제 번역

- 시작 인덱스(`start`), 문자 배열(`chars`: BYTE 배열), 그리고 링크 배열(`links`: DWORD 배열)이 있다고 가정함.
- 프로그램은 링크 배열에서 인덱스를 따라가면서 각 문자를 올바른 순서대로 새로운 배열에 복사해야 함.
- 아래는 예시 데이터임:
  - `start = 1`
  - `chars`: H A C E B D F G
  - `links`: 0 4 5 6 2 3 7 0  (index 순서)
  - 배열 인덱스는 0부터 시작
- 결과적으로 새 배열에 복사되는 순서는 A, B, C, D, E, F, G, H가 되어야 함.

## 해결 아이디어

- 현재 인덱스를 `start`로 초기화한다.
- **반복**: 
  - `chars[current]`를 출력 배열에 복사하고,
  - `current = links[current]`로 업데이트한다.
  - `current`이 0이 되면 멈춘다.

## 의사코드(알고리즘 설명)

```plaintext
current = start
output_index = 0
while current != 0:
    output[output_index] = chars[current]
    output_index = output_index + 1
    current = links[current]
output[output_index] = chars[current]  ; 마지막 0 인덱스 문자 복사
```

## 예시(파이썬 유사 코드)

```python
start = 1
chars = ['H','A','C','E','B','D','F','G']
links = [0,4,5,6,2,3,7,0]
output = []
current = start
while current != 0:
    output.append(chars[current])
    current = links[current]
output.append(chars[current]) # 마지막 0 인덱스 문자
print(output)  # ['A','B','C','D','E','F','G','H']
```

## 코드 구현 팁
- 배열 인덱스가 0부타임에 주의.
- 링크 배열은 DWORD로 선언(4바이트 접근 사용)
- 반복 종료 조건은 `current == 0` 임을 기억!

#  3. Simple Addition (1)
 Write a program that clears the screen, locates the cursor near the middle of the screen, prompts
 the user for two integers, adds the integers, and displays their sum.


## 문제 번역
-화면을 지운 후, 커서를 화면 중앙에 위치시키고, 두 정수를 입력받아 그 합을 출력하는 프로그램을 작성하시오.

***

## MASM + Irvine32 라이브러리 예시

```assembly
INCLUDE Irvine32.inc
.data
msg1 BYTE "Enter first integer: ", 0
msg2 BYTE "Enter second integer: ", 0
summsg BYTE "Sum: ", 0
.code
main PROC
    call Clrscr                  ; 화면 지움
    mov dh, 12                   ; 대략 중간 행으로 이동 (0-based)
    mov dl, 35                   ; 대략 중간 열로 이동 (0-based)
    call Gotoxy
    mov edx, OFFSET msg1         ; 첫 정수 입력 프롬프트
    call WriteString
    call ReadInt                 ; 첫 입력 → eax
    mov ebx, eax                 ; ebx에 저장
    call Crlf
    mov edx, OFFSET msg2         ; 두번째 정수 입력 프롬프트
    call WriteString
    call ReadInt                 ; 두번째 입력 → eax
    add eax, ebx                 ; 합 계산
    call Crlf
    mov edx, OFFSET summsg       ; 결과 메시지
    call WriteString
    call WriteInt                ; 결과 출력
    call Crlf
    call WaitMsg                 ; 프로그램 종료 대기
    exit
main ENDP
END main
```

***

### 핵심 포인트
- `Clrscr` : 화면 전체 클리어
- `Gotoxy dh,dl` : 화면 중앙 근처로 커서 이동 (콘솔 기본크기 25x80 기준, dh=12, dl=35)
- `ReadInt`, `WriteInt`, `WriteString` : Irvine32 라이브러리의 표준 입출력 지원








#  4. Simple Addition (2)
 Use the solution program from the preceding exercise as a starting point. Let this new program
 repeat the same steps three times, using a loop. Clear the screen after each loop iteration.


**문제 번역:**
이전 연습문제(Single Addition) 프로그램을 참고해서, 같은 동작(두 정수를 입력받아 합을 출력)을 3번 반복하는 프로그램을 작성하시오. 각 반복이 끝날 때마다 화면을 지워야 합니다.

***

## MASM x86 + Irvine32 라이브러리 예시

```assembly
INCLUDE Irvine32.inc
.data
msg1 BYTE "Enter first integer: ", 0
msg2 BYTE "Enter second integer: ", 0
summsg BYTE "Sum: ", 0
.code
main PROC
    mov ecx, 3           ; 3번 반복
RepeatAdd:
    call Clrscr
    mov dh, 12           ; 화면 중간 행
    mov dl, 35           ; 화면 중간 열
    call Gotoxy
    mov edx, OFFSET msg1
    call WriteString
    call ReadInt         ; 첫 번째 정수 → eax
    mov ebx, eax         ; ebx에 저장
    call Crlf
    mov edx, OFFSET msg2 
    call WriteString
    call ReadInt         ; 두 번째 정수 → eax
    add eax, ebx         ; 합 계산
    call Crlf
    mov edx, OFFSET summsg
    call WriteString
    call WriteInt        ; 결과 출력
    call Crlf
    call WaitMsg         ; 각 반복 대기 (입력 시 다음 루프 진행)
    loop RepeatAdd       ; 3회 반복
    exit
main ENDP
END main
```

***

**리뷰 포인트:**
- Clrscr: 화면 지우기
- Gotoxy: 커서 위치 중앙 이동(대략 25x80 콘솔 기준)
- ecx/loop: 3번 반복제어
- WaitMsg: 각 반복마다 사용자 입력 대기 후 다음 반복

좀 더 사용자 친화적으로 하고 싶으면 반복마다 프롬프트를 바꾸거나, 합계 출력을 더 눈에 띄게 꾸밀 수도 있습니다.


***















# 5.BetterRandomRange Procedure

## 문제 해석
Irvine32 라이브러리의 RandomRange는 0 이상 N 미만의 난수를 만듭니다. 이를 확장해 M 이상 N 미만의 난수를 반환하는 BetterRandomRange 프로시저를 작성하세요. 호출 시 EBX에는 M(하한), EAX에는 N(상한)이 들어갑니다. 이 프로시저를 50번 호출하고 각 결과를 화면에 출력하는 테스트 프로그램도 작성하세요.

***

## 예시(MASM + Irvine32 라이브러리)

```assembly
INCLUDE Irvine32.inc
.data
prompt BYTE "Random number: ",0

.code
main PROC
    call Clrscr
    mov ebx, -300   ; lower bound (M)
    mov eax, 100    ; upper bound (N)
    call Randomize
    mov ecx, 50
RepeatLoop:
    push eax         ; (백업) 상한
    push ebx         ; (백업) 하한
    call BetterRandomRange
    call WriteInt    ; 난수 출력
    call Crlf
    pop ebx          ; 하한 복구
    pop eax          ; 상한 복구
    loop RepeatLoop
    call WaitMsg
    exit
main ENDP

; 입력: EBX = M (하한), EAX = N (상한)
; 출력: EAX = [M, N) 내 임의의 정수
BetterRandomRange PROC
    sub eax, ebx      ; (N-M) 구하기
    call RandomRange  ; 0 이상 N-M 미만 난수(EAX)
    add eax, ebx      ; 하한을 더해 M <= EAX < N
    ret
BetterRandomRange ENDP
END main
```

***
### 설명 요약
- BetterRandomRange는 (N-M) 범위로 난수 생성 뒤, 하한(M, EBX)을 더합니다. 결과적으로 M 이상 N 미만이 됩니다.
- 50회 반복 테스트용 루프(main에서 RepeatLoop).
- 결과는 화면에 차례대로 출력됩니다.

(주: 예시 코드는 의 논리와 일치하며, Irvine32 환경에서 실습 가능합니다.)



# 6. Random Strings
 Create a procedure that generates a random string of length L, containing all capital letters.
 When calling the procedure, pass the value of L in EAX, and pass a pointer to an array of byte
 that will hold the random string. Write a test program that calls your procedure 20 times and dis
plays the strings in the console window. 

### 문제 번역

***

L 길이의 무작위 대문자 문자열을 생성하는 프로시저를 만드세요. 프로시저 호출 시, EAX에는 L 값을, 그리고 랜덤 문자열을 저장할 byte 배열의 포인터를 넘기세요. 테스트 프로그램에서는 이 프로시저를 20번 호출하여 각각의 문자열을 콘솔에 출력하세요.

***

## 예시 (Irvine32 라이브러리, x86 MASM, 10글자 문자열)```assembly
INCLUDE Irvine32.inc

strLen = 10       ; 한 문자열의 길이
numStrings = 20   ; 생성할 문자열 개수

.data
str1 BYTE strLen+1 DUP(?)   ; 랜덤 문자열 저장 공간(+1은 널 문자)
msg  BYTE "Random string:", 0

.code
main PROC
    call Clrscr
    mov ecx, numStrings
GenerateLoop:
    mov eax, strLen         ; string length
    lea esi, str1           ; 배열 주소
    call RandomString       ; 랜덤 문자열 생성 (eax=strLen, esi=배열주소)
    ; null termination(optional)
    mov BYTE PTR [esi+strLen], 0
    mov edx, OFFSET msg
    call WriteString
    call Crlf
    lea esi, str1
    mov ecx, strLen
PrintLoop:
    mov al, [esi]
    call WriteChar
    inc esi
    loop PrintLoop
    call Crlf
    loop GenerateLoop
    call WaitMsg
    exit
main ENDP

; 랜덤 대문자 문자열 생성 프로시저
; 입력: EAX = 길이, ESI = 저장할 배열 포인터
RandomString PROC
    push ecx
    mov ecx, eax           ; 문자열 길이 반복
GenChar:
    mov eax, 26
    call RandomRange       ; 0 ~ 25 무작위
    add al, 'A'            ; 'A'(65)~'Z'(90)
    mov [esi], al
    inc esi
    loop GenChar
    pop ecx
    ret
RandomString ENDP
END main
```**설명**  
- `RandomString` 프로시저: 지정 길이(ecx 반복)에 맞게 무작위 대문자('A'~'Z') 생성 후 배열에 차례로 저장합니다.  
- 메인 루프는 20번 반복해 매번 다른 문자열을 생성, 콘솔에 출력합니다.
- 필요하다면 strLen 등의 값을 조정해 다양한 길이와 개수로 테스트할 수 있습니다.


## Random Screen Locations
 Write a program that displays a single character at 100 random screen locations, using a timing
 delay of 100 milliseconds. Hint: Use the GetMaxXY procedure to determine the current size of
 the console window.

**문제 해석:**
- "100번 반복하여 화면 내 임의 위치(좌표)에 단일 문자를 출력하라. 매번 출력 후 100밀리초 딜레이를 넣을 것. 화면 크기는 GetMaxXY 프로시저로 판별해서 좌표를 선정하도록 하라."

***

### Irvine32 라이브러리 활용 예시 코드

```assembly
INCLUDE Irvine32.inc

.data
rows WORD ?
cols WORD ?

.code
main PROC
    call Clrscr
    mov ecx, 100         ; 100회 반복
L1:
    call GetMaxXY        ; 화면 최대 Y(AX), X(DX)
    mov rows, ax
    mov cols, dx
    movzx eax, rows
    call RandomRange     ; Y좌표(행) 무작위
    mov dh, al
    movzx eax, cols
    call RandomRange     ; X좌표(열) 무작위
    mov dl, al
    call Gotoxy          ; 커서 이동 (dh=row, dl=col)
    mov al, 'H'          ; 출력 문자
    call WriteChar
    mov eax,100          ; 100ms 딜레이
    call Delay
    loop L1              ; 반복
    call WaitMsg         ; 종료 대기
    exit
main ENDP
END main
```

***

**포인트 정리**
- `GetMaxXY`로 전체 콘솔 화면 크기 얻기
- 각각 Y, X 좌표 랜덤 생성 → 커서 이동(`Gotoxy`)
- 지정 문자('H')를 출력
- 각 반복마다 100ms 딜레이
- 100회 반복 후 종료(입력 대기)


# 8. Color Matrix
 Write a program that displays a single character in all possible combinations of foreground and
 background colors (16 
 16  
256). The colors are numbered from 0 to 15, so you can use a
 nested loop to generate all possible combinations.

### 문제 번역

"16가지 전경(글자) 색상과 16가지 배경 색상을 모두 조합하여(16X16=256) 단일 문자를 다양한 색상으로 화면에 출력하는 어셈블리 프로그램을 작성하시오. 색상 번호는 0부터 15까지며, 중첩된 루프를 사용하면 모든 조합을 만들 수 있습니다."

***

### Irvine32 라이브러리 기준 예시 코드

아래는 x86 MASM 및 Irvine32 라이브러리 환경에서 모든 전경/배경 조합의 색상으로 단일 문자('H')를 출력하는 예시입니다.

```assembly
INCLUDE Irvine32.inc
.data
count DWORD ?
.code
main PROC
    mov ecx, 16             ; foreground 색상(0~15)
L1:
    mov count, ecx          ; 바깥쪽 루프 ECX 백업
    mov eax, ecx
    sub eax, 1              ; foreground 값(0~15)
    mov ebx, 0              ; background 값 초기화
    mov ecx, 16             ; background 색상(0~15)
L2:
    mov edx, eax            ; foreground 값
    mov esi, ebx            ; background 값
    mov edi, edx
    add edi, esi            ; foreground + background
    mov eax, edx
    add eax, esi
    mov eax, edx
    mov eax, esi
    mov eax, edx
    mov eax, esi
    mov eax, edx
    mov eax, esi
    mov eax, edx
    mov eax, esi
    mov eax, edx
    mov eax, esi
    mov eax, edx
    mov eax, esi
    mov eax, edx
    mov eax, esi
    ; 실제 색상 값 계산: foreground + (background * 16)
    mov eax, edx            ; foreground
    mov ebx, esi            ; background
    imul ebx, 16            ; background * 16
    add eax, ebx            ; foreground + background*16
    call SetTextColor       ; 색상 적용
    mov al, 'H'             ; 출력 문자
    call WriteChar
    inc esi                 ; 다음 background 색상
    loop L2
    call Crlf               ; 줄 바꿈
    mov ecx, count
    dec eax                 ; 다음 foreground 색상
    loop L1
    call WaitMsg
    exit
main ENDP
END main
```

***

### 주요 포인트
- foreground(글자색)와 background(배경색)를 각각 0~15로 루프하며 모든 조합 시도
- Irvine32의 `SetTextColor`를 사용할 때 색상값은 (foreground)+(background*16)으로 전달
- 예시에서는 'H'라는 문자로 테스트
- 각 조합별 줄 바꿈과 간단한 대기 메시지를 추가함


# 9. Recursive Procedure
 Direct recursion is the term we use when a procedure calls itself. Of course, you never want to
 let a procedure keep calling itself forever, because the runtime stack would fill up. Instead, you
 must limit the recursion in some way. Write a program that calls a recursive procedure. Inside
 this procedure, add 1 to a counter so you can verify the number of times it executes. Run your
 program with a debugger, and at the end of the program, check the counter’s value. Put a num
ber in ECX that specifies the number of times you want to allow the recursion to continue. Using
 only the LOOP instruction (and no other conditional statements from later chapters), find a way
 for the recursive procedure to call itself a fixed number of times.

## 문제 번역
직접 재귀(Direct recursion)는 프로시저가 자기 자신을 호출하는 것을 말한다. 무한 반복될 경우 프로그램의 런타임 스택이 가득 차기 때문에 반드시 종료 조건을 두어야 한다. 

ECX에 재귀 횟수를 지정하고, 오직 `LOOP` 명령만을 사용해(추가 조건분기 없이) 고정 횟수만큼만 재귀 호출이 이뤄지도록 하라. 재귀 함수 내부에서 카운터 값을 1씩 증가시켜 진입 횟수를 기록하고, 프로그램 끝난 뒤 그 값을 디버거로 확인하라.

***

## 정답 및 코드 예시 (MASM 기준)

### 아이디어
- 카운터 변수(counter)를 메모리에 마련
- ECX에 원하는 반복 횟수 넣기
- 재귀 함수 내부: counter를 1 증가시키고, `LOOP` 명령을 사용해 ECX가 0이 아니면 자기 자신 다시 호출

### 어셈블리 코드 예시
```assembly
.data
counter DWORD 0

.code
main PROC
    mov ecx, 10        ; 원하는 재귀 횟수
    call RecurProc
    ; 여기서 디버거나 WriteDec 등으로 counter를 확인
    exit
main ENDP

RecurProc PROC
    inc counter         ; 카운터 증가
    loop RecurProc      ; ECX--, 0이 아니면 다시 자기 자신 호출
    ret
RecurProc ENDP
```

### 설명
- main에서 ECX를 10으로 세팅하고 RecurProc 호출
- RecurProc 진입 시 counter를 1 증가
- `loop RecurProc`: ECX-- 후, ECX ≠ 0이면 RecurProc을 다시 호출(재귀)
- ECX=0이 되면 함수가 끝까지 ret로 복귀함
- counter에는 재귀 호출 횟수가 누적되어 있게 됨

***



# 10. Fibonacci Generator
 Write a procedure that produces N values in the Fibonacci number series and stores them in an
 array of doubleword. Input parameters should be a pointer to an array of doubleword, a
 counter of the number of values to generate. Write a test program that calls your procedure,
 passing N = 47. The first value in the array will be 1, and the last value will be 2,971,215,073.
 Use the Visual Studio debugger to open and inspect the array contents.

## 문제 번역

N개의 피보나치 수열 값을 doubleword 배열에 저장하는 프로시저를 작성하세요. 인수로 doubleword 배열 포인터와 생성할 값의 개수(N)를 받습니다. 테스트 프로그램에서 N=47을 전달해야 하며, 첫 번째 값은 1, 마지막 값은 2,971,215,073이 됩니다. (Visual Studio 디버거로 배열을 확인하세요.)

***

## 풀이 및 어셈블리 코드 예시 (MASM, x86 형식)

**프로시저 설계 요점:**
- 배열 포인터(예: ESI), 개수(N, 예: ECX) 전달
- a = 1, a = 1로 초기화 후, a[i] = a[i-1] + a[i-2] 방식의 반복문 실행[1]

```assembly
; 입력: ESI = 배열 주소, ECX = 생성할 값 개수
; 배열은 DWORD(4바이트) 단위, N>=2
FiboArray PROC
    mov DWORD PTR [esi], 1        ; 첫 값 1
    mov DWORD PTR [esi+4], 1      ; 두 번째 값 1
    mov eax, 1                   ; 이전값 (a[1])
    mov ebx, 1                   ; 그전값 (a[0])
    mov edx, 2                   ; 인덱스

L1:
    cmp edx, ecx
    jge EndFibo
    mov ecx, eax      ; 임시로 a[n-1] 저장
    add eax, ebx      ; eax = a[n-1] + a[n-2]
    mov [esi+edx*4], eax ; 배열 저장
    mov ebx, ecx      ; ebx = a[n-1]
    inc edx
    jmp L1
EndFibo:
    ret
FiboArray ENDP
```

**테스트 프로그램 예시:**
```assembly
.data
arr DWORD 47 DUP(?)

.code
main PROC
    mov esi, OFFSET arr
    mov ecx, 47        ; N = 47
    call FiboArray
    ; (디버거로 arr 배열 확인)
    exit
main ENDP
end main
```

**설명:**
- 배열의 첫 번째, 두 번째 값은 1로 고정
- 세 번째 원소부터 a[n] = a[n-1] + a[n-2] 식으로 반복해 값을 저장 
- 마지막 값(47번째)은 2,971,215,073이 됨

***


