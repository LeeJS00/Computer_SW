# Computer_SW
CSED_211 : Introduction to Computer SW Architecture

## Book

Computer Systems: A programmer’s Perspective by R. Bryant and D. O’Hallaron, Prentice Hall, 3rd ed.

---

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

- (-1)^s M 2^E
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

x86-64, intel, CISC Archiecture.

- ISA(Instruction Set Architecture)
- Assembler VS. Linker
    - binary encoding | resolves references
    - nearly-complete | runtime stack
- movq src, dest
    - imme, reg, mem
    - mem to mem X
- D(Rb, Ri, S) = Mem [ Reg[Rb] + Reg[Ri] *s + D]
- leaq without a memory reference
- addq, subq, imulq, xorq, andq, orq
- salq(dest << src), sarq(dest>>src. sign),shrq(dest>>>src. 0)
- incq ++, decq —, negq -, notq ~

---

## 5. Control

- Registers.
    - Temporary data : %rax, %rbx ...
    - Location of runtime stack : %rsp
    - Location of current code control point : %rip
    - Status of recent tests : CF, ZF, SF, OF
- Test registers cmpq. (a-b)
    - CF(Carry Flag) : unsigned
    - ZF(Zero Flag) : (a == b) ? 1 : 0
    - SF(Sign Flag) : (a-b)<0 ? 1 : 0
    - OF(Overflow Flag) : Overflow
- testq. a&b
    - CF : X
    - ZF : (a&b==0) ? 1:0
    - SF : (a&b < 0) ? 1:0
    - OF : X
- Condition codes. setX
    - setX 1bit reg(%al, %bl)
    - X alter bytes
    - movzbl %al, %eax
- Jumping. jX
- Conditional Moves (X branch)
    - Branches are not good to use pipelines.
    - Conditonal moves don't require control transfer
    - Bad cases
        - compute all cases. Expensive, risky.
        - Bac peformance. unsafe, side-effect free
    - Do-while, while, for
        - init, test, update
    - switch, jmp *.Lx(,%rdi,8)

---

## 6. Procedures

Designer choose the mechanisms of procedures : ABI

- x86-64 use Stack in memory
- stack pointer : %rsp. TOP.
    - Bottom has higher addresses
    - pushq src : %rsp -= 8
    - popq src : %rsp += 8
    - Mem X change
- Passing control
    - call label : jmp to label
    - ret : pop & jmp stack address
- Passing data
    - Argument :%rdi, %rsi, %rdx, %rcx, %r8, %r9 + stack(when needed)
    - Return : %rax
- Managing local data
    - recursion. Local variables
    - Stack Frames
        - use frame to allocate each function : %rbp
        - return, local variables, temp space
    - Caller saved reg : %r10, %r11
    - Callee saved reg : %rbx, %r12 ... %r15. %rbp, %rsp

---

## 7. Data

- Arrays
    - (array start, i, sizeof(type)) access
    - A[], *A, *A[], (*A)[] differences
    - Multidimensional array
        - nest : A[R][C], A[i][j] = Mem[A+(i*C+j)*sizeof(Data)]
        - multi-level : find each start
- Structures
    - block of mem
    - field order important
    - Alignment
        - multiple of largest datatype
        - save space with largest first
- Floating Point
    - %xmmX regs : caller-saved
    - ss : single, sd : double
    - PF : Parity Flag.
    - xorpd set 0

---

## 8. Advanced Topics

- Unions
    - allocate only largest element
    - Little Endian
- Memory layout
    - Stack, heap, data, text, shared libraries
- Buffer overflow
    - Buffer Overflow - Smashed stack
        - buffer is smaller than input, so stack unexpected change
        - wrong return
    - Code injection
        - gets code string & return address to string
    - Prevent
        - Avoid overflow vulnerabilites
            - fgets, strncpy ⇒ size
        - system-level protections
            - Randomized stack offsets
                - allocate random amount of space on stack
                - Shift stack address
            - Nonexecutable code segments
                - stack mark as non executable
        - Stack canaries
            - plac special value "canary" just beyond buffer
            - check the value before exiting
    - ROP
        - overcome randomized stack, non executale code. X canary
        - Use existing code ⇒ gadgets

---

## 9. Optimizations

- Code motion
    - reduce frequency with moving. out of loop
- Reduction in strength
    - +, shift better than * /
- Share common subexpressions
    - reuse
- Optimization Blockers
    - Procedure calls
        - move function call to out loop
    - Memory aliasing
        - reduce memory repeat access
        - reduce compiler memory cheking
- Instruction level parallelism
    - superscalar processor
        - execute multiple instructions in one cycle
        - pipelined
    - loop unrolling
        - reduce number of loops
    - Reassociation
    - branch prediction 2 bit

---

## 10. The memory hierarchy

- RAM (random access memory)
- Dram. cheap, but longer access time
- Disk Drive
    - Dist ⇒ 2 surface platters.
    - rings called tracks. sectors seperated by gaps
    - cylinder form
    - Capacity = (# bytes/sector) * (# sector/track) * (# track/surface) * (# surface/platter) * (# platter/disk)
    - Access time = Tavg seek + Tavg rotation + Tavg transfer
    - Tavg rotation = 0.5 * 1/RPM * 60 sec/min
    - Tavg transfer = 1/RPM * 1/(# sector/track) * 60 sec/min

---
