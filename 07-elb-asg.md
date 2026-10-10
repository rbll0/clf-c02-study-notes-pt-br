---
title: ELB e ASG
subtitle: Seção 7 do CLF-C02 — Elastic Load Balancing e Auto Scaling Groups
cols: 3
exam: Domínio 3 · Tecnologia e serviços (34%)
---
**Escalar** é ajustar a capacidade à demanda. O **ELB** distribui o tráfego entre as instâncias. O **ASG** liga e desliga as instâncias sozinho. Juntos, entregam **alta disponibilidade** e **elasticidade**.

## A Escalabilidade vs alta disponibilidade {span=3}
| Conceito | Significado | Como se faz |
|---|---|---|
| Escala vertical (scale up) | Aumentar o **tamanho** da instância | t2.micro → t2.large |
| Escala horizontal (scale out) | Aumentar o **número** de instâncias | Mais servidores atrás de um balanceador |
| Alta disponibilidade | Rodar em **2 ou mais AZs** | O sistema sobrevive à queda de um datacenter |
| Elasticidade | A capacidade acompanha a demanda automaticamente | Auto Scaling |

- Escala vertical é comum em bancos de dados. Ela tem limite de hardware.
- Escala horizontal é comum em apps web. Ela se aplica a sistemas distribuídos.
- Elasticidade difere de escalabilidade: o sistema **cresce e diminui** conforme a carga e você paga só o que usa.

## B O que é um Load Balancer {span=2}
```flow LR
(Usuários) -> *Load Balancer: um único ponto de entrada
Load Balancer -> Instância A & Instância B & Instância C: distribui
group Várias AZs: Instância A, Instância B, Instância C
```
- Ele **encaminha** o tráfego para vários servidores.
- Ele expõe **um único DNS** para a sua aplicação.
- Ele faz **health checks** e deixa de enviar tráfego a instâncias com problema.
- Ele termina o **SSL/TLS** (HTTPS) e pode funcionar em várias AZs.
- É um serviço **gerenciado**: a AWS cuida de atualização, manutenção e alta disponibilidade.

## C Por que usar um ELB
- Distribui a carga entre instâncias.
- Esconde a falha de instâncias com health checks.
- Dá acesso único à aplicação.
- Cobre falhas de uma AZ inteira.
- Separa tráfego público do privado.

## D Os 4 tipos de load balancer {span=3}
| Tipo | Camada | Protocolos | Melhor para |
|---|---|---|---|
| Application Load Balancer (ALB) | 7 (aplicação) | HTTP, HTTPS, gRPC, WebSocket | Apps web, microsserviços, containers |
| Network Load Balancer (NLB) | 4 (transporte) | TCP, UDP, TLS | Desempenho extremo, milhões de requisições, baixa latência, **IP estático** |
| Gateway Load Balancer (GWLB) | 3 (rede) | Protocolo GENEVE (porta 6081) | Firewalls, detecção de intrusão e inspeção de pacotes |
| Classic Load Balancer (CLB) | 4 e 7 | HTTP, HTTPS, TCP | warn Geração antiga. Está **sendo aposentado** |

## E ALB — Application Load Balancer {span=2}
O ALB toma decisões olhando o **conteúdo** da requisição.

| Roteamento por | Exemplo |
|---|---|
| Caminho da URL | `/users` vai para um serviço, `/search` vai para outro |
| Hostname | `one.exemplo.com` e `other.exemplo.com` |
| Parâmetros da query | `?platform=mobile` |
| Cabeçalhos HTTP | Tipo de cliente |

- Ele roteia para **vários grupos de destino** (target groups): EC2, containers ECS, Lambda e IPs.
- Ele tem **DNS fixo**, mas o IP **muda**.

## F NLB e GWLB
| | NLB | GWLB |
|---|---|---|
| Camada | 4 | 3 |
| Destaque | **IP estático** por AZ e Elastic IP | Passa o tráfego por **appliances de terceiros** |
| Desempenho | Milhões de requisições por segundo, latência muito baixa | Inspeção transparente |
| Caso de uso | Jogos, IoT, apps com TCP ou UDP | Firewall, IDS e IPS, análise de pacotes |

## G Auto Scaling Group (ASG) {span=2}
```flow LR
Métrica CloudWatch --> *Auto Scaling Group: dispara alarme
Auto Scaling Group -> Instância EC2: scale out (adiciona)
Auto Scaling Group --> Instância EC2: scale in (remove)
Auto Scaling Group -> Load Balancer: registra as instâncias
```
O ASG mantém o número de instâncias **entre um mínimo e um máximo**.

| Parâmetro | Significado |
|---|---|
| Mínimo | Menor número de instâncias sempre ativas |
| Capacidade desejada | Número alvo no momento |
| Máximo | Teto, que limita o custo |

## H Benefícios do ASG
- **Scale out**: adiciona instâncias quando a carga sobe.
- **Scale in**: remove instâncias quando a carga cai.
- **Substitui** automaticamente uma instância com falha.
- Registra e remove instâncias do **load balancer** sozinho.
- **Economiza**: você roda só o necessário.
- O ASG em si é **gratuito**. Você paga pelas instâncias.

## I Estratégias de escala do ASG {span=3}
| Estratégia | Como funciona | Exemplo |
|---|---|---|
| Manual | Você muda a capacidade desejada | Ajuste pontual |
| Dinâmica: tracking de alvo | Mantém uma métrica em um valor | Manter a CPU média em 40% |
| Dinâmica: escala simples ou por passos | Um alarme do CloudWatch dispara uma ação | CPU > 70% adiciona 2 instâncias |
| Agendada | Muda a capacidade em horários conhecidos | Mais servidores às 17h de sexta |
| Preditiva | Aprende com o histórico e prevê a carga (usa ML) | Padrões cíclicos e recorrentes |

## J ELB + ASG: a dupla {span=3}
| Peça | Papel |
|---|---|
| ELB | Distribui o tráfego e faz health checks |
| ASG | Cria e remove instâncias conforme a demanda |
| Juntos | Alta disponibilidade, elasticidade e custo otimizado |

```callout warn Pegadinhas de prova
- "Mais servidores" = escala **horizontal**. "Servidor maior" = escala **vertical**.
- "Rotear por URL ou HTTP" = **ALB**. "IP estático e desempenho extremo" = **NLB**.
- "Firewall de terceiros na frente do tráfego" = **GWLB**.
- "Substituir instância com falha" = **ASG**.
- "Escalar nos horários conhecidos" = **agendada**. "Prever com ML" = **preditiva**.
- Alta disponibilidade = **várias AZs**.
```
