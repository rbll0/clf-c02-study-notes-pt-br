---
title: Contas, Billing e Suporte
subtitle: Seção 18 do CLF-C02 — Organizations, modelos de preço, ferramentas de custo e planos de suporte
cols: 3
exam: Domínio 4 · Faturamento, preços e suporte (12%)
---
Esta seção cobre **como organizar contas**, **como a AWS cobra**, **como reduzir custo** e **qual plano de suporte contratar**. Os **modelos de preço do EC2** e os **planos de suporte** caem muito na prova.

## A AWS Organizations {span=2}
```tree
Raiz | Conta de gerenciamento (management account)
  OU Produção | Unidade organizacional
    Conta App A | Membro
    Conta App B | Membro
  OU Desenvolvimento | Unidade organizacional
    Conta Dev | Membro
  OU Segurança | Unidade organizacional
    Conta de logs | Membro
```
- **Faturamento consolidado**: uma fatura para todas as contas.
- **Descontos por volume** somam o uso de todas as contas.
- As contas **compartilham** Reserved Instances e Savings Plans.
- Você cria contas por **API**.
- **SCP** (Service Control Policy): limita o que as contas de uma **OU** podem fazer. Ela **não** afeta a conta de gerenciamento.
- A SCP **nega** ou **permite** no limite. Ela não concede permissão sozinha.

## B Control Tower {span=1}
- Monta um **ambiente multiconta** (landing zone) seguindo **boas práticas**.
- Usa **guardrails**: **preventivos** (SCP) e **detectivos** (Config).
- Mostra um **painel** de conformidade.
- Gatilho: "configurar contas com governança", "ambiente seguro em poucos cliques".

## C Modelos de preço do EC2 {span=3}
| Modelo | Como funciona | Desconto | Melhor para |
|---|---|---|---|
| **On-Demand** | Paga por segundo (Linux e Windows) ou por hora. **Sem compromisso** | Nenhum | Cargas curtas e imprevisíveis |
| **Reserved Instances** | Compromisso de **1 ou 3 anos** com um tipo de instância. Pagamento total, parcial ou sem entrada | Até **72%** | Uso **estável** e contínuo |
| **Savings Plans** | Compromisso de **US$/hora** por 1 ou 3 anos. Mais flexível que o RI | Até **72%** | Uso estável com flexibilidade de família ou região |
| **Spot** | Capacidade **ociosa**. A AWS pode **interromper** a instância | Até **90%** | Tarefas **tolerantes a falha**: lotes, big data, CI |
| **Dedicated Host** | **Servidor físico** só seu. Você controla o posicionamento | O mais caro | **Licenças por socket** (BYOL) e **conformidade** |
| **Dedicated Instance** | Hardware dedicado a **você**. Sem controle do host | Alto | Isolamento de hardware |
| **Capacity Reservation** | **Reserva** capacidade em uma **AZ**. Sem compromisso de prazo | Nenhum, cobra a tarifa On-Demand | Garantir que a capacidade exista |

- O **Capacity Reservation** cobra **mesmo sem uso** da instância.
- **Convertible RI** permite trocar o tipo de instância. **Standard RI** rende mais desconto.
- Savings Plans: **Compute** (mais flexível, até 66%) e **EC2 Instance** (até 72%).

## D Free Tier {span=1}
- **12 meses grátis**: por exemplo, 750 h/mês de t2.micro ou t3.micro.
- **Sempre grátis**: por exemplo, 1 milhão de chamadas Lambda por mês.
- **Testes curtos**: período limitado por serviço.
- Contas novas após **jul/2025** usam um plano com **créditos**. A prova pode seguir o modelo antigo.

## E Ferramentas de custo {span=3}
| Ferramenta | Função |
|---|---|
| **Pricing Calculator** | **Estima** o custo de uma arquitetura **antes** de criar |
| **Budgets** | Cria **orçamentos** e envia **alertas** quando o custo real ou **previsto** passa do limite |
| **Billing Alarm** | Alarme do **CloudWatch** sobre a **cobrança estimada**. Fica na região **us-east-1** |
| **Cost Explorer** | **Visualiza** e analisa custos e uso. **Prevê** custos futuros. Recomenda Reserved Instances e Savings Plans |
| **Cost and Usage Report (CUR)** | Relatório **mais detalhado**. Chega ao **S3** e abre no Athena ou QuickSight |
| **Cost Allocation Tags** | **Etiquetas** que agrupam custos por projeto ou time. Você precisa **ativá-las** no Billing |
| **Cost Anomaly Detection** | Detecta **gastos incomuns** com ML |
| **Compute Optimizer** | Recomenda o tamanho certo (**rightsizing**) para EC2, ASG, EBS e Lambda |

