# SQL Injection

Inicialmente, damos setup ao ambiente de trabalho necessário para resolver este lab, preparando os containers do docker e a base de dados.

### Task 1

Executamos a primeira tarefa para nos familiarizarmos com esta base de dados MySQL "sqllab_users". Primeiro, acedemos à base de dados e depois executamos um comando para mostrar as informações de perfil da Alice.

![](https://lh7-us.googleusercontent.com/hBOyjbUjxoO0yuMzEx1B-EiVJ85VBTBHfeQEpzwUrtt_Ul-DFKsAf1NS5W576EZqON2e1UmfOMxtRsHwk21DjbOYW5PZnI4XV-jzBXt0RvJJ0eVF5mw-v0NMQR64ZWf0GQh8XTZMpecuOugXdLNCETM)

### Task 2.1

Como neste código SQL as strings não são sanitizadas, inserimos o nome de utilizador: **"admin' OR ''='"** que através da declaração **OR**, torna a verificação da senha dispensável, uma vez que a primeira condição é satisfeita. Assim, foi possível aceder à conta de administrador.

![](https://lh7-us.googleusercontent.com/oJvWOGKcuXpEDTM-7aSOKV-fBZ8RWPefptx14Nq8Z-dZs8hCDqtqgFBuaTDg7A933E2ToO2oHFdYX0nU_49iJbcB9mEAchSb0Q8zCpgEE9HgHICt5zcVM0drneD3keDZYR8W_ZD7JdnPI3Y02Q5XDhQ)

### Task 2.2

Nesta tarefa repetimos o processo descrito anteriormente no terminal usando o comando:



```html
curl "http://www.seed-server.com/unsafe_home.php?username=admin%27%23&Password="
```

![](https://lh7-us.googleusercontent.com/hjKBuznUI12C2Vj5zpFDpQgs2heYqTUq9GjpzyAHL-IvYZ7ZM4GI1OYU6kGZutHmMs5RnnkuGAGx_H_cKk7SO4dEUaDijbxWGEC5IsAAgxOZZ5mxBW8xC7XLXbr2-ha0kr2pvDgkDT_sdOXzmJNOteU)

### Task 2.3

Nesta tarefa tentamos fazer um ataque através do user input que transforma uma declaração em duas, através de **";"** no código.\
Infelizmente, existe uma proteção neste MySQL do PHP que nos impediu de concluir o ataque com sucesso.

### Task 3.1

Como a página não sanitiza o user input fornecido, podemos injetar a seguinte linha para atualizar o salário de um utilizador:

```html
', salary='9999999

```

![](https://lh7-us.googleusercontent.com/cuwacw7mLcpw6qtcVG8cVwnTHFSvynK03Fo5qScK2rvKrKwiXqRtkH3biGczbyf0ricLDX8vPIDwdnP1t5Ex7HcRO0S217cOqSHK6Q7hWOGbZByvg9DM3q3YQJQXWICV72CA9i99L55LLJOuDY3gjlY)

### Task 3.2

Nesta tarefa, o objetivo foi alterar o salário de outro utilizador. Foi repetida a técnica anterior e foi injetado o seguinte código:

```html
', salary='1' where name='Boby';#
```

Foi utilizado o **#** para comentar o código posterior e assim ser o nosso WHERE a ser executado.

![](https://lh7-us.googleusercontent.com/hMj0SdWddo1MoPWo4WTH18Dv5fhuFHTFV3iEHA_aENWOugPB9MmNVLcUpLJbemIdb3X5KqZvdF8tZngDTS6ksGN7bcrox5PJmhWgJvO2W_inNNV8BJylO-XU-MdwCIzJyUaPzWZOf-g60G1f7uF1bSw)

### Task 3.3

Nesta tarefa o objetivo é alterar a palavra passe do Boby. 

Como as as palavras passe são encriptadas, temos que utilizar um **SHA 1 decoder** para obter a hash da password introduzida.

Depois apenas é necessário injetar o código seguinte:
```html
Password='7110eda4d09e062aa5e4a390b0a572ac0d2c0220' where Name = 'Boby' #
```


O valor colocado na string, é a hash da nova password.