# Trabalho realizado na Semana #13

### Overview

Neste lab, o foco incide sobre "sniffing" e "spoofing", aspetos essenciais da segurança de redes com implicações significativas na comunicação em rede. 

| Sniffing      | Spoofing |
| ----------- | ----------- |
| Refere-se à prática de intercetar e analisar o tráfego de dados em uma rede para obter informações, podendo ser usada para fins de monitoramento legítimo ou para atividades maliciosas.      | Envolve a falsificação de identidade, onde um dispositivo ou usuário mascara sua identidade real, podendo ser utilizado para enganar sistemas de segurança, realizar ataques de phishing ou manipular informações dentro de uma rede.       |


### Setup

Neste laboratório, foram utilizadas três máquinas ligadas à mesma LAN. 
Todos os ataques foram realizados na máquina do atacante, enquanto as outras máquinas serviam como máquinas de utilizadores. 

A configuração dos contentores acima descritos envolveu o download e descompactação do ficheiro Labsetup.zip, mais concretamente o ficheiro docker-compose.yml para estabelecer o ambiente do laboratório. 

De seguida, foi-nos pedido que utilizassemos o comando "docker ps" para descobrir o ID do contentor, seguido do comando "docker exec" para iniciar uma shell nesse contentor.

Neste laboratório, a máquina do atacante pode ser tanto uma VM quanto um contentor de atacante, conforme especificado no ficheiro Docker Compose. 

O contentor de atacante é configurado de forma diferente, apresentando uma pasta partilhada para uma edição de código conveniente e um modo de anfitrião para permitir a interceção de pacotes. 

O endereço IP atribuído à network é 10.9.0.1, e o ID da redeé 10.9.0.0/24. Foi-nos pedido ainda que encontrássemos o nome da interface de rede da nossa VM. Como podemos observar abaixo, no nosso caso: **br-ca72c778a626**.

<img src="../screenshots/logbook13/setup.png" alt="setup">


## Task 1.1: Sniffing Packets

O objetivo da tarefa era familiarizar-nos com o uso do Scapy em programas Python para interceção de pacotes (sniffing). Foi fornecido um código de exemplo para esse efeito. 

A estrutura do código inclui a importação do Scapy, a definição de uma função para exibir detalhes do pacote e o uso da função 'sniff' com parâmetros específicos de interface e filtro:

```python
pkt = sniff(iface='br-ca72c778a626', filter='icmp', prn=print_pkt)
```

### Task 1.1 A

Com root privilege:

<img src="../screenshots/logbook13/task11_a_rootprivelge.png" alt="task11_a_rootprivelge">

Sem root privilege:

<img src="../screenshots/logbook13/task11_a_norootprivelge.png" alt="task11_a_norootprivelge">

### Task 1.1 B

De seguida, foi-nos pedido que alterassemos o script de python, para que capturasse packets apenas de um determinado tipo.

**1. Capturar apenas o pacote ICMP**

```python
#!/usr/bin/env python3
from scapy.all import *
def print_pkt(pkt):
  pkt.show()
pkt = sniff(iface='br-ca72c778a626', filter='icmp', prn=print_pkt)  
```

**2. Capturar qualquer pacote TCP proveniente de um IP específico e com um número de porta de destino 23**

```python
#!/usr/bin/env python3
from scapy.all import *
def print_pkt(pkt):
  pkt.show()
pkt = sniff(iface='br-ca72c778a626', filter='tcp && src host 10.9.0.6 && dst port 23', prn=print_pkt) 
```

**3. Capturar pacotes provenientes ou destinados a uma sub-rede específica. (Qualquer sub-rede, como 128.230.0.0/16, mas não a sub-rede à qual a VM está conectada)**

```python
#!/usr/bin/env python3
from scapy.all import *
def print_pkt(pkt):
  pkt.show()
pkt = sniff(iface='br-ca72c778a626', filter='128.230.0.0/16', prn=print_pkt)  
```

## Task 1.2: Spoofing ICMP Packets


Através da aplicação da ferramenta de manipulação de pacotes, Scapy, conseguimos ajustar os parâmetros dos pacotes IP conforme necessário. 

O propósito central desta atividade consistiu em criar pacotes IP falsificados com um endereço IP de origem arbitrário. Esses pacotes, que simulavam solicitações de eco ICMP, foram então direcionados a outra máquina virtual na mesma rede. 

A utilização do Wireshark possibilitou a observação quanto à aceitação da solicitação pelo destinatário. Caso fosse aceita, um pacote de resposta de eco seria encaminhado para o endereço IP falsificado. 

A implementação dessa tarefa envolveu a execução do seguinte código:

```python
#!/usr/bin/env python3
from scapy.all import *
a = IP()
a.src = '10.9.0.1'
a.dst = '10.9.0.5'
b = ICMP()
p = a/b
send(p)
```

Ao indicarmos o endereço IP de um dos nossos contentores ('10.9.0.5') e ao gerarmos um pacote ICMP, fomos capazed de enviar esse pacote para o contentor que escolhemos (neste caso o contentor do host A).

<img src="../screenshots/logbook13/task12.png" alt="task12">

Assim, podemos ver no WireShark:

<img src="../screenshots/logbook13/task12_wireshark.png" alt="task12_wireshark">



## Task 1.3: Traceroute


Nesta tarefa, utilizamos o Scapy para estimar a distância, em termos de número de routers, entre máquina virtual (VM) e um destino selecionado. 

O objetivo era criar uma ferramenta semelhante ao traceroute. O processo consistiu em enviar um pacote (de qualquer tipo) para o destino com o campo Time-To-Live (TTL) inicialmente definido como 1. 

O primeiro router descartou o pacote, enviando-nos uma mensagem de erro ICMP indicando que o tempo de vida tinha sido excedido, revelando assim o endereço IP do primeiro router. 

Esse procedimento foi repetido aumentando gradualmente o valor do TTL até que o pacote finalmente atingisse o destino. 

No final, obtemos o seguint script:

```python
#!/usr/bin/env python3
from scapy.all import *

for i in range(1, 30):
	a = IP()
	a.dst = '8.8.8.8'
	a.ttl = i
	b = ICMP()
	a_response = sr1(a/b, timeout=1, verbose=0)
	
	if a_response is not None:
		print("Source:", a_response.src)
		if a_response.src == '8.8.8.8':
			print("Distance: ", a.ttl)
			break  
	else:
		print("No response for TTL =", i)
	time.sleep(1)
```

E aqui está o resultado de alguns testes para diferentes destinos:

| '8.8.8.8' | '8.8.4.4' | '10.9.0.5' | '10.9.0.6' | 
| ----------- | ----------- | ----------- | ----------- |
| <img src="../screenshots/logbook13/task13_8888.png" alt="task13_8888">      | <img src="../screenshots/logbook13/task13_8844.png" alt="task13_8844">      | <img src="../screenshots/logbook13/task13_10905.png" alt="task13_10905">      | <img src="../screenshots/logbook13/task13_10906.png" alt="task13_10906">      |


## Task 1.4: Sniffing and-then Spoofing