## F Trusted Advisor {span=2}
O **Trusted Advisor** inspeciona a sua conta e **recomenda melhorias** em **seis categorias**:

| Categoria | Exemplo |
|---|---|
| **Otimização de custos** | Instâncias subutilizadas |
| **Desempenho** | Volumes EBS lentos |
| **Segurança** | Bucket S3 aberto, falta de MFA |
| **Tolerância a falhas** | Falta de Multi-AZ ou de backup |
| **Limites de serviço** | Uso perto da cota |
| **Excelência operacional** | Boas práticas de operação |

- **Basic e Developer**: só as verificações **centrais** (core) de segurança e de limites.
- **Business, Enterprise On-Ramp e Enterprise**: **todas** as verificações e acesso por **API**.

## G Cotas e contas técnicas {span=1}
- **Service Quotas**: consulta e **pede aumento** das cotas (limites) de cada serviço.
- **TAM** (Technical Account Manager): gerente técnico **dedicado**. Só nos planos **Enterprise**.
- **Health Dashboard**: mostra o estado dos serviços e eventos que afetam **a sua conta**.

## H Planos de suporte {span=3}
| Plano | Preço inicial | Canais | Tempos de resposta | Extras |
|---|---|---|---|---|
| **Basic** | Grátis | Documentação, fóruns e atendimento de **conta e cobrança** 24/7 | — | Trusted Advisor **core**, Health Dashboard |
| **Developer** | ~US$ 29/mês | E-mail em **horário comercial** | Geral < 24 h. Sistema prejudicado < 12 h | Um contato principal |
| **Business** | ~US$ 100/mês | **24/7**: telefone, e-mail e chat | Produção prejudicada < 4 h. Produção parada < 1 h | Trusted Advisor completo, **API**, suporte a software de terceiros |
| **Enterprise On-Ramp** | ~US$ 5.500/mês | 24/7 | Produção parada < 1 h. **Crítico para o negócio** parado < 30 min | **Pool de TAMs**, equipe Concierge, revisões de arquitetura |
| **Enterprise** | ~US$ 15.000/mês | 24/7 | **Crítico para o negócio** parado < **15 min** | **TAM dedicado**, Concierge, revisão Well-Architected, gestão de eventos |

- Valores e tempos são os **do curso**. A AWS atualiza os planos. Confira a página oficial.
- Memorize a **escada**: Basic → Developer → Business → On-Ramp → Enterprise.
- **Business** é o primeiro com **24/7** e **Trusted Advisor completo**.

## I Ciclo de gestão de custo {span=2}
```flow LR
Pricing Calculator -> *Budgets: estima e define o limite
Budgets -> Cost Explorer: alerta e analisa
Cost Explorer -> Compute Optimizer: acha desperdício
Compute Optimizer -> Pricing Calculator: reestima
```
- **Estimar** antes. **Controlar** durante. **Analisar** e **otimizar** depois.

## J Resumo {span=1}
- Muitas contas = **Organizations**.
- Regras entre contas = **SCP**.
- Fatura única = **consolidada**.

```callout warn Pegadinhas de prova
- "Fatura única e desconto por volume" = **Organizations**. "Restringir serviços numa OU" = **SCP**.
- "Uso estável, 1 ou 3 anos" = **Reserved Instances** ou **Savings Plans**. "Mais flexível" = **Savings Plans**.
- "Pode ser interrompido, mais barato" = **Spot**. "Licença por socket" = **Dedicated Host**.
- "Garantir capacidade em uma AZ" = **Capacity Reservation**. "Sem compromisso" = **On-Demand**.
- "Estimar antes de criar" = **Pricing Calculator**. "Alerta de orçamento" = **Budgets**.
- "Analisar e prever custos" = **Cost Explorer**. "Mais detalhado" = **CUR**. "Agrupar por projeto" = **tags de alocação**.
- "Rightsizing" = **Compute Optimizer**. "Recomendações em seis categorias" = **Trusted Advisor**.
- "TAM" = **Enterprise** (On-Ramp tem um pool). "24/7 por telefone, o primeiro plano" = **Business**.
```
