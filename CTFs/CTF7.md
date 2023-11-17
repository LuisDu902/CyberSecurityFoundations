# CTF Semana #5 (Format string)

## Desafio 1

### Recolha de informação

Começamos por analisar os ficheiros disponibilizados na plataforma CTF: um executável (`program`), o código fonte (`main.c`) e um script em python (`exploit_example.py`).

De seguida, corremos o comando `checksec program`, onde obtivemos o seguinte output:

```bash
Arch:     i386-32-little
RELRO:    Partial RELRO
Stack:    Canary found
NX:       NX enabled
PIE:      No PIE (0x8048000)
```

Ao analisar apercebemo-nos que:
- a arquitetura do ficheiro é x86 (Arch)
- existe um canário a proteger o return address (Stack)
- a stack não tem permisssão de execução (NX)
- as posições do binário não estão randomizadas (PIE)

### Reconhecimento da vulnerabilidade

No `main.c`, verificamos que a funcão `scanf` guarda o input introduzido pelo utilizador num buffer local de 32 bytes, sendo impresso por um `printf` sem argumentos adicionais.

```c
scanf("%32s", &buffer);
printf("You gave me this: ");
printf(buffer);
```

Assim sendo, concluimos que podemos explorar as vulnerabilidades associadas ao format string se o nosso input tiver format specifiers.


### Exploração da vulnerabilidade

Primeiramente, recorremos ao `gdb` para descobrir o endereço de memória da variável onde se encontra a flag:

<img src="../screenshots/ctf7/d1_flag.PNG" alt="flag">

No `exploit-example.py`, que interage com o serviço, mudamos a seguinte linha de código para que injetasse o endereço de memória da flag e que desse print `%s`:

```py
p.recvuntil(b"got:")
p.sendline(b"\x60\xC0\x04\x08%s")
p.interactive()
```

Ao executar o script, obtivemos a flag como o esperado:

<img src="../screenshots/ctf7/flag1.PNG" alt="flag">


