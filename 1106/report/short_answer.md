### 1. What will be the value of BX after the following instructions execute?  
**- 다음 명령어 실행 후 BX의 값은 무엇인가?**

```asm
mov  bx,0FFFFh
and  bx,6Bh
````

**답:** BX = `006Bh`

---

### 2. What will be the value of BX after the following instructions execute?

**- 다음 명령어 실행 후 BX의 값은 무엇인가?**

```asm
mov  bx,91BAh
and  bx,92h
```

**답:** BX = `0092h`

---

### 3. What will be the value of BX after the following instructions execute?

**- 다음 명령어 실행 후 BX의 값은 무엇인가?**

```asm
mov  bx,0649Bh
or   bx,3Ah
```

**답:** BX = `64BBh`

---

### 4. What will be the value of BX after the following instructions execute?

**- 다음 명령어 실행 후 BX의 값은 무엇인가?**

```asm
mov  bx,029D6h
xor  bx,8181h
```

**답:** BX = `A857h`

---

### 5. What will be the value of EBX after the following instructions execute?

**- 다음 명령어 실행 후 EBX의 값은 무엇인가?**

```asm
mov  ebx,0AFAF649Bh
or   ebx,3A219604
```

**답:** EBX = `0BFAFF69Fh`

---

### 6. What will be the value of RBX after the following instructions execute?

**- 다음 명령어 실행 후 RBX의 값은 무엇인가?**

```asm
mov  rbx,0AFAF649Bh
xor  rbx,0FFFFFFFFh
```

**답:** RBX = `0000000050509B64h`

---

### 7. In the following instruction sequence, show the resulting value of AL where indicated, in binary:

**- 다음 명령어 실행 후 AL의 값을 이진수로 나타내시오.**

```asm
mov  al,01101111b
and  al,00101101b        ; a.
mov  al,6Dh
and  al,4Ah              ; b.
mov  al,00001111b
or   al,61h              ; c.
mov  al,94h
xor  al,37h              ; d.
```

**답:**
a. `00101101b`
b. `01001000b`
c. `01101111b`
d. `10100011b`

---

### 8. In the following instruction sequence, show the resulting value of AL where indicated, in hexadecimal:

**- 다음 명령어 실행 후 AL의 값을 16진수로 나타내시오.**

```asm
mov  al,7Ah
not  al               ; a.
mov  al,3Dh
and  al,74h           ; b.
mov  al,9Bh
or   al,35h           ; c.
mov  al,72h
xor  al,0DCh          ; d.
```

**답:**
a. `85h`
b. `34h`
c. `0BFh`
d. `0AEh`

---

### 9. In the following instruction sequence, show the values of the Carry, Zero, and Sign flags where indicated:

**- 다음 명령어 실행 후 Carry, Zero, Sign 플래그 값을 쓰시오.**

```asm
mov  al,00001111b
test al,00000010b       ; a.
mov  al,00000110b
cmp  al,00000101b       ; b.
mov  al,00000101b
cmp  al,00000111b       ; c.
```

**답:**
a. CF = 0, ZF = 0, SF = 0
b. CF = 0, ZF = 0, SF = 0
c. CF = 1, ZF = 0, SF = 1

---

### 10. Which conditional jump instruction executes a branch based on the contents of ECX?

**- ECX의 값에 따라 분기하는 조건부 점프 명령어는?**

**답:** `JECXZ`

---

### 11. How are JA and JNBE affected by the Zero and Carry flags?

**- JA와 JNBE는 Zero와 Carry 플래그에 의해 어떻게 영향을 받는가?**

**답:** Zero = 0이고 Carry = 0일 때 점프한다.

---

### 12. What will be the final value in EDX after this code executes?

**- 다음 코드 실행 후 EDX의 최종 값은?**

```asm
mov  edx,1
mov  eax,7FFFh
cmp  eax,8000h
jl   L1
mov  edx,0
L1:
```

**답:** EDX = `0000 0000 0000 0001b`

---

### 13. What will be the final value in EDX after this code executes?

**- 다음 코드 실행 후 EDX의 최종 값은?**

```asm
mov  edx,1
mov  eax,7FFFh
cmp  eax,8000h
jb   L1
mov  edx,0
L1:
```

**답:** EDX = `0000 0000 0000 0001b`

---

### 14. What will be the final value in EDX after this code executes?

**- 다음 코드 실행 후 EDX의 최종 값은?**

```asm
mov  edx,1
mov  eax,7FFFh
cmp  eax,0FFFF8000h
jl   L2
mov  edx,0
L2:
```

**답:** EDX = `0000 0000 0000 0000b`

---

### 15. (True/False): The following code will jump to the label named Target.

**- True/False: 다음 코드가 Target 라벨로 점프하는가?**

```asm
mov  eax,-30
cmp  eax,-50
jg   Target
```

**답:** True

---

### 16. (True/False): The following code will jump to the label named Target.

**- True/False: 다음 코드가 Target 라벨로 점프하는가?**

```asm
mov  eax,-42
cmp  eax,26
ja   Target
```

**답:** True

---

### 17. What will be the value of RBX after the following instructions execute?

**- 다음 명령어 실행 후 RBX의 값은?**

```asm
mov  rbx,0FFFFFFFFFFFFFFFFh
and  rbx,80h
```

**답:** RBX = `0FFFFFFFFFFFFFF80h`

---

### 18. What will be the value of RBX after the following instructions execute?

**- 다음 명령어 실행 후 RBX의 값은?**

```asm
mov  rbx,0FFFFFFFFFFFFFFFFh
and  rbx,808080h
```

**답:** RBX = `0FFFFFFFFFF808080h`

---

### 19. What will be the value of RBX after the following instructions execute?

**- 다음 명령어 실행 후 RBX의 값은?**

```asm
mov  rbx,0FFFFFFFFFFFFFFFFh
and  rbx,80808080h
```

**답:** RBX = `0FFFFFFFF80808080h`


