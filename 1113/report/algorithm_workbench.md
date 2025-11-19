## 문제 1  
### Q : Write a sequence of shift instructions that cause AX to be sign-extended into EAX. In other words, the sign bit of AX is copied into the upper 16 bits of EAX. Do not use the CWD instruction.
- AX를 EAX로 부호 확장(sign-extend) 시키는 시프트 명령어 시퀀스를 작성하세요. 즉, AX의 부호 비트가 EAX의 상위 16비트로 복사되어야 합니다. CWD 명령어는 사용하지 마세요.  

A :  
```asm
shl eax, 16
sar eax, 16
```

## 문제 2  
### Q : Suppose the instruction set contained no rotate instructions. Show how you would use SHR and a conditional jump instruction to rotate the contents of the AL register 1 bit to the right.

- 명령어 집합에 로테이트 명령어가 없다면 SHR와 조건 분기 명령어를 사용하여 AL 레지스터의 내용을 오른쪽으로 1비트 회전시키는 방법을 보여주세요.  

A :  
```asm
SHR AL, 1
jnc next
or al, 80h
next:
```

## 문제 3  
### Q : Write a logical shift instruction that multiplies the contents of EAX by 16.

- 논리적 시프트 명령어를 사용하여 EAX의 내용을 16배 곱하는 코드를 작성하세요. 
 
A :  
```asm
SHL EAX, 4
```

## 문제 4
### Q : Write a logical shift instruction that divides EBX by 4.
- 논리적 시프트 명령어를 사용하여 EBX를 4로 나누는 코드를 작성하세요.  

A :  
```asm
SHR EBX, 2
```

## 문제 5
### Q : Write a single rotate instruction that exchanges the high and low halves of the DL register.
- DL 레지스터의 상하반부를 교환하는 단일 로테이트 명령어를 보여주세요.  

A :  
```asm
ROR DL, 4
```

## 문제 6
### Q : Write a single SHLD instruction that shifts the highest bit of the AX register into the lowest bit position of DX and shifts DX one bit to the left.

- AX 레지스터의 최고 비트를 DX의 최하위 비트 위치로 이동시키고 DX를 왼쪽으로 1비트 시프트하는 단일 SHLD 명령어를 작성하세요.  

A :  
```asm
SHLD DX, AX, 1
```

## 문제 7
### Write a sequence of instructions that shift three memory bytes to the right by 1 bit position. Use the following test data:

- 다음과 같이 메모리의 3바이트를 1비트 오른쪽으로 시프트하는 시퀀스를 작성하세요. 테스트 데이터를 사용하세요:  
```asm
byteArray BYTE 81h,20h,33h  
```
A :  
```asm
SHR BYTE PTR[byteArray], 1
SHR BYTE PTR[byteArray+1], 1
SHR BYTE PTR[byteArray+2], 1
```

## 문제 8
### Write a sequence of instructions that shift three memory words to the left by 1 bit position. Use the following test data:

- 다음 테스트 데이터를 사용하여 3개의 메모리 워드를 1비트 왼쪽으로 시프트하는 명령어 시퀀스를 작성하세요:  
```asm
wordArray WORD 810Dh, 0C064h, 93ABh  
```
A :  
```asm
SHL WORD PTR[wordArray], 1
SHL WORD PTR[wordArray+2], 1
SHL WORD PTR[wordArray+4], 1
```

## 문제 9
### Write instructions that multiply -5 by 3 and store the result in a 16-bit variable val1.
 
- -5를 3배 곱하여 16비트 변수 val1에 결과를 저장하는 명령어 시퀀스를 작성하세요.  

A :  
```asm
mov ax, 3
IMUL ax, -5
mov val1, ax
```

## 문제 10  
### Write instructions that divide -276 by 10 and store the result in a 16-bit variable val1.
- -276를 10으로 나누고 결과를 16비트 변수 val1에 저장하는 명령어를 작성하세요.  

A :  
```asm
mov ax, -276
cwd
IDIV 10
mov val1, ax
```

## 문제 11  
### Implement the following C++ expression in assembly language, using 32-bit unsigned operands:

- 다음 C++ 표현식을 어셈블리 언어로 구현하세요, 32비트 부호 없는 피연산자를 사용하여:  
```asm
val1 = (val2 * val3) / (val4 - 3)  
```
A :  
```asm
mov eax, val2
mul val3
mov ebx, val4
sub ebx, 3
div ebx
mov val1, eax
```

