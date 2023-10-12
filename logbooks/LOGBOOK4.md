# Trabalho realizado na Semana #4

## 2.1 Task 1: Manipulating Environment Variables

Nesta tarefa, estamos a utilizar o Bash numa seed account para aprender sobre a gestão de variáveis de ambiente. As variáveis de ambiente armazenam informações para os processos de sistema Unix-based. O objetivo principal é compreender como manipular estas variáveis com comandos Bash.

Para começar, utilizamos comandos como **printenv** e **env** para visualizar todas as variáveis de ambiente atuais. Além disso,exibimos variáveis específicas, como o diretório de trabalho atual (PWD), utilizando comandos como **printenv PWD**.
<table>
  <tr>
    <td><img src="../screenshots/logbook4/2.1__1_.png" alt="Image 2.1_1"></td>
    <td><img src="../screenshots/logbook4/2.1__2_.png" alt="Image 2.1_2"></td>
  </tr>
</table>


A segunda parte desta tarefa envolve o uso dos comandos export e unset para modificar variáveis de ambiente.

- **export** é usado para definir ou criar novas variáveis de ambiente.
- **unset** é usado para remover ou desativar uma variável de ambiente, excluindo-a eficasmente do ambiente.

<img src="../screenshots/logbook4/2.1__3_.jpg" alt="Image 2.1_3">


## 2.2 Task 2: Passing Environment Variables from Parent Process to Child Process

Nesta tarefa, investigamos como os processos filhos herdam as variáveis de ambiente dos seus pais em sistemas operativos Unix (usando a chamada ao sistema fork()).

Quando se invoca a função fork() no Unix, cria-se um novo processo duplicando o processo pai. O processo filho é quase idêntico ao pai, mas nem todas as características são herdadas.

O objetivo da tarefa é determinar se as variáveis de ambiente do processo pai são herdadas pelo processo filho quando se utiliza o fork().

<img src="../screenshots/logbook4/2.2__1_.png" alt="Image 2.2_1">

Quando usamos o comando diff no Passo 3 para comparar os dois arquivos de saída, não são reveladas diferenças, porque ambos os arquivos devem ser idênticos, o que ilustra que o processo filho herda as variáveis de ambiente do processo pai.


## 2.3 Task 3: Environment Variables and execve()

Nesta tarefa, estamos a investigar como as variáveis de ambiente se comportam quando um novo programa é executado utilizando a função **execve()**.

Ao fazer isto, pretendemos compreender se as variáveis de ambiente do processo de chamada são herdadas pelo novo programa.

<img src="../screenshots/logbook4/2.3__1_.png" alt="Image 2.3_1">

No primeiro passo, quando o **execve()** foi chamado com o argumento environ definido como NULL, o novo programa não herdou as variáveis de ambiente do processo chamador, o que indica que as variáveis de ambiente não são herdadas automaticamente por padrão.

No entanto, no segundo passo, quando o **execve()** foi chamado com o argumento environ **explicitly set** como a variável environ, o novo programa herdou as variáveis de ambiente.


## 2.5 Task 5: Environment Variable and Set-UID Programs

Inicialmente, criámos um programa para apresentar as variáveis de ambiente dentro do processo atual. Posteriormente, compilámos o programa, alterámos a sua propriedade para o utilizador root e transformámo-lo num programa Set-UID seguindo as instruções fornecidas. Na linha de comandos, utilizámos o comando "export" em três cenários diferentes descritos no tutorial do laboratório.

<table>
  <tr>
    <td><img src="../screenshots/logbook4/2.5__1_.png" alt="Image 2.1_1"></td>
    <td><img src="../screenshots/logbook4/2.5__2_.png" alt="Image 2.1_2"></td>
  </tr>
</table>

Após executarmos o programa, confirmámos que as variáveis de ambiente que tínhamos previamente definido estavam presentes na lista de variáveis de ambiente do programa.

No entanto, uma exceção importante foi LD_LIBRARY_PATH, que foi omitida. Esta omissão deve-se à capacidade do LD_LIBRARY_PATH de especificar um caminho onde o programa procura bibliotecas dinâmicas partilhadas.

Permitir modificações ilimitadas nesta variável poderia criar riscos de segurança, ao possibilitar a execução de programas potencialmente prejudiciais que substituíssem bibliotecas essenciais. Portanto, por motivos de segurança, o LD_LIBRARY_PATH foi omitido para evitar tais vulnerabilidades.


## 2.6 Task 6: The PATH Environment Variable and Set-UID Programs

Em primeiro lugar, começamos por criar um ficheiro com o nome "2_6.c" no qual implementamos o código conforme especificado na tarefa. Em seguida, desenvolvemos o nosso código malicioso num ficheiro com o nome "ls.c." Depois, compilamos e executamos o nosso código malicioso.

Posteriormente, alteramos a propriedade para root e transformamo-lo num programa Set-UID.

Por fim, modificamos a variável de ambiente PATH para apontar para o diretório onde o código malicioso está localizado.

Como resultado, quando o comando "ls" é executado, o código produzido será o código malicioso em vez do "ls" do sistema.

<img src="../screenshots/logbook4/2.6__1_.png" alt="Image 2.6">
