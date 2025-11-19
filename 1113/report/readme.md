# 📘 **11/13 수업내용 요약**

## 1. **Shift & Rotate 명령어**

### ✔ Logical Shift

* **SHL(=SAL)**: 왼쪽 논리 시프트, LSB에 0 삽입
* **SHR**: 오른쪽 논리 시프트, MSB에 0 삽입

### ✔ Arithmetic Shift

* **SAR**: 오른쪽 산술 시프트, 부호비트(MSB) 유지

### ✔ Rotate

* **ROL / ROR**: 비트를 양쪽으로 회전, Carry 플래그와 상호작용
* **RCL / RCR**: Carry 플래그를 포함한 회전

### ✔ SHLD / SHRD

* 두 레지스터를 묶어서 넓은 비트 시프트 수행
* SHLD: 왼쪽, SHRD: 오른쪽

---

## 2. **Shift 활용 예제**

* 바이트 배열 전체를 오른쪽으로 1비트씩 이동시키는 과정
* 정수를 ASCII 바이너리 문자열로 변환하는 예제(BinToAsc)

---

## 3. **Multiplication / Division**

### ✔ MUL, IMUL (곱셈)

* **MUL**: unsigned
* **IMUL**: signed
* 곱셈 결과는 피연산자 크기의 **2배 크기 레지스터**에 저장
* 2-operand, 3-operand IMUL은 오버플로 발생 가능 → **OF/CF 체크 필수**

### ✔ DIV, IDIV (나눗셈)

* DIV: unsigned, IDIV: signed
* DIV/IDIV 실행 전, **피제수(dividend)는 항상 정해진 방식으로 확장**해야 함
* 결과 범위를 벗어나면 **divide overflow 예외 발생**
* divisor 0인지 반드시 확인

---

## 4. **Sign Extension**

* 정수를 더 큰 정수로 변환할 때 부호 유지 필요
* BYTE → WORD, WORD → DWORD, DWORD → QWORD 변환 예시 포함

---

## 5. **Extended Add/Sub**

### ✔ ADC (Add with Carry)

* Carry 플래그 포함 덧셈

### ✔ SBB (Subtract with Borrow)

* Carry 플래그 포함 뺄셈

---

## 6. **ASCII 및 Unpacked Decimal 연산**

* ASCII 숫자는 상위 4비트가 0011b
* AAA, AAS, AAM, AAD 등 **ASCII 숫자 조정 명령** 사용
* 문자열 형태의 큰 정수 덧셈 구현 예제 포함

---

## 7. **Packed Decimal 연산**

* BCD(Binary-Coded Decimal) 방식
* **DAA, DAS** 명령으로 ADD/SUB 결과 조정
* Packed decimal 덧셈 예제(AddPacked.asm) 제공

---

## 8. **챕터 요약**

* 비트 시프트/회전
* 곱셈·나눗셈(부호 포함)
* 캐리 기반 확장 연산
* ASCII/BCD 숫자 조정 명령
* 실제 예제 코드 중심으로 설명

---


