---
title: Preparando para a prova
subtitle: Seção 22 do CLF-C02 — formato, estratégia, mapa gatilho → serviço e checklist final
cols: 3
exam: Todos os domínios
---
A prova **CLF-C02** mede conhecimento **básico** de nuvem. Ela cobra **o que cada serviço faz**, não detalhes de configuração. Esta página resume o formato, a estratégia e os **gatilhos** das seções 3 a 21.

## A Formato da prova {span=2}
| Item | Detalhe |
|---|---|
| Questões | **65** (50 valem nota, 15 são não pontuadas) |
| Tempo | **90 minutos** |
| Nota | Escala de 100 a 1000. Aprovação com **700** |
| Tipos | **Múltipla escolha** (uma certa) e **múltipla resposta** (duas ou mais certas) |
| Penalidade | **Nenhuma** por resposta errada. Responda tudo |

- Confira **formato, valor e idiomas** no site oficial da AWS. Esses dados mudam.

## B Os 4 domínios {span=1}
```limits
Conceitos de nuvem | 24 / 100 | %
Segurança e conformidade | 30 / 100 | %
Tecnologia e serviços | 34 / 100 | %
Faturamento e suporte | 12 / 100 | %
```

## C Estratégia de resolução {span=3}
1. Leia a **última frase** primeiro. Ela diz o que a questão pede.
2. Procure **palavras-gatilho**: "serverless", "menor custo", "alta disponibilidade", "gerenciado".
3. **Elimine** as respostas obviamente erradas. Quase sempre sobram duas.
4. Em questões de **múltipla resposta**, marque **exatamente** o número pedido.
5. Marque as dúvidas para **revisão** e volte depois.
6. **Não deixe questão em branco**. Chute no fim.
7. Reserve **10 minutos** para revisar.

## D Mapa gatilho → serviço (compute e armazenamento) {span=3}
| Gatilho na questão | Resposta |
|---|---|
| Servidor virtual na nuvem | **EC2** |
| Disco de uma instância | **EBS** |
| Arquivos compartilhados Linux | **EFS** |
| Objetos, backup, site estático | **S3** |
| Arquivo morto e retenção longa | **Glacier**, **Deep Archive** |
| Padrão de acesso desconhecido | **S3 Intelligent-Tiering** |
| Código sem servidor, por evento | **Lambda** |
| Containers | **ECS**, **EKS**, **Fargate** |
| Simples e barato, VPS | **Lightsail** |
| Jobs em lote | **AWS Batch** |
| Distribuir tráfego | **ELB**: ALB (HTTP), NLB (TCP, IP estático), GWLB (firewalls) |
| Substituir instância com falha, escalar | **Auto Scaling Group** |
| Mover dados offline | **Snow Family** |
| Mover dados online e agendado | **DataSync** |
| Armazenamento híbrido | **Storage Gateway** |

## E Mapa gatilho → serviço (dados, rede e integração) {span=3}
| Gatilho na questão | Resposta |
|---|---|
| SQL gerenciado | **RDS** |
| MySQL ou PostgreSQL mais rápido | **Aurora** |
| NoSQL serverless | **DynamoDB** |
| Cache em memória | **ElastiCache** (**DAX** para DynamoDB) |
| MongoDB, grafo, séries temporais | **DocumentDB**, **Neptune**, **Timestream** |
| Data warehouse | **Redshift** |
| SQL direto no S3 | **Athena** |
| Dashboards e BI | **QuickSight** |
| ETL | **Glue** |
| Migrar banco com origem ativa | **DMS** |
| DNS e roteamento | **Route 53** |
| CDN e conteúdo em cache | **CloudFront** |
| Rede privada isolada | **VPC** |
| Fila e desacoplamento | **SQS** |
| Notificações (pub/sub) | **SNS** |
| Fluxo de dados em tempo real | **Kinesis** |

## F Mapa gatilho → serviço (monitoramento, segurança e ML) {span=3}
| Gatilho na questão | Resposta |
|---|---|
| Métricas, alarmes, logs | **CloudWatch** |
| Chamadas de API (auditoria) | **CloudTrail** |
| Mudanças de configuração e conformidade | **AWS Config** |
| Saúde dos serviços da AWS | **Health Dashboard** |
| Infra como código | **CloudFormation** |
| Provisionar app sem gerenciar infra | **Elastic Beanstalk** |
| Permissões e usuários | **IAM** |
| SSO em várias contas | **IAM Identity Center** |
| Login de clientes do app | **Cognito** |
| Chaves de criptografia | **KMS** |
| Segredos e senhas | **Secrets Manager** |
| Ameaças por análise de logs | **GuardDuty** |
| Vulnerabilidades em EC2 e containers | **Inspector** |
| Firewall web (SQL injection, XSS) | **WAF** |
| DDoS | **Shield** |
| Relatórios de conformidade | **Artifact** |
| Imagens, texto, fala, previsão | **Rekognition**, **Comprehend**, **Polly**, **Transcribe**, **Forecast** |
| Treinar e implantar modelos | **SageMaker** |
| Chatbot | **Lex** |
| IA generativa | **Bedrock** |

## G Mapa gatilho → serviço (custos, suporte e arquitetura) {span=3}
| Gatilho na questão | Resposta |
|---|---|
| Custos por tag, gráficos | **Cost Explorer** |
| Alerta de orçamento | **AWS Budgets** |
| Estimar custo antes de usar | **Pricing Calculator** |
| Faturas e relatórios de custo detalhados | **Cost and Usage Report** |
| Várias contas, faturamento consolidado | **AWS Organizations** |
| Verificar a conta e recomendar | **Trusted Advisor** |
| Plano de suporte com TAM | **Enterprise** |
| Revisar contra os 6 pilares | **Well-Architected Tool** |
| Lift and shift | **Rehost**, **Application Migration Service** |
| Perspectivas de adoção | **CAF** |
| Software de terceiros | **Marketplace** |
| Resposta compartilhada | **Modelo de responsabilidade compartilhada** |

- **Confirme** os nomes e os recursos de cada linha nas seções correspondentes. Este mapa é um resumo.

## H Checklist final de estudo
- [ ] Explico o **modelo de responsabilidade compartilhada** sem consultar.
- [ ] Sei os **6 pilares** do Well-Architected e as **6 perspectivas** do CAF.
- [ ] Diferencio **ALB, NLB e GWLB** e **escala vertical e horizontal**.
- [ ] Diferencio as **classes do S3** e sei o caso de cada uma.
- [ ] Diferencio **Multi-AZ** de **read replica**.
- [ ] Sei os **modelos de preço** do EC2: sob demanda, Reserved, Savings Plans, Spot e Dedicated.
- [ ] Sei os **planos de suporte** e o que cada um inclui.
- [ ] Diferencio **security group** e **NACL**.
- [ ] Diferencio **CloudWatch**, **CloudTrail** e **Config**.
- [ ] Faço **pelo menos dois simulados** e reviso os erros.

```callout warn Dicas do dia da prova
- Leia **toda** a questão. Uma palavra muda a resposta ("menor custo", "serverless").
- A resposta **mais simples e gerenciada** costuma ser a certa.
- Responda **todas** as questões. Não há penalidade por erro.
- Agende a prova em um horário em que você está **descansado**.
- Treine com **simulados próprios**. Não use questões reais vazadas.
```
