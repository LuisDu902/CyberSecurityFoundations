# Trabalho realizado nas Semanas #2 e #3

## Identificação

Uma vulnerabilidade nos recursos de acesso remoto VPN possibilitou a atividade imprópria de hackers sem autenticação. 

Assim, foram capazes de obter informações sensíveis de utilizadores.

Esta vulnerabilidade afetou dispositivos Cisco, caso estes estivessem a rodar uma release vulnerável do “Cisco ASA” ou do “FTD software”.

## Catalogação

A vulnerabilidade foi explorada em abril de 2023 durante a resolução de um caso de suporte da Cisco TAC. 

Ela permitia estabelecer uma sessão VPN SSL sem a necessidade de um cliente (apenas quando executando o Cisco ASA Software Release 9.16 ou versões anteriores). 

Após ser reportada pela Rapid7, recebeu uma avaliação de gravidade de 9.1/10 no CVSS (Common Vulnerability Scoring System).

## Exploit

Esta vulnerabilidade pode ser explorada de duas formas. A primeira, usando um ataque brute-force.

Isto significa que o atacante pode tentar várias combinações de credenciais até acertar e poder estabelecer uma sessão VPN de acesso remoto não autorizada.
       
Outra forma de explorar a vulnerabilidade é caso um invasor esteja autenticado, mesmo que não esteja autorizado.

Tal significa que o invasor pode acessar à rede VPN sem a necessária autorização, representando um risco de segurança significativo.

## Ataques

Em março de 2023, os atacantes responsáveis ​​pelo ransomware Akira utilizaram diferentes estratégias de extorsão de modo a explorar esta vulnerabilidade. 

Assim sendo, operaram um site na rede TOR (The Onion Router), onde listaram vítimas e quaisquer informações roubadas, caso as exigências de resgate não fossem atendidas.

Consequentemente, as vítimas foram orientadas a entrar em contacto direto com os invasores por meio deste site, usando um identificador exclusivo encontrado na mensagem de resgate que recebem, para iniciar negociações.
