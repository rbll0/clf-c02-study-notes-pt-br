---
title: AWS Cloud Practitioner (CLF-C02)
subtitle: Os conceitos que caem na prova, por domínio
cols: 3
exame: 65 questões · 90 min · nota mínima 700/1000
---
A prova testa conceitos, não configuração. Saiba **o que cada serviço resolve** e **quem é responsável pelo quê**.

## A Os 4 domínios da prova {span=3}
| Domínio | Peso | Foco |
|---|---|---|
| 1. Conceitos de nuvem | 24% | Benefícios, Well-Architected, migração, economia |
| 2. Segurança e compliance | 30% | Responsabilidade compartilhada, IAM, proteção |
| 3. Tecnologia e serviços | 34% | Infraestrutura global, compute, storage, banco, rede |
| 4. Cobrança e suporte | 12% | Modelos de preço, ferramentas de custo, planos de suporte |

## B Benefícios da nuvem
- **Elasticidade**: a capacidade cresce e diminui com a demanda.
- **Escalabilidade**: o sistema suporta mais carga ao adicionar recursos.
- **Alta disponibilidade**: o sistema continua ativo com várias AZs.
- **Agilidade**: você cria recursos em minutos.
- **Economia de escala**: a AWS compra em volume e baixa o preço.
- **CapEx → OpEx**: você troca investimento fixo por custo variável.
- **Alcance global**: você publica em várias regiões rapidamente.

## C Modelos de serviço
| Modelo | Você gerencia | Exemplo AWS |
|---|---|---|
| IaaS | SO, apps, dados | EC2 |
| PaaS | Apps e dados | Elastic Beanstalk |
| SaaS | Só o uso | Amazon WorkMail |

**Implantação**: nuvem pública, on-premises (privada) e híbrida.

## D Well-Architected: 6 pilares
| Pilar | Pergunta-chave |
|---|---|
| Excelência operacional | Como você opera e melhora? |
| Segurança | Como você protege dados e sistemas? |
| Confiabilidade | O sistema se recupera de falhas? |
| Eficiência de performance | Você usa os recursos certos? |
| Otimização de custo | Você evita gasto inútil? |
| Sustentabilidade | Você reduz o impacto ambiental? |

## E Responsabilidade compartilhada (exemplo EC2) {span=2}
```flow LR
Datacenter físico -> Hardware e rede global -> Hipervisor -> *SO e patches -> Security Groups -> IAM e acessos -> Dados e criptografia
group AWS cuida da nuvem: Datacenter físico, Hardware e rede global, Hipervisor
group Cliente cuida do que põe na nuvem: SO e patches, Security Groups, IAM e acessos, Dados e criptografia
```
- Em serviços gerenciados (RDS, Lambda, S3), a AWS cuida de mais camadas, como SO e patches.
- O **cliente sempre** é dono dos dados e dos acessos.

## F Segurança: serviços-chave
| Serviço | Função |
|---|---|
| IAM | Usuários, grupos, roles e políticas |
| MFA | Segundo fator. Ative na conta root |
| Organizations + SCP | Governa várias contas |
| Shield | Proteção DDoS (Standard grátis) |
| WAF | Firewall de aplicação web |
| GuardDuty | Detecta ameaças |
| Inspector | Busca vulnerabilidades em EC2 e containers |
| Macie | Encontra dados sensíveis no S3 |
| KMS | Gerencia chaves de criptografia |
| Artifact | Relatórios de compliance (ISO, SOC, PCI) |
| CloudTrail | Registra quem chamou cada API |
| Config | Histórico de configuração dos recursos |

## G Infraestrutura global
```tree
Infraestrutura global AWS
  Região | Área geográfica isolada. Escolha por compliance, latência, preço e serviços
    Availability Zone | Um ou mais datacenters. Mínimo de 3 por região
  Edge Location | Cache do CloudFront perto do usuário
  Local Zone / Wavelength | Latência muito baixa em cidades e redes 5G
```

## H Serviços principais {span=2}
| Categoria | Serviços |
|---|---|
| Compute | EC2, Lambda (serverless), ECS / EKS (containers), Fargate, Elastic Beanstalk |
| Storage | S3 (objetos), EBS (disco do EC2), EFS (arquivos compartilhados), Glacier (arquivo), Storage Gateway (híbrido) |
| Banco de dados | RDS e Aurora (relacional), DynamoDB (NoSQL), ElastiCache (cache), Redshift (data warehouse) |
| Rede | VPC, Route 53 (DNS), CloudFront (CDN), ELB, Direct Connect, VPN |
| Monitoramento | CloudWatch (métricas e alarmes), Trusted Advisor |
| Migração | Snowball, DMS, Migration Hub, Application Migration Service |
| IA / ML | SageMaker, Rekognition, Comprehend, Lex, Polly, Bedrock |

## I Modelos de preço do EC2 {span=2}
| Modelo | Quando usar | Desconto |
|---|---|---|
| On-Demand | Carga curta e imprevisível | Nenhum |
| Reserved Instances | Carga estável por 1 ou 3 anos | Até 72% |
| Savings Plans | Compromisso de gasto por hora, mais flexível | Até 72% |
| Spot | Carga que tolera interrupção | Até 90% |
| Dedicated Host | Licença por servidor ou compliance | Mais caro |

## J Ferramentas de custo
- **Pricing Calculator**: estima custo antes de criar.
- **Cost Explorer**: analisa o gasto passado.
- **Budgets**: alerta quando passa do limite.
- **Cost and Usage Report**: relatório mais detalhado.
- **Tags de alocação**: separam custo por projeto.
- **Free Tier**: sempre grátis, 12 meses e trials.

## K Planos de suporte {span=3}
| Plano | Resposta em caso crítico | TAM | Trusted Advisor completo |
|---|---|---|---|
| Basic | — | no | no |
| Developer | — | no | no |
| Business | < 1 h (produção fora do ar) | no | ok |
| Enterprise On-Ramp | < 30 min | warn Pool de TAMs | ok |
| Enterprise | < 15 min | ok Dedicado | ok |

```callout info Dica de prova
Leia o verbo da pergunta. "Auditar chamadas de API" = CloudTrail. "Monitorar métricas" = CloudWatch. "Recomendações de custo e segurança" = Trusted Advisor. "Relatório de compliance" = Artifact.
```
