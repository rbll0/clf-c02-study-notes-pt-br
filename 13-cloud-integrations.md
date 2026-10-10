---
title: Cloud Integrations
subtitle: Seção 13 do CLF-C02 — SQS, SNS, Kinesis, MQ, EventBridge e Step Functions
cols: 3
exam: Domínio 3 · Tecnologia e serviços (34%)
---
Aplicações grandes têm várias partes que **conversam entre si**. Se elas se ligam de forma direta, uma falha derruba as outras. Os serviços desta seção **desacoplam** as partes: uma fila ou um evento fica no meio e absorve picos e falhas.

## A Acoplamento forte vs fraco {span=2}
```flow LR
Comprador -> *Fila SQS: envia pedido
Fila SQS -> Processador: lê no seu ritmo
Comprador --> Processador: ~~chamada direta~~
```
- **Acoplamento forte**: o serviço A chama o serviço B direto. Se B cai ou lota, A falha.
- **Acoplamento fraco**: A grava em um **intermediário** (fila, tópico, stream). B lê quando puder.
- Cada parte **escala sozinha**. Um pico no comprador não derruba o processador.

## B Os três modelos
- **Fila** (SQS): uma mensagem, **um** consumidor.
- **Pub/sub** (SNS): uma mensagem, **vários** assinantes.
- **Stream** (Kinesis): dados **em tempo real**, em fluxo contínuo.

## C Visão geral dos serviços {span=3}
| Serviço | Modelo | Função | Gatilho de prova |
|---|---|---|---|
| **SQS** | Fila | Guarda mensagens até um consumidor ler | "Desacoplar", "fila", "buffer" |
| **SNS** | Pub/sub | Envia uma mensagem a **vários** destinos | "Notificar", "fan-out", "e-mail ou SMS" |
| **Kinesis** | Stream | Coleta e processa dados **em tempo real** | "Streaming", "logs em tempo real", "IoT" |
| **Amazon MQ** | Broker | ActiveMQ e RabbitMQ gerenciados | "Migrar app que usa MQTT, AMQP ou JMS" |
| **EventBridge** | Barramento de eventos | Roteia eventos de serviços AWS, apps e SaaS | "Reagir a eventos", "agendar tarefas" |
| **Step Functions** | Orquestração | Encadeia passos em um **fluxo visual** | "Workflow", "orquestrar Lambdas" |
| **AppSync** | API | **GraphQL** gerenciado | "GraphQL" |

## D SQS — Simple Queue Service {span=2}
O SQS é a **primeira** oferta da AWS. É **serverless** e escala sozinho.

| Item | Detalhe |
|---|---|
| Modelo | **Pull**: o consumidor **busca** as mensagens |
| Mensagem | Até **256 KB** |
| Retenção | De 1 minuto a **14 dias** (padrão: 4 dias) |
| Fila **Standard** | Vazão quase ilimitada. Entrega **pelo menos uma vez**. Ordem **não** garantida |
| Fila **FIFO** | **Ordem** garantida e **sem duplicatas** |

- O consumidor **apaga** a mensagem depois de processar.
- O ASG pode **escalar** pelo tamanho da fila.

## E SNS — Simple Notification Service
- Modelo **pub/sub**: o publicador envia a um **tópico**.
- Todos os **assinantes** do tópico recebem a mensagem (**push**).
- Destinos: **SQS**, **Lambda**, **HTTP/S**, **e-mail**, **SMS** e notificações push móveis.
- Os alarmes do CloudWatch e vários serviços AWS publicam no SNS.

## F Fan-out: SNS + SQS {span=2}
```flow LR
Serviço de pedidos -> *Tópico SNS: publica um evento
Tópico SNS -> Fila de estoque: assina
Tópico SNS -> Fila de e-mail: assina
Tópico SNS -> Fila de análise: assina
```
- O produtor publica **uma vez**. Cada fila recebe uma **cópia**.
- Cada consumidor trabalha **no seu ritmo** e não perde mensagens.

## G Kinesis {span=3}
| Serviço | Função |
|---|---|
| **Kinesis Data Streams** | Captura **fluxos de dados** em tempo real. Você escolhe a capacidade e escreve os consumidores |
| **Amazon Data Firehose** | **Entrega** o fluxo a destinos como S3, Redshift e OpenSearch. Gerenciado, quase em tempo real |
| **Managed Service for Apache Flink** | Analisa o fluxo em tempo real com **SQL ou Flink** (antigo Kinesis Data Analytics) |
| **Kinesis Video Streams** | Captura e processa **vídeo** em fluxo |

- Casos de uso: logs de aplicações, cliques de sites, métricas de **IoT**.

## H Amazon MQ, EventBridge e Step Functions
- **Amazon MQ**: broker gerenciado de **ActiveMQ** e **RabbitMQ**. Use para **migrar** apps que já usam protocolos abertos (MQTT, AMQP, JMS). SQS e SNS são APIs próprias da AWS.
- **EventBridge**: recebe eventos e os **roteia por regras**. Dispara também tarefas **agendadas** (tipo cron).
- **Step Functions**: monta um **fluxo de passos** em diagrama. Cada passo pode chamar Lambda, ECS, Batch e outros. Trata **erros e repetições** sem código extra.
- **AppSync**: API **GraphQL** gerenciada. Busca dados de várias fontes e atualiza apps em **tempo real**.

## I Quando usar cada um {span=3}
| Situação | Serviço |
|---|---|
| Absorver picos e **desacoplar** dois serviços | SQS |
| Avisar **vários** sistemas sobre um evento | SNS (+ SQS) |
| Receber **milhões** de eventos por segundo em tempo real | Kinesis |
| Levar o fluxo para o **S3** sem escrever código | Data Firehose |
| Mover um sistema **on-premises** com RabbitMQ | Amazon MQ |
| Reagir a uma mudança na conta (por exemplo, instância parada) | EventBridge |
| Coordenar várias Lambdas em ordem | Step Functions |

```callout warn Pegadinhas de prova
- "Desacoplar" ou "buffer" = **SQS**. "Notificar vários" = **SNS**. "Tempo real" = **Kinesis**.
- SQS é **pull**. SNS é **push**.
- "Ordem e sem duplicatas" = **SQS FIFO**.
- "Migrar app com RabbitMQ ou ActiveMQ" = **Amazon MQ**, não SQS.
- "Entregar o stream ao S3" = **Data Firehose**.
- "Rotear eventos" ou "agendar" = **EventBridge**. "Orquestrar" = **Step Functions**.
- "GraphQL" = **AppSync**.
```
