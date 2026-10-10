---
title: Cloud Monitoring
subtitle: Seção 14 do CLF-C02 — CloudWatch, EventBridge, CloudTrail, X-Ray, Config e Health
cols: 3
exam: Domínio 3 · Tecnologia e serviços (34%)
---
Monitorar responde a três perguntas: **como está** (métricas e logs), **quem fez** (auditoria) e **o que mudou** (configuração). Cada pergunta tem um serviço. Decore o par **pergunta → serviço**.

## A Qual serviço responde o quê {span=3}
| Pergunta | Serviço | Resumo |
|---|---|---|
| Como estão CPU, rede e erros? | **CloudWatch Metrics** | Números ao longo do tempo |
| O que a aplicação escreveu no log? | **CloudWatch Logs** | Coleta e guarda logs |
| Avise-me quando algo passar do limite | **CloudWatch Alarms** | Dispara uma ação |
| Quem chamou a API e quando? | **CloudTrail** | Auditoria de chamadas de API |
| Qual é o caminho da requisição e onde ela demora? | **X-Ray** | Rastreamento distribuído |
| Como a configuração dos recursos mudou? | **AWS Config** | Histórico e conformidade |
| Algum serviço da AWS está com problema? | **Health Dashboard** | Status e eventos da AWS |
| Reagir a um evento da conta | **EventBridge** | Regras que disparam ações |

## B CloudWatch Metrics {span=2}
- Uma **métrica** é uma variável monitorada: `CPUUtilization`, `NetworkIn` e outras.
- Os dados pertencem a um **namespace** (por exemplo, `AWS/EC2`) e têm **dimensões**.
- Todo serviço AWS **publica métricas** sozinho.
- **EC2**: monitoramento **básico** a cada **5 minutos** e **detalhado** a cada **1 minuto** (pago).
- Você também envia **métricas personalizadas**, como o uso de **memória** do EC2. A memória **não** vem por padrão.
- **Dashboards** juntam gráficos de métricas em uma tela, em **várias regiões**.

## C CloudWatch Alarms
- Um alarme **vigia uma métrica** e muda de estado: **OK**, **ALARM** ou **INSUFFICIENT_DATA**.
- Ações possíveis: notificar o **SNS**, rodar uma ação do **EC2** (parar, reiniciar, recuperar) ou **escalar** um ASG.
- **Alarme de cobrança** (billing) usa a métrica de gastos. Ele existe **só em us-east-1**.
- Um **composite alarm** combina vários alarmes.

## D CloudWatch Logs {span=2}
```flow LR
Instância EC2 -> *CloudWatch Logs: agente envia logs
Lambda -> CloudWatch Logs: logs automáticos
CloudWatch Logs -> Logs Insights: consulta
CloudWatch Logs --> Amazon S3: exporta
CloudWatch Logs --> CloudWatch Alarms: filtro de métrica
```
- Os logs ficam em **log groups** e **log streams**.
- Você define a **retenção** de cada grupo. O padrão é **nunca expirar**.
- O **CloudWatch Agent** envia logs e métricas do **EC2** e de servidores **on-premises**.
- **Logs Insights**: consulta os logs com uma linguagem própria.
- **Lambda**, **ECS**, **API Gateway** e **CloudTrail** enviam logs ao CloudWatch.

## E EventBridge
- Antigo **CloudWatch Events**.
- Uma **regra** casa com um evento e dispara um **destino**.
- **Fontes**: serviços AWS, seus apps e parceiros SaaS.
- **Agenda** (cron): rodar uma Lambda a cada hora.
- Exemplo: "o usuário root entrou" → **SNS** envia um e-mail.

## F CloudTrail {span=2}
O **CloudTrail** registra as **chamadas de API** da conta. Ele responde "**quem** fez **o quê**, **quando** e de **onde**".

| Item | Detalhe |
|---|---|
| Ativado | **Por padrão**, em toda conta |
| Histórico de eventos | Os **últimos 90 dias**, de graça |
| Trail | Configuração que **guarda** os eventos no **S3** por mais tempo |
| Eventos de **gerenciamento** | Ações de controle (criar, apagar, mudar permissões) |
| Eventos de **dados** | Ações em recursos (ler um objeto S3, rodar uma Lambda). Ficam **desligados** por padrão |
| **Insights** | Detecta atividade **incomum** de API |

- Cobre console, CLI, SDKs e outros serviços.
- Use para **auditoria**, **segurança** e investigação de incidentes.

## G X-Ray
- Mostra o **caminho** de uma requisição por **vários serviços**.
- Ajuda a achar **gargalos** e **erros**.
- Traz um **mapa visual** das chamadas entre os componentes.
- Use em arquiteturas de **microsserviços** e **serverless**.
- Pergunta típica: "por que esta requisição está lenta?".

## H AWS Config
- **Grava** a configuração de cada recurso e **como ela mudou**.
- **Regras** avaliam a conformidade. Exemplos: "buckets S3 não podem ser públicos" e "volumes EBS devem ser criptografados".
- Ele **não impede** a mudança. Ele **avisa** e registra.
- Ele **não** mostra métricas de desempenho. Isso é do CloudWatch.

## I Health e Trusted Advisor {span=3}
| Serviço | O que mostra |
|---|---|
| **AWS Health Dashboard — Service Health** | Status **geral** dos serviços da AWS por região (a antiga Service Health Dashboard) |
| **AWS Health Dashboard — Your account** | Eventos que afetam **os seus recursos**: manutenção, falhas e prazos (a antiga Personal Health Dashboard) |
| **Trusted Advisor** | **Recomendações** de custo, desempenho, segurança, tolerância a falhas, limites de serviço e excelência operacional |

- O Health Dashboard da conta pode disparar ações pelo **EventBridge**.
- O **Trusted Advisor** completo (todas as verificações) exige o plano de suporte **Business** ou **Enterprise**.

## J CloudWatch × CloudTrail × Config {span=3}
| | CloudWatch | CloudTrail | Config |
|---|---|---|---|
| Foco | **Desempenho** e saúde | **Quem** chamou a API | **Estado** e mudanças de configuração |
| Dado | Métricas e logs | Eventos de API | Itens de configuração |
| Exemplo | CPU em 90% | Fulano apagou um bucket | O bucket ficou público |

```callout warn Pegadinhas de prova
- "Métricas, logs e alarmes" = **CloudWatch**. "Quem fez a chamada de API" = **CloudTrail**.
- "Histórico de configuração e conformidade" = **Config**. "Rastrear a requisição" = **X-Ray**.
- "Problema na AWS que afeta **meus** recursos" = **Health Dashboard da conta**.
- "Status geral dos serviços AWS" = **Service Health**.
- "Reagir a um evento ou agendar" = **EventBridge**.
- CloudTrail vem **ligado**. O histórico dura **90 dias**.
- "Memória do EC2" não vem por padrão: envie uma **métrica personalizada**.
- "Alarme de cobrança" = CloudWatch em **us-east-1**.
```
