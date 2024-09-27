## Registers

Generaral purpose registers.

- amd64 = rax, rcx, rdx, rbx, rsp, rbp, rsi, rdi, r8, r9, r10, r11, r12, r13, r14, r15
- x86 = eax, ecx, edx, ebx, esp, ebp, esi, edi

On a 64-bit architectura (most) registers will hold 64 bits (8 bytes).

```
10110110
11011110
01111101
10110000
00111100
11110000
01000101
```

You can also `mov` data between registers. Ej: this set both `rax` and `rbx` to 0x539.

```
mov rax, 0x539
mov rbx, rax
```

Specials registers

- rip = Instruction Pointer
- rsp = Stack Pointer

Some other registers are, by convention, used for important things.
