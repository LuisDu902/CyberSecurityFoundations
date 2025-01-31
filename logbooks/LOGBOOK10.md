# Trabalho realizado na Semana #10

## Task 1: Frequency Analysis

A nossa primeira tarefa foi decifrar uma mensagem criptografada usando uma cifra monoalfabética. Tal como diz no enunciado, este tipo de cifra é vulnerável a uma técnica chamada de 'frequency analysis', sendo por isso a técnica que nos é pedida para realizar neste exercício.

Esta análise consiste em contar a ocorrência de cada letra do texto cifrado e compará-las com as diferentes frequências com que certas letras ou combinações aparcem em qualquer porção de texto do idioma do texto original (neste caso: inglês).

Numa fase inicial fomos tentando de diveras formas substituir as letras de modo a obtermos o resultado esperado. Para isso procuramos tabelas com as combinações de caracteres mais comuns para conseguirmos compreender quais as substituições mais provaveis de funcionar.

Essas combinações foram comparadas com as combinações mais prováveis baseando-nos no ficheiro 'freq.py' que nos foi fornecido.

No final, o resultado obtido foi o seguinte (a troca de letras foi feita por partes na mesma ordem que a versão final):

```bash
$ tr 'ytnvupmxqahbrizdlcgesfkowj' 'THEANDIOSCRFGLUYWMBPKVXJZQ' < ciphertext.txt > out.txt

```

Após a descodificação obtivemos o texto contido no ficheiro [out.txt](textfiles/logbook10_out.txt).

## Task 2: Encryption using Different Ciphers and Modes

Nesta tarefa, o objetivo é explorar vários algoritmos e modos de criptografia. Para isso, decidimos criar um ficheiro 'plain.txt' com um texto de exemplo ("second task message example") no qual aplicamos os diferentes modos de criptografia, usando o código fornecido no guião:

```bash
$ openssl enc -<ciphertype> -e -in plain.txt -out cipher.txt -K 00112233445566778889aabbccddeeff -iv 0102030405060708
```

Decimos também usar os três tipos de encriptação que nos foram fornecidos no guião de trabalho: 

<table>
    <tr>
        <th>'-aes-128-cbc'</th>
        <th><img src="../screenshots/logbook10/-aes-128-cbc.png" alt="Image -aes-128-cbc"></th>
    </tr>
    <tr>
        <th>'-bf-cbc'</th>
        <th><img src="../screenshots/logbook10/-bf-cbc.png" alt="Image -bf-cbc"></th>
    </tr>
    <tr>
        <th>'-aes-128-cfb'</th>
        <th><img src="../screenshots/logbook10/-aes-128-cfb.png" alt="Image -aes-128-cfb"></th>
    </tr>
</table>
    
## Task 3: Encryption Mode – ECB vs. CBC

### ECB (Electronic Code Book)

Para encriptar a imagem neste modo, recorremos ao openssl enc da seguinte forma:

```bash
$ openssl enc -aes-128-ecb -e -in pic_original.bmp -out ecb.bmp -K 00112233445566778889aabbccddeeff
```
<img src="../screenshots/logbook10/ecb_code.png" alt="Image ecb_code">

<table>
    <tr>
        <th><img src="../screenshots/logbook10/original.png" alt="original"></th>
        <th><img src="../screenshots/logbook10/ecb.png" alt="ecb"></th>
    </tr>
</table>

Ao observar ambas as imagens, percebemos claramente que ficaram bastante parecidas. As formas das imagens mantiveram-se semelhantes e identificá-las foi bastante simples, visto que este modo de encriptação apenas cifra blocos iguais de texto produzindo blocos de texto cifrado também semelhantes.

### CBC (Cipher Block Chaining)

Para encriptar a imagem neste modo, recorremos novamente ao openssl enc da seguinte forma:

```bash
$ openssl enc -aes-128-cbc -e -in pic_original.bmp -out cbc.bmp -K 00112233445566778889aabbccddeeff -iv 0102030405060708
```
<img src="../screenshots/logbook10/cbc_code.png" alt="Image cbc_code">

<table>
    <tr>
        <th><img src="../screenshots/logbook10/original.png" alt="original"></th>
        <th><img src="../screenshots/logbook10/cbc.png" alt="cbc"></th>
    </tr>
</table>

Ao contrário da experiência anterior, neste caso torna-se impossível detetar qualquer semelhança entre as duas imagens. Isto ocorre uma vez que cada bloco de texto é combinado com o bloco cifrado anterior impedindo assim a identificação de padrões na imagem encriptada.

No final, tal como pedido, repetimos todo o experimento para outra imagem à nossa escolha e os resultados mantiveram-se semelhantes.
