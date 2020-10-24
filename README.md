# Computer_SW
CSED_211 : Introduction to Computer SW Architecture

## Book
Computer Systems: A programmer’s Perspective by R. Bryant and D. O’Hallaron, Prentice Hall, 3rd ed.

## 2. Bits, Bytes, Integers

- Hex form. 0xFA ...
- Bit level operations. & | ^ ~
- Logic operations. && || !
- Shift. << >> >>>
- Two's complement. -x = ~x+1
- Casting. Unsigned bit ⇒ sign bit act as real value
- Sign extension. sign bit .
- pointer 8 byte
- Endian, x86 Little endian. ⇒ reverse
- Integer C Puzzles (x, y : int  ux, uy : unsigned int)
    - x < 0  ⇒  ((x*2) < 0)  F
    - ux ≥ 0  T
    - x & 7 == 7  ⇒ (x << 30) <0 T
    - ux > -1  F
    - x > y  ⇒ -x < -y F  (T_min)
    - x * x ≥ 0 F
    - x > 0 && y > 0  ⇒ x+y > 0  F
    - x ≥ 0  ⇒ -x ≤ 0  T
    - x ≤ 0  ⇒ -x ≥ 0  F (T_min)
    - (x | -x) >> 31 == -1 F (0)
    - ux >> 3 == ux/8 T
    - x >> 3 == x/8 F (-)
    - x & (x-1)  ! = 0  F (0, 1)

---

## 3. Floating Point

$$(-1)^s M 2^E$$

- Bit form  sign 1 , exp X, frac Y
- Normalized Values
    - exp ≠ 000...0 or 111...1
    - E = exp - bias = exp - (2^k-1 - 1)
    - M = 1.x1,x2 ... xn
- Denormalized Values
    - exp = 000...0
        - E = 1- bias
        - M = 0.x1x2...xn
        - frac = 000...0
            - zero. +0 -0
        - frac ≠ 000...0
            - really small
    - exp = 111...1
        - frac = 000...0
            - INF
        - frac ≠ 000...0
            - NaN
    - Rounding. GRS even
- Floating Point Puzzles
    - x == (int)(float) x  F
    - x == (int)(double) x  T
    - f == (float)(double) x  T
    - d == (float) d  F
    - f == -(-f) T
    - 2/3 == 2/3.0  F
    - d < 0.0  ⇒ ((d*2) < 0.0)  T
    - d > f  ⇒  -f > -d  T
    - d * d≥ 0.0 T
    - (d+f) -d == f  F

    ---

    ## 4. Machine-Level Basic
