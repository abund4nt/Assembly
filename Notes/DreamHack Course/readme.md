Answers and explanation of the questionnaire done at the [DreamHack Assembly course](https://dreamhack.io/lecture/roadmaps/all).

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

Vemos que el registro `rbx `se inicializa con `0x401A40`. Mas abajo en la línea 1 la instruccion `mov rax, [rbx+8]` carga el valor de la dirección `0x401A48` (que es `0x0000000000C0FFEE`) en `rax`. En la línea 2 `lea rax, [rbx+8]` actualiza rax con la dirección 0x401A48 en lugar del contenido. Por lo tanto al finalizar la ejecución hasta la línea 2, el valor en rax sera `0x401A48`.
