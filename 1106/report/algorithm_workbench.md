## 1. Write a single instruction that converts an ASCII digit in AL to its corresponding binary value. If AL already contains a binary value (00h to 09h), leave it unchanged.  
- AL 레지스터에 들어 있는 ASCII 숫자를 해당 이진 값으로 변환하는 단일 명령어를 작성하시오. 만약 AL에 이미 이진 값(00h~09h)이 들어 있으면, 그대로 두시오.

**답:**  
```asm
sub al, 30h
````

---

## 2. Write instructions that calculate the parity of a 32-bit memory operand.
   Hint: Use the formula presented earlier in this section: B0 XOR B1 XOR B2 XOR B3.

* 32비트 메모리 피연산자의 패리티를 계산하는 명령어를 작성하시오.
  힌트: 이 섹션의 앞부분에 제시된 공식 — B0 XOR B1 XOR B2 XOR B3.

**답:**

```asm
mov   eax, DWORD PTR [mem]   ; 메모리에서 32비트 로드 (바이트: B3 B2 B1 B0, little-endian)
mov   edx, eax
shr   edx, 8
xor   eax, edx               ; eax = x ^ (x >> 8)
mov   edx, eax
shr   edx, 16
xor   eax, edx               ; eax = (x ^ (x>>8)) ^ ((x ^ (x>>8)) >> 16)
and   eax, 0FFh              ; 선택적: 결과를 하위 바이트만 남김
; 결과: AL = B0 ^ B1 ^ B2 ^ B3
```

---

## 3. Given two bit-mapped sets named SetX and SetY, write a sequence of instructions that generate a bit string in EAX that represents members in SetX that are not members of SetY.

* SetX와 SetY라는 두 비트맵 집합이 있을 때, SetX에는 속하지만 SetY에는 속하지 않는 멤버를 나타내는 비트 문자열을 EAX에 생성하는 명령어를 작성하시오.

**답:**

```asm
.data
   SetX DWORD ?
   SetY DWORD ?
.code
   mov eax, SetX
   xor eax, SetY
   and eax, SetX
```

---

## 4. Write instructions that jump to label L1 when the unsigned integer in DX is less than or equal to the integer in CX.

* DX 레지스터에 있는 부호 없는 정수가 CX보다 작거나 같으면 L1 라벨로 점프하는 명령어를 작성하시오.

**답:**

```asm
cmp dx, cx
jbe L1
```

---

## 5. Write instructions that jump to label L2 when the signed integer in AX is greater than the integer in CX.

* AX 레지스터에 있는 부호 있는 정수가 CX보다 크면 L2 라벨로 점프하는 명령어를 작성하시오.

**답:**

```asm
cmp ax, cx
jg L2
```

---

## 6. Write instructions that first clear bits 0 and 1 in AL. Then, if the destination operand is equal to zero, the code should jump to label L3. Otherwise, it should jump to label L4.

* AL 레지스터의 0번과 1번 비트를 먼저 0으로 클리어한 후, 결과가 0이면 L3로 점프하고, 그렇지 않으면 L4로 점프하는 명령어를 작성하시오.

**답:**

```asm
and al, 11111100h
jz L3
jmp L4
```

---

## 7. Implement the following pseudocode in assembly language. Use short-circuit evaluation and assume that val1 and X are 32-bit variables.

* 다음 의사코드를 어셈블리로 구현하시오. short-circuit 평가를 사용하며, val1과 X는 32비트 변수라고 가정합니다.

```c
if( val1 > ecx ) AND ( ecx > edx ) 
   X = 1
else
   X = 2;
```

**답:**

```asm
cmp val1, ecx
ja L1
jmp next
L1:
   cmp ecx, edx
   ja L2
   jmp next
L2:
   mov X, 1
   jmp END
next:
   mov X, 2
END:
```

---

## 8. Implement the following pseudocode in assembly language. Use short-circuit evaluation and assume that X is a 32-bit variable.

* 다음 의사코드를 어셈블리로 구현하시오. short-circuit 평가를 사용하며, X는 32비트 변수라고 가정합니다.

```c
if( ebx > ecx ) OR ( ebx > val1 ) 
   X = 1
else
   X = 2
```

**답:**

```asm
cmp ebx, ecx
jbe L1
jmp next
L1:
   cmp ebx, val1
   jbe L2
   jmp next
L2:
   mov X, 2
   jmp END
next:
   mov X, 1
END:
```

---

## 9. Implement the following pseudocode in assembly language. Use short-circuit evaluation and assume that X is a 32-bit variable.

* 다음 의사코드를 어셈블리로 구현하시오. short-circuit 평가를 사용하며, X는 32비트 변수라고 가정합니다.

```c
if( ebx > ecx AND ebx > edx) OR ( edx > eax ) 
   X = 1
else
   X = 2
```

**답:**

```asm
cmp ebx, ecx
jbe L1
cmp ebx, edx
ja next
L1:
   cmp edx, eax
   ja next
   jmp L2
L2:
   mov X, 2
   jmp END
next:
   mov X, 1
END:
```

---

## 10. Implement the following pseudocode in assembly language. Use short-circuit evaluation and assume that A, B, and N are 32-bit signed integers.

* 다음 의사코드를 어셈블리로 구현하시오. short-circuit 평가를 사용하며, A, B, N은 32비트 부호 있는 정수라고 가정합니다.

```c
while N > 0
  if N != 3 AND (N < A OR N > B)
     N = N – 2
  else
     N = N – 1
end while
```

**답:**

```asm
begin:
   cmp N, 0
   jng ENDwhile
   cmp N, 3
   jnz L1
   jmp ELSE
L1:
   cmp N, A
   jl YAP
   cmp N, B
   jg YAP
   jmp ELSE
YAP:
   sub N, 2
   jmp begin
ELSE:
   sub N, 1
   jmp begin  
ENDwhile:
```

```
```
