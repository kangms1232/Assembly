

***

### 1. Write a sequence of statements that use only PUSH and POP instructions to exchange the values in the EAX and EBX registers (or RAX and RBX in 64-bit mode).
- **해석:** PUSH와 POP 명령만 사용해서 EAX와 EBX(또는 RAX와 RBX)의 값을 서로 교환하는 명령문을 작성하시오.
- **정답:**
```assembly
push eax
push ebx
pop eax
pop ebx
```

***

### 2. Suppose you wanted a subroutine to return to an address that was 3 bytes higher in memory than the return address currently on the stack. Write a sequence of instructions that would be inserted just before the subroutine’s RET instruction that accomplish this task.
- **해석:** 현재 스택에 있는 반환 주소보다 3바이트 더 높은 주소로 복귀하고 싶을 때, 해당 동작을 RET 바로 전에 수행하도록 명령문 시퀀스를 작성하시오.
- **정답:**
```assembly
pop eax          ; 스택에서 반환 주소를 eax로 가져옴
add eax, 3       ; 반환 주소에 3바이트 더함
push eax         ; 수정된 주소를 다시 스택에 푸시
```

***

### 3. Functions in high-level languages often declare local variables just below the return address on the stack. Write an instruction that you could put at the beginning of an assembly language subroutine that would reserve space for two integer doubleword variables. Then, assign the values 1000h and 2000h to the two local variables.
- **해석:** 고급 언어의 함수에서는 반환 주소 바로 아래에 지역 변수를 선언한다. 어셈블리 서브루틴 시작에서 doubleword 2개(4바이트씩) 공간을 확보하고, 각각 1000h와 2000h를 할당하는 명령문을 작성하시오.
- **정답:**
```assembly
sub esp, 8              ; 지역 변수 2개(8바이트) 확보
mov DWORD PTR [esp], 1000h    ; 첫 번째 변수
mov DWORD PTR [esp+4], 2000h  ; 두 번째 변수
```

***

### 4. Write a sequence of statements using indexed addressing that copies an element in a double word array to the previous position in the same array.
- **해석:** 인덱스 주소 지정 방식을 사용해 doubleword 배열의 특정 요소 값을 바로 이전 위치로 복사하는 명령문을 작성하시오.
- **정답:**
```assembly
mov eax, [esi+4]   ; 다음 원소 값 가져오기
mov [esi], eax     ; 현재 위치에 복사하기 (이전 인덱스에 복사)
```

***

### 5. Write a sequence of statements that display a subroutine’s return address. Be sure that whatever modifications you make to the stack do not prevent the subroutine from returning to its caller.
- **해석:** 서브루틴 반환 주소를 표시(출력)하는 명령문 시퀀스를 작성하시오. 스택 변경으로 인해 서브루틴이 정상적으로 호출자에게 복귀하지 못하는 일이 없도록 해야 한다.
- **정답:**
```assembly
mov eax, [esp]      ; 반환 주소를 eax에 복사
call WriteHex       ; eax값(반환 주소)을 16진수 형태로 출력 (Irvine32 라이브러리 예)
```
