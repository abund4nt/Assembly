Answers and explanation of the questionnaire done at the [DreamHack Assembly course](https://dreamhack.io/lecture/roadmaps/all).

# Quiz: x86 Assembly 1

Q1. **Given the registers, memory, and code, what is the value stored in rax when the code is executed up to line 1?**

```asm
[Register]
rbx = 0x401A40

=================================

[Memory]
0x401a40 | 0x0000000012345678
0x401a48 | 0x0000000000C0FFEE
0x401a50 | 0x00000000DEADBEEF
0x401a58 | 0x00000000CAFEBABE
0x401a60 | 0x0000000087654321

=================================

[Code]
1: mov rax, [rbx+8]
2: lea rax, [rbx+8]
```

Vemos que el registro `rbx` tiene el valor de `0x401A40`, mas abajo la instruccion `mov` le suma `8` bytes al valor del registro, dejandolo en `0x401a48`.

```shell
>>> hex(0x401a40 + 8)
'0x401a48'
```

El valor `0x401a48` corresponde a `0x0000000000C0FFEE`. La respuesta correcta es `0xC0FFEE`.

Q2. **What is the value in rax when the code is executed up to line 2?**

```asm
[Register]
rbx = 0x401A40

=================================

[Memory]
0x401a40 | 0x0000000012345678
0x401a48 | 0x0000000000C0FFEE
0x401a50 | 0x00000000DEADBEEF
0x401a58 | 0x00000000CAFEBABE
0x401a60 | 0x0000000087654321

=================================

[Code]
1: mov rax, [rbx+8]
2: lea rax, [rbx+8]
```

Vemos que el registro `rbx` se inicializa con `0x401A40`. Mas abajo en la línea 1 la instruccion `mov rax, [rbx+8]` carga el valor de la dirección `0x401A48` (que es `0x0000000000C0FFEE`) en `rax`. En la línea 2 `lea rax, [rbx+8]` actualiza rax con la dirección 0x401A48 en lugar del contenido. Por lo tanto al finalizar la ejecución hasta la línea 2, el valor en rax sera `0x401A48`.

Q3. **Given the registers, memory, and code, what is the value stored in rax when the code is executed up to line 1?**

```asm
[Register]
rax = 0x31337
rbx = 0x555555554000
rcx = 0x2

=================================

[Memory]
0x555555554000| 0x0000000000000000
0x555555554008| 0x0000000000000001
0x555555554010| 0x0000000000000003
0x555555554018| 0x0000000000000005
0x555555554020| 0x000000000003133A

==================================

[Code]
1: add rax, [rbx+rcx*8]
2: add rcx, 2
3: sub rax, [rbx+rcx*8]
4: inc rax
```

El registro `rax` se inicializa con el valor de `0x31337`, `rbx` con el valor de `0x555555554000` y `rcx` con `0x2`. En la linea una suma con la instruccion `add` al registro `rax` el valor de `rbx + rcx * 8`.

```shell
>>> rax = 0x31337
>>> rbx = 0x555555554000
>>> rcx = 0x2
>>> hex(rbx + rcx * 8)
'0x555555554010'
```

El resultado `0x555555554010` corresponde al valor `0x0000000000000003`.

```
[Memory]
0x555555554000| 0x0000000000000000
0x555555554008| 0x0000000000000001
0x555555554010| 0x0000000000000003
0x555555554018| 0x0000000000000005
0x555555554020| 0x000000000003133A
```

Entonces la operacion seria `0x31337 + 0x0000000000000003`.

```shell
>>> hex(0x31337 + 0x0000000000000003)
'0x3133a'
```

Q4. **What is the value stored in rax when the code is executed up to line 3?**

```asm
[Register]
rax = 0x31337
rbx = 0x555555554000
rcx = 0x2

=================================

[Memory]
0x555555554000| 0x0000000000000000
0x555555554008| 0x0000000000000001
0x555555554010| 0x0000000000000003
0x555555554018| 0x0000000000000005
0x555555554020| 0x000000000003133A

==================================

[Code]
1: add rax, [rbx+rcx*8]
2: add rcx, 2
3: sub rax, [rbx+rcx*8]
4: inc rax
```


Q12. **Let's assume that the program terminates when it jumps to the end label. When the program terminates, what string do the ASCII characters from the data at addresses 0x400000 to 0x400019 form?**

```asm
[Register]
rcx = 0
rdx = 0
rsi = 0x400000

=======================

[Memory]
0x400000 | 0x67 0x55 0x5c 0x53 0x5f 0x5d 0x55 0x10
0x400008 | 0x44 0x5f 0x10 0x51 0x43 0x43 0x55 0x5d
0x400010 | 0x52 0x5c 0x49 0x10 0x47 0x5f 0x42 0x5c
0x400018 | 0x54 0x11 0x00 0x00 0x00 0x00 0x00 0x00

=======================

[code]
1: mov dl, BYTE PTR[rsi+rcx]
2: xor dl, 0x30
3: mov BYTE PTR[rsi+rcx], dl
4: inc rcx
5: cmp rcx, 0x19
6: jg end
7: jmp 1
```

```python
data = [
    0x67, 0x55, 0x5c, 0x53, 0x5f, 0x5d, 0x55, 0x10,
    0x44, 0x5f, 0x10, 0x51, 0x43, 0x43, 0x55, 0x5d,
    0x52, 0x5c, 0x49, 0x10, 0x47, 0x5f, 0x42, 0x5c,
    0x54, 0x11, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00
]

for byte in data:
    print(chr(byte ^ 0x30), end="")
```

