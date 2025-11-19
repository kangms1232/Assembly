문제 1  
Q : In the following code sequence, show the value of AL after each shift or rotate instruction has executed:  
- 다음 코드 순서에서 각 시프트(shift) 또는 로테이트(rotate) 명령어가 실행된 후 AL의 값을 보여주세요.

mov al,0D4h  
shr al,1             ; a.  
mov al,0D4h  
sar al,1             ; b.  
mov al,0D4h  
sar al,4             ; c.  
mov al,0D4h  
rol al,1             ; d.

A : 0D4h = 11010100b

- a. 01101010b => 6Ah  
- b. 11101010b => 0EAh  
- c. 11111101b => 0FDh  
- d. 10101001b => 0A9h

문제 2  
Q : In the following code sequence, show the value of AL after each shift or rotate instruction has executed:  
- 다음 코드 순서에서 각 시프트(shift) 또는 로테이트(rotate) 명령어가 실행된 후 AL의 값을 보여주세요.

mov al,0D4h  
ror al,3             ; a.  
mov al,0D4h  
rol al,7             ; b.  
stc  
mov al,0D4h  
rcl al,1             ; c.  
stc  
mov al,0D4h  
rcr al,3             ; d.

A : 0D4h = 11010100b, stc에서 CF = 1

- a. 10011010b => 9Ah  
- b. 01101010b => 6Ah  
- c. 10101001b => 0A9h  
- d. 00111010b => 3Ah

문제 3  
Q : What will be the contents of AX and DX after the following operation?  
- 다음 연산 후 AX와 DX의 내용은 무엇입니까?

mov dx,0  
mov ax,222h  
mov cx,100h  
mul cx

A : DX : 0002h, AX : 2200h

문제 4  
Q : What will be the contents of AX after the following operation?  
- 다음 연산 후 AX의 내용은 무엇입니까?

mov ax,63h  
mov bl,10h  
div bl

A : AX : 0306h

문제 5  
Q : What will be the contents of EAX and EDX after the following operation?  
- EAX와 EDX의 내용은 다음 연산 후 어떻게 됩니까?

mov eax,123400h  
mov edx,0  
mov ebx,10h  
div ebx

A : EDX : 00000000h, EAX : 00012340h

문제 6  
Q : What will be the contents of AX and DX after the following operation?  
- 다음 연산 후 AX와 DX의 내용은 무엇입니까?

mov ax,4000h  
mov dx,500h  
mov bx,10h  
div bx

A : 나눗셈 결과의 몫이 목적지인 AX에 들어갈 수 없다. 오버플로우가 발생하고 프로그램은 종료된다.

문제 7  
Q : What will be the contents of BX after the following instructions execute?  
- 다음 명령어들이 실행된 후 BX의 내용은 무엇입니까?

mov bx,5  
stc  
mov ax,60h  
adc bx,ax

A : BX : 0066h

문제 8  
Q : Describe the output when the following code executes in 64-bit mode:  
- 다음 코드가 64비트 모드에서 실행될 때 출력 결과를 설명하세요:

.data  
dividend_hi  QWORD 00000108h  
dividend_lo  QWORD 33300020h  
divisor      QWORD 00000100h  
.code  
mov  rdx,dividend_hi  
mov  rax,dividend_lo  
div  divisor

A : 나눗셈 결과의 몫이 목적지인 RAX에 들어갈 수 없다. 오버플로우가 발생하고 프로그램은 종료된다.

문제 9  
Q : The following program is supposed to subtract val2 from val1. Find and correct all logic errors (CLC clears the Carry flag):  
- 다음 프로그램은 val1에서 val2를 빼는 동작을 수행해야 합니다. 모든 논리 오류를 찾아 수정하세요 (CLC는 캐리 플래그를 초기화합니다):

.data  
 val1   QWORD 20403004362047A1h  
 val2   QWORD 055210304A2630B2h  
 result QWORD 0  
.code  
   mov  cx,8               ; loop counter  
   mov  esi,val1           ; set index to start  
   mov  edi,val2  
   clc                     ; clear Carry flag  
top:  
   mov  al,BYTE PTR[esi]   ; get first number  
   sbb  al,BYTE PTR[edi]    ; subtract second  
   mov  BYTE PTR[esi],al    ; store the result  
   dec  esi  
   dec  edi  
   loop top

A :  
esi와 edi가 변수의 주소를 가리키게 하고 싶을 때는 OFFSET 지시어를 사용해야 한다. 해당 명령어는 val1, val2의 하위 8바이트 값을 가지게 한다.  
변수 저장방식은 '리틀 엔디안'으로 가장 작은 주소에 가장 하위 바이트의 값이 들어간다. dec를 하면 주소가 이상한 값을 가리킨다.  
뺄셈의 결과 값을 return에 저장하지 않고 esi가 가리키는 주소로 덮어씌운다.

문제 10  
Q : What will be the hexadecimal contents of RAX after the following instructions execute in 64-bit mode?  
- 다음 명령어들이 64비트 모드에서 실행된 후 RAX의 16진수 내용은 무엇입니까?

.data  
multiplicand QWORD 0001020304050000h  
.code  
imul rax,multiplicand, 4

A : RAX : 0004080C10140000h  
다른 풀이 : x4는 왼쪽으로 2번 시프트 하는 것과 같다.  
multiplicand : 0000 0000 0000 0001 0000 0010 0000 0011 0000 0100 0000 0101 0000 0000 0000 0000b  
=> RAX : 0000 0000 0000 0100 0000 1000 0000 1100 0001 0000 0001 0100 0000 0000 0000 0000b = 0004080C10140000h
