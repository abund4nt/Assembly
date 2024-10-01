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

## Operations

Most used operations.

- `add` some data together
- `sub` subtract some data
- `mul` multiply some data
- `div` divide some data
- `mov` move data into our of storage
- `com` compare two pieces of data with each other
- `test` some other properties of data
- `pop` retrieves the value from the top of the stack and stores it into a specified register

![](https://i.imgur.com/W9iqMkP.png)

Example code to analyze which has some of the mentioned operations.

```asm
<+0>:     endbr64 
<+4>:     push   rbp
<+5>:     mov    rbp,rsp
<+8>:     mov    DWORD PTR [rbp-0x14],edi
<+11>:    mov    QWORD PTR [rbp-0x20],rsi
<+15>:    mov    DWORD PTR [rbp-0xc],0x9fe1a
<+22>:    mov    DWORD PTR [rbp-0x8],0x4
<+29>:    mov    eax,DWORD PTR [rbp-0xc]
<+32>:    imul   eax,DWORD PTR [rbp-0x8]
<+36>:    add    eax,0x1f5
<+41>:    mov    DWORD PTR [rbp-0x4],eax
<+44>:    mov    eax,DWORD PTR [rbp-0x4]
<+47>:    pop    rbp
<+48>:    ret
```

## Memory

- Memory -> Registers
- Memory -> Disk
- Memory -> Network
- Memory -> Network
- Memory -> Video Card

There is to much memory to name every location.

Registers and immediates can be `push`ed onto the stack to save values.

```asm
mov rax, 0xc001ca75
push rax
push 0xb0bacafe
push rax
```

Then.

```
+----------+                                                              
|          |                                                              
|          |                                                              
|  STACK   |                                                              
|          |                                                              
|          |                                                              
|          |                                                              
|----------|                                                                                                                       
| c001ca75 |                                                             
|----------|                                                              
|          |                                                              
| b0bacafe |                                                             
|----------|                                                              
|          |                 
| c001ca75 |                                                              
|          |                                                              
+----------+
```

Values can be `pop`ped back off the stack.

```asm
pop rbx ; sets rbx to 0xc001ca75
pop rcx ; sets rcx to 0xb0bacafe
```

Then.

```
+----------+                                                              
|          |                                                              
|          |                                                              
|  STACK   |                                                              
|----------|                                                              
|          |                 
| c001ca75 |                                                              
|          |                                                              
+----------+
```

The CPU knows where the stack is because its address is stored in `rsp`. Example.

- rsp = `0x7f01f3453050`

```
+----------+                                                              
|          |                                                              
|          |                                                              
|  STACK   |                                                              
|----------|                                                              
|          |                 
| c001ca75 |                                                              
|          |                                                              
+----------+
```

The instruction applies.

```asm
push 0xb0bacafe
```

- rsp = `0x7f01f3453048`

Then the stack.

```
+----------+                                                              
|          |                                                              
|          |                                                              
|  STACK   |                                                              
|----------|                                                              
|          |                 
| b0bacafe |                                                              
|          |
|----------|                                                              
|          |                 
| c001ca75 |                                                              
|          |
+----------+
```

The instruction applies.

```asm
pop rcx
```

- rsp = `0x7f01f3453050`

Then the stack.

```
+----------+                                                              
|          |                                                              
|          |                                                              
|  STACK   |                                                              
|----------|                                                              
|          |                 
| c001ca75 |                                                              
|          |                                                              
+----------+
```

> `push` decreases rsp, `pop` increases it.

This will load the 64-bits value stored at memory address 0x12345 into `rbx`.

```asm
mov rax, 0x12345
mov rbx, [rax]
```

This will store the 64-bit value in `rbx` into memory at address 0x133337.

```asm
mov rax, 0x133337
mov [rax], rbx
```

This is equivalent to push `rcx`.

```asm
sub rsp, 4
mov [rsp], rcx
```

Load 64 bits from addr 0x12345 and store the lower 32 bits to addr 0x133337

```asm
mov rax, 0x12345
mov rbx, [rax]
mov rax, 0x133337
mov [rax], ebx
```

Store 8 bits from `ah` to addr 0x12345

```asm
mov rax, 0x12345
mov rbx, [ah]
```
Data on most modern systems is stored *backwards*, in *little endian*.

```asm
mov eax, 0xc001ca75 ; sets rax to c0 | 01 | ca | 75
mov rcx, 0x10000
mov [rcx], eax ; stores data as 75 | ca | 01 | c0
mov bh, [rcx] ; reads 0x75
```

`lea` is one of the few instructions that can directly access the rip register.

```asm
lea rax, [rip] ; load the address of the next instruction into rax
lea rax, [rip+8] ; the address of the instruction, plus 8 bytes.
```

You can also use move to read directly from those locations.

```asm
mov rax, [rip] ; load 8 bytes from the location pointend to by the address of the next instruction
```

Or even write there.

```asm
mov [rip], rax
```

You can also write immediate values. However, you must specify their size.

```asm
mov rax, 133337
mov DWORD PTR [rax], 0x1337
```

## Registers first arguments

- `rdi`: First argument
- `rsi`: Second argument
- `rdx`: Third argument
- `rcx`: Four argument
- `r8`: Fifth argument
- `r9`: Sixth argument
