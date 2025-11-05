# MASM Procedure & Stack Review (문제 및 풀이)

이 문서는 x86 어셈블리어(주로 MASM 기준)의 스택, 프로시저 관련 주요 문제를 한글 해설과 함께 README.md 형식으로 정리한 예시입니다.

***

### 1. Which instruction pushes all of the 32-bit general-purpose registers on the stack?
- 32비트 범용 레지스터 전체를 스택에 푸시하는 명령어는?

  정답: `PUSHAD`

### 2. Which instruction pushes the 32-bit EFLAGS register on the stack?
- 32비트 EFLAGS 레지스터를 스택에 푸시하는 명령어는?

  정답: `PUSHFD`

### 3. Which instruction pops the stack into the EFLAGS register?
- 스택의 값을 EFLAGS 레지스터로 POP하는 명령어는?

  정답: `POPFD`

### 4.  Challenge: Another assembler (called NASM) permits the PUSH instruction to list multiple specific registers. Why might this approach be better than the PUSHAD instruction in MASM? Here is a NASM example:
    PUSH EAX EBX ECX

- (Challenge) NASM은 PUSH EAX EBX ECX처럼 여러 레지스터를 한번에 나열 가능하다.
이 방법이 MASM의 PUSHAD보다 나은 점은?

  정답: **필요한 일부 레지스터만 선택적으로 저장할 수 있어, 메모리와 실행 시간을 아낄 수 있습니다. PUSHAD는 항상 8개 전체 레지스터를 저장합니다.**

### 5. Challenge: Suppose there were no PUSH instruction. Write a sequence of two other instructions that would accomplish the same as push eax.
 
- (Challenge) PUSH 명령이 없다면 push eax와 같은 동작을 만드는 두 개의 명령어는?

 정답:
```assembly
sub esp, 4
mov [esp], eax
```

### 6. (True/False): The RET instruction pops the top of the stack into the instruction pointer.
- (O/X) RET 명령어는 스택 최상단 값을 명령어 포인터에 pop한다.

  정답: **True (O)**

### 7. (True/False): Nested procedure calls are not permitted by the Microsoft assembler unless the NESTED operator is used in the procedure definition.
- (O/X) NESTED 연산자를 사용하지 않으면 MASM에서 중첩된 프로시저 호출이 허용되지 않는다.

  정답: **False (X)**

### 8. (True/False): In protected mode, each procedure call uses a minimum of 4 bytes of stack space.
- (O/X) 보호 모드에서 각 프로시저 호출마다 최소한 4바이트의 스택 공간을 사용한다.

  정답: **True (O)**

### 9. (True/False): The ESI and EDI registers cannot be used when passing 32-bit parameters to procedures.
- (O/X) 32비트 인자 전달 시 ESI와 EDI는 사용할 수 없다.

  정답: **False (X)**

### 10. (True/False): The ArraySum procedure (Section 5.2.5) receives a pointer to any array of doublewords.
- (O/X) ArraySum 프로시저는 doubleword 배열 포인터를 인수로 받는다.

  정답: **True (O)**

### 11. (True/False): The USES operator lets you name all registers that are modified within a procedure.
- (O/X) USES 지시어는 프로시저 내에서 변경되는 모든 레지스터를 명시할 수 있게 해준다.

  정답: **True (O)**

### 12. (True/False): The USES operator only generates PUSH instructions, so you must code POP instructions yourself.
- (O/X) USES는 PUSH만 생성한다. POP은 직접 작성해야 한다.

  정답: **False (X)**

### 13. (True/False): The register list in the USES directive must use commas to separate the register names.
- (O/X) USES 지시어의 레지스터 목록은 컴마(,)로 구분해야 한다.

  정답: **True (O)**

### 14. Which statement(s) in the ArraySum procedure (Section 5.2.5) would have to be modified so it could accumulate an array of 16-bit words? Create such a version of ArraySum and test it.
- (수정) ArraySum 프로시저를 16비트 배열 합산용으로 바꿔라.
정답 예시:
```assembly
ArraySum16 PROC
    push esi
    push ecx
    xor eax, eax
L1:
    add ax, [esi]   ; 16비트(WORD) 더하기
    add esi, 2      ; 2바이트씩 전진
    loop L1
    pop ecx
    pop esi
    ret
ArraySum16 ENDP
```

### 15. What will be the final value in EAX after these instructions execute?
```assembly
push 5
push 6
pop eax
pop eax
```
- 다음 실행 후 EAX 값은?

  정답: **5**


***

### 16. Which statement is true about what will happen when the example code runs?
- 해당 예제 코드가 실행될 때 실제로 일어나는 현상에 대해 옳은 설명은 무엇입니까?

```assembly
main PROC
  push 10
  push 20
  call Ex2Sub
  pop  eax
  INVOKE ExitProcess,0
main ENDP
Ex2Sub PROC
  pop eax
  ret
Ex2Sub ENDP
```



**정답:** a. EAX will equal 10 on line 6

***

### 17. Which statement is true about what will happen when the example code runs?
- 해당 예제 코드가 실행될 때 실제로 어떤 일이 일어날지에 대해 옳은 설명은 무엇입니까?

```assembly
main PROC
  mov eax,30
  push eax
  push 40
  call Ex3Sub
  INVOKE ExitProcess,0
main ENDP
Ex3Sub PROC
  pusha
  mov eax,80
  popa
  ret
Ex3Sub ENDP
```


**정답:** c. EAX will equal 30 on line 6

***

### 18. Which statement is true about what will happen when the example code runs?
- 해당 예제 코드가 실행될 때 실제로 어떤 일이 일어날지에 대해 옳은 설명은 무엇입니까?

```assembly
main PROC
  mov eax,40
  push offset Here
  jmp Ex4Sub
Here:
  mov eax,30
  INVOKE ExitProcess,0
main ENDP
Ex4Sub PROC
  ret
Ex4Sub ENDP
```


**정답:** a. EAX will equal 30 on line 7

***

### 19. Which statement is true about what will happen when the example code runs?
- 해당 예제 코드가 실행될 때 실제로 어떤 일이 일어날지에 대해 옳은 설명은 무엇입니까?

```assembly
main PROC
  mov edx,0
  mov eax,40
  push eax
  call Ex5Sub
  INVOKE ExitProcess,0
main ENDP
Ex5Sub PROC
  pop eax
  pop edx
  push eax
  ret
Ex5Sub ENDP
```


**정답:** c. EDX will equal 0 on line 6

***

### 20. What values will be written to the array when the following code executes?
- 다음 코드가 실행될 때 배열에는 어떤 값들이 저장됩니까?

```assembly
.data
array DWORD 4 DUP(0)
.code
main PROC
  mov eax,10
  mov esi,0
  call proc_1
  add esi,4
  add eax,10
  mov array[esi],eax
  INVOKE ExitProcess,0
main ENDP
proc_1 PROC
  call proc_2
  add esi,4
  add eax,10
  mov array[esi],eax
  ret
proc_1 ENDP
proc_2 PROC
  call proc_3
  add esi,4
  add eax,10
  mov array[esi],eax
  ret
proc_2 ENDP
proc_3 PROC
  mov array[esi],eax
  ret
proc_3 ENDP
```


**정답**
- array = 10
- array = 20[4]
- array = 30[8]
- array = 40

