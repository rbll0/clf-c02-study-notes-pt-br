---
title: VPC e Networking
subtitle: Seção 15 do CLF-C02 — VPC, subnets, gateways, firewalls e conexões híbridas
cols: 3
exam: Domínio 3 · Tecnologia e serviços (34%)
---
A **VPC** é a **sua rede privada** dentro da AWS. Você escolhe a faixa de IPs, divide em **subnets** e controla o que entra e sai. Entenda o caminho do tráfego: **internet → gateway → subnet → instância**.

## A Anatomia de uma VPC {span=2}
```flow LR
(Internet) -> *Internet Gateway: entra e sai
Internet Gateway -> Subnet pública: tabela de rotas
Subnet pública -> Servidor web: Security Group
Subnet pública -> NAT Gateway: fica aqui
NAT Gateway -> Subnet privada: saída para a internet
Subnet privada -> Banco de dados: sem IP público
group VPC (uma região): Internet Gateway, Subnet pública, NAT Gateway, Subnet privada, Servidor web, Banco de dados
```
- A VPC é **regional**. Ela cobre **todas as AZs** da região.
- Cada **subnet** vive em **uma AZ**.
- A **Default VPC** já vem em cada região, com subnets **públicas**.

## B Blocos básicos {span=1}
| Peça | Função |
|---|---|
| **VPC** | Rede isolada, com uma faixa **CIDR** |
| **Subnet** | Pedaço da VPC, em **1 AZ** |
| **Route table** | Diz **para onde** vai o tráfego |
| **ENI** | Placa de rede virtual da instância |

## C Subnet pública vs privada {span=3}
| | Subnet **pública** | Subnet **privada** |
|---|---|---|
| Rota para o **Internet Gateway** | Sim | Não |
| Acesso vindo da internet | Possível, com IP público | **Não** |
| Saída para a internet | Direto pelo Internet Gateway | Pelo **NAT Gateway** |
| Uso típico | Servidores web, load balancers | Bancos de dados, servidores de aplicação |

## D Internet Gateway e NAT Gateway {span=2}
- O **Internet Gateway (IGW)** liga a VPC à **internet**. Ele escala sozinho e tem **alta disponibilidade**.
- O **NAT Gateway** deixa instâncias **privadas** acessarem a internet (por exemplo, para baixar atualizações). A internet **não** consegue iniciar uma conexão de volta.
- O NAT Gateway é **gerenciado pela AWS** e fica em uma **subnet pública**, em **uma AZ**. Para alta disponibilidade, crie um por AZ.
- A **NAT Instance** é a versão antiga. Você a gerencia no EC2. Prefira o NAT Gateway.
- **IPv6**: o IPv6 não usa NAT. O **Egress-only Internet Gateway** permite só **saída** para instâncias com IPv6.

## E Route tables
- Cada subnet usa **uma** route table.
- Cada rota liga um **destino** (CIDR) a um **alvo** (IGW, NAT, peering, endpoint).
- A rota `0.0.0.0/0 → IGW` **torna a subnet pública**.
- A rota `0.0.0.0/0 → NAT` dá **saída** a uma subnet privada.

## F Security Group vs NACL {span=3}
| | **Security Group** | **NACL** (Network ACL) |
|---|---|---|
| Nível | **Instância** (ENI) | **Subnet** |
| Estado | **Stateful**: a resposta volta sozinha | **Stateless**: libere ida **e** volta |
| Regras | Só **allow** | **Allow** e **deny** |
| Avaliação | Avalia **todas** as regras | Segue a **ordem** dos números, a primeira que casa vale |
| Padrão | **Nega** a entrada. Libera toda a saída | A NACL **padrão** libera tudo. Uma NACL **nova** nega tudo |

- Os dois **trabalham juntos**: a NACL é a **primeira** barreira da subnet e o Security Group é a **última**, na instância.
- Para **bloquear um IP específico**, use a **NACL**. O Security Group não tem regra de deny.

## G Observar e proteger o tráfego
- **VPC Flow Logs**: registram o **tráfego de IP** (origem, destino, porta, aceito ou recusado) de VPC, subnet ou ENI.
- Eles enviam os dados ao **CloudWatch Logs**, ao **S3** ou ao **Data Firehose**.
- Eles ajudam a **depurar** conexões e a **detectar** acessos suspeitos.
- Eles **não** mostram o conteúdo dos pacotes.

## H Conectar redes {span=3}
| Serviço | O que faz | Detalhe |
|---|---|---|
| **VPC Peering** | Liga **2 VPCs** pela rede da AWS | **Não é transitivo**. Os CIDRs não podem se **sobrepor**. Funciona entre contas e regiões |
| **VPC Endpoint** | Acessa serviços AWS **sem sair para a internet** | **Gateway**: só **S3** e **DynamoDB**, sem custo. **Interface**: outros serviços, via **PrivateLink** |
| **PrivateLink** | Expõe um serviço a **outras VPCs**, de forma **privada** | Sem peering, sem IGW, sem sobreposição de CIDR |
| **Transit Gateway** | **Hub** central que liga **milhares** de VPCs e redes on-premises | Topologia **estrela**. Substitui uma malha de peerings |
| **Site-to-Site VPN** | Liga o datacenter à VPC por **túnel criptografado** pela **internet** | Rápido de configurar. Usa um **Virtual Private Gateway** e um **Customer Gateway** |
| **Direct Connect (DX)** | **Cabo dedicado**, privado, do datacenter à AWS | Banda **alta e estável**, latência baixa. Leva **semanas ou meses** para instalar. warn **Não é criptografado** por padrão |
| **Client VPN** | Conecta **usuários** (notebooks) à VPC ou à rede on-premises | Baseado em **OpenVPN** |

## I Caminhos híbridos {span=2}
```flow LR
Datacenter -> *Site-to-Site VPN: túnel criptografado
Site-to-Site VPN -> VPC: pela internet
Datacenter --> Direct Connect: cabo dedicado
Direct Connect --> VPC: rede privada
```
- **VPN**: barato e **rápido**. A qualidade segue a internet.
- **Direct Connect**: caro e **lento de contratar**. Desempenho **previsível**.
- Para ter **os dois**: use a VPN como **backup** do Direct Connect.

## J Quando usar cada um
- Duas VPCs conversam: **Peering** (poucas) ou **Transit Gateway** (muitas).
- Instância privada precisa de internet: **NAT Gateway**.
- App acessa o S3 **sem internet**: **Gateway Endpoint**.
- Datacenter à AWS, **rápido e barato**: **Site-to-Site VPN**.
- Datacenter à AWS, **banda alta e privada**: **Direct Connect**.
- Funcionário remoto: **Client VPN**.

```callout warn Pegadinhas de prova
- **Security Group** = instância, **stateful**, só allow. **NACL** = subnet, **stateless**, allow e deny.
- "Bloquear um IP" = **NACL**.
- "Subnet pública" = tem rota para o **Internet Gateway**.
- "Instância privada baixa atualizações" = **NAT Gateway** (em subnet **pública**).
- "Peering **não** é transitivo." Para muitas VPCs, use **Transit Gateway**.
- "S3 ou DynamoDB sem internet" = **Gateway Endpoint**. "Outros serviços" = **Interface Endpoint** (PrivateLink).
- "Conexão **dedicada** e privada" = **Direct Connect**. "Criptografada pela internet" = **VPN**.
- "Tráfego de IP registrado" = **VPC Flow Logs**.
- VPC é **regional**. Subnet é de **uma AZ**.
```
