# AWS EC2 – Subnets Pública e Privada

Implementação de duas instâncias EC2 em uma mesma VPC, uma em subnet pública e outra em subnet privada, com validação de conectividade entre elas e com a Internet.

> Atividade prática da disciplina de SDN (Teste 04) – Centro Universitário Senac, executada no AWS Academy Learner Lab.

## Objetivo

Criar e configurar duas instâncias EC2 na mesma VPC e validar, por meio de comandos de rede, o comportamento de cada segmento:

- a instância da **subnet pública** possui IPv4 privado e público e acessa a Internet;
- a instância da **subnet privada** possui somente IPv4 privado e é alcançada de dentro da VPC.

## Serviços e tecnologias

- Amazon EC2 (Amazon Linux 2023, `t3.micro`)
- Amazon VPC
- Subnets públicas e privadas
- Security Groups
- EC2 Instance Connect / SSH
- ICMP (`ping`)

## Arquitetura

| Item | Valor |
|---|---|
| Região | US East (N. Virginia) – `us-east-1` |
| VPC | `10.0.0.0/16` |
| Subnet pública | `10.0.1.0/24` |
| Subnet privada | `10.0.3.0/24` |

| Instância | Subnet | IPv4 privado | IPv4 público |
|---|---|---|---|
| `Teste04-Publica` | Pública | `10.0.1.56` | Atribuído automaticamente |
| `Teste04-Privada` | Privada | `10.0.3.172` | Nenhum |

## Evidências

### 1. Instância privada (somente IPv4 privado)
Estado *Running*, na subnet privada, sem IPv4 público.

![Instância privada](imagens/01-instancia-privada.png)

### 2. Instância pública (IPv4 privado e público)
Estado *Running*, na subnet pública, com IPv4 público atribuído automaticamente.

![Instância pública](imagens/02-instancia-publica.png)

### 3. Acesso à instância pública e ping para a Internet
Acesso à instância pública e `ping -c 4 www.google.com`, com resolução de nome e 0% de perda de pacotes.

![Ping para a Internet](imagens/03-ping-internet.png)

### 4. Instância pública → instância privada
`ping -c 4 10.0.3.172` a partir da instância pública, com respostas e 0% de perda.

![Ping da pública para a privada](imagens/04-ping-publica-para-privada.png)

### 5. Tentativa de SSH da instância pública para a privada
O `ssh ec2-user@10.0.3.172` a partir da instância pública retornou `Permission denied (publickey,...)`: a instância pública não tinha a chave privada para autenticar na privada. Por isso, o ping da privada para o IPv4 privado da pública (`10.0.1.56`) não chegou a ser executado nesta etapa.

![SSH da pública para a privada](imagens/05-ssh-publica-para-privada.png)

## Resultados

- Instâncias criadas nas subnets corretas da mesma VPC. 
- Instância pública com acesso à Internet via ICMP. 
- Comunicação interna da VPC (pública → privada) funcionando. 
- Acesso SSH à instância privada a partir da pública: não concluído (falta de chave). 

## Aprendizados

- A diferença prática entre subnet pública e privada: a privada não recebe IPv4 público e só é alcançada de dentro da VPC.
- Dentro da mesma VPC, as instâncias se comunicam pelo IPv4 privado, desde que o Security Group permita o tráfego (ICMP e SSH).
- Para acessar uma instância privada por *bastion host* (salto), a chave de acesso precisa estar disponível na origem, por exemplo com `ssh -A` (encaminhamento do agente SSH) ou outro método de autenticação.

## Próximos passos

- Refazer o teste de SSH com encaminhamento de agente e registrar o `ping` da instância privada para `10.0.1.56`.
- Avaliar um NAT Gateway para dar saída à Internet à instância privada sem expô-la.

> As instâncias foram encerradas ao final do teste para evitar consumo de créditos do Learner Lab. O ID da conta AWS foi ocultado nas capturas de tela.