## 문제 12  
### Implement the following C++ expression in assembly language, using 32-bit signed operands:
- 다음 C++ 표현식을 어셈블리 언어로 구현하세요, 32비트 부호 있는 피연산자를 사용하여:  
```asm
val1 = (val2 / val3) * (val1 + val2)
```  
A :  
```asm
mov eax, val2
cdq
idiv val3
mov ebx, val1
add ebx, val2
imul eax, ebx
mov val1, eax
```

## 문제 13
### Write a procedure that displays an unsigned 8-bit binary value in decimal format. Pass the binary value in AL. The input range is limited to 0 to 99, decimal. The only procedure you can call from the book’s link library is WriteChar. The procedure should contain approximately eight instructions. Here is a sample call:

- AL에 전달된 부호 없는 8비트 이진 값을 10진수 형식으로 출력하는 프로시저를 작성하세요. 입력 범위는 0부터 99까지입니다. 책에서 제공하는 링크 라이브러리의 WriteChar 함수만 호출할 수 있습니다. 프로시저는 약 8개의 명령어로 구성되어야 합니다.  
```asm
mov  al,65
call showDecimal8
```
A :  
```asm
showDecimal8:
  AAM             ; 10의 자리가 AH에, 1의 자리가 AL에 저장
  or AX, 3030h    ; 각 자리에 ASCII 문자열의 값을 씌움
  push eax
  mov al, ah      ; 10의 자리를 먼저 출력
  call WriteChar
  pop eax         ; 1의 자리를 출력
  call WriteChar
  ret
```

## 문제 14  
### Q : Challenge: Suppose AX contains 0072h and the Auxiliary Carry flag is set as a result of adding two unknown ASCII decimal digits. Use the Intel 64 and IA-32 Instruction Set Reference to determine what output the AAA instruction would produce. Explain your answer.
- 도전 문제: AX에 0072h가 들어 있고, 미지의 두 ASCII 10진수가 더해져서 보조 캐리 플래그(Auxiliary Carry 플래그)가 설정되었다고 가정합니다. Intel 64 및 IA-32 명령어 세트 참조를 사용하여 AAA 명령어가 어떤 출력을 생성하는지 결정하고, 그 답을 설명하세요.﻿

A : AAA를 실행하면 al & 0Fh의 결과가 9보다 큰지, 아니면 AF = 1인지 검사한다. 그렇다면 AH+1, AL+6을 하여 비압축 10진수 형태로 만들어 저장한다.
AF가 셋 되었으므로 al & 0Fh -> ah+1, al+6 => AX : 0108h

## 문제 15  
### Q : Challenge: Using only SUB, MOV, and AND instructions, show how to calculate x = n mod y, assuming that you are given the values of n and y. You can assume that n is any 32-bit unsigned integer, and y is a power of 2.
- 도전 문제: SUB, MOV, AND 명령어만 사용하여 x = n mod y를 계산하는 방법을 보여주세요. 여기서 n과 y의 값이 주어지며, n은 32비트 부호 없는 정수이고, y는 2의 거듭제곱임을 가정합니다.﻿

A :  
```asm
mov edx, divisor
sub edx, 1
mov eax, dividend
and eax, edx
mov answer, eax
```

## 문제 16  
### Q : Challenge: Using only SAR, ADD, and XOR instructions (but no conditional jumps), write code that calculates the absolute value of the signed integer in the EAX register. Hints: A number can be negated by adding -1 to it and then forming its one’s complement. Also, if you XOR an integer with all 1s, its 1s are reversed. On the other hand, if you XOR an integer with all zeros, the integer is unchanged.
- 도전 문제: 조건 분기 없이 SAR, ADD, XOR 명령어만 사용하여 EAX 레지스터에 있는 부호 있는 정수의 절대값을 계산하는 코드를 작성하세요. 힌트: 어떤 수는 -1을 더한 후 1의 보수를 취하면 부호를 바꿀 수 있습니다. 또한, 정수를 모두 1인 값과 XOR하면 비트가 반전되고, 모두 0인 값과 XOR하면 값이 변하지 않습니다.

A :  
```asm
mov edx, eax
sar edx, 31
add eax, edx
xor eax, edx
```
