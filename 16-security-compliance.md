---
title: Segurança e Compliance
subtitle: Seção 16 do CLF-C02 — responsabilidade compartilhada, proteção, criptografia e detecção
cols: 3
exam: Domínio 2 · Segurança e conformidade (30%)
---
A AWS protege a **nuvem**. Você protege **o que coloca nela**. Esta seção reúne os serviços que **bloqueiam** ataques, **guardam** chaves, **detectam** ameaças e **provam** conformidade. Memorize o **gatilho** de cada um.

## A Modelo de responsabilidade compartilhada {span=3}
| | AWS: segurança **DA** nuvem | Você: segurança **NA** nuvem |
|---|---|---|
| Infraestrutura | Hardware, datacenters, rede global, regiões e AZs | Configuração de rede: security groups, NACLs, rotas |
| Software | Hipervisor e software dos serviços gerenciados | Sistema operacional convidado e aplicações |
| Dados | Isolamento entre clientes | Classificação, **criptografia** e backup dos dados |
| Acesso | Controle físico dos datacenters | **IAM**: usuários, permissões, MFA |

- **EC2**: você aplica as correções do **SO**. A AWS cuida do host físico.
- **RDS**: a AWS aplica as correções do banco e do SO. Você cuida do **acesso** e dos **dados**.
- **S3**: a AWS cuida da infraestrutura. Você cuida das **políticas** e da **criptografia**.
- Quanto mais **gerenciado** o serviço, mais a AWS assume.

## B Proteção contra ataques {span=3}
| Serviço | O que faz | Camada | Detalhes |
|---|---|---|---|
| **Shield Standard** | Protege contra **DDoS** | 3 e 4 | **Grátis** e automático para todos os clientes |
| **Shield Advanced** | DDoS avançado | 3, 4 e 7 | **Pago**. Equipe de resposta 24/7 (DRT) e **proteção de custo** durante o ataque |
| **WAF** | Firewall de **aplicação web** | 7 | Regras em uma **web ACL**: SQL injection, XSS, país, IP e limite de requisições. Atua em ALB, API Gateway e CloudFront |
| **Network Firewall** | Firewall da **VPC inteira** | 3 a 7 | Filtra o tráfego que entra e sai de **toda** a VPC |
| **Firewall Manager** | Gerencia regras em **várias contas** | — | Aplica WAF, Shield Advanced, Network Firewall e security groups em toda a **Organization** |

## C Defesa em camadas {span=2}
```flow LR
Internet -> Shield: absorve DDoS nas camadas 3 e 4
Shield -> *WAF: filtra requisições da camada 7
WAF -> ALB: entrega o tráfego limpo
ALB -> EC2: passa pelo security group
Firewall Manager --> WAF: aplica as regras em todas as contas
```
- Nenhuma peça sozinha basta. Combine várias.
- O **security group** é a última barreira de cada instância.

## D Chaves e segredos {span=3}
| Serviço | Função | Quem gerencia as chaves |
|---|---|---|
| **KMS** | Cria e gerencia **chaves de criptografia** | A AWS opera o serviço. Você controla o **uso** das chaves |
| **CloudHSM** | **Hardware dedicado** para chaves | **Você** gerencia as chaves. A AWS cuida do hardware |
| **ACM** | Emite e **renova** certificados **TLS/SSL** | Certificados públicos são **grátis** |
| **Secrets Manager** | Guarda segredos: senhas e chaves de API | Faz **rotação automática**, por exemplo de senhas do **RDS** |

- O **KMS** integra com quase todos os serviços (S3, EBS, RDS) e registra cada uso no **CloudTrail**.
- **KMS** = a AWS gerencia as chaves. **CloudHSM** = você gerencia as chaves.

## E Criptografia {span=1}
- **Em repouso**: dados parados em disco. Use **KMS**. Exemplos: EBS, S3, RDS.
- **Em trânsito**: dados em movimento. Use **TLS/HTTPS**, com certificado do **ACM**.
- Ligue as duas.

## F Detecção e investigação {span=3}
| Serviço | O que faz | Gatilho de prova |
|---|---|---|
| **GuardDuty** | Detecta **ameaças** com ML. Lê **CloudTrail**, **VPC Flow Logs** e logs de **DNS** | "Atividade suspeita", "conta comprometida", "criptomineração" |
| **Inspector** | Varre **vulnerabilidades** em EC2, imagens no ECR e Lambda | "Vulnerabilidade de software", "exposição de rede" |
| **Config** | Registra a **configuração** dos recursos e o **histórico** de mudanças. Avalia **regras** de conformidade | "Quem mudou?", "o recurso está em conformidade?" |
| **Macie** | Acha **dados sensíveis** (PII) no **S3** com ML | "PII", "dados pessoais no S3" |
| **Security Hub** | **Painel central** que reúne os alertas de vários serviços | "Visão única", "postura de segurança" |
| **Detective** | Investiga a **causa raiz** de um problema de segurança | "Causa raiz", "investigar" |

## G Do alerta à investigação {span=2}
```flow LR
GuardDuty -> *Security Hub: envia alertas
Inspector -> Security Hub: envia vulnerabilidades
Macie -> Security Hub: envia dados sensíveis
Security Hub -> Detective: investiga a causa raiz
```
- O **Security Hub** agrega. O **Detective** investiga.
- **Config** responde "o que mudou". **GuardDuty** responde "algo está errado".

## H Conformidade {span=1}
- **Artifact**: portal para baixar **relatórios** de conformidade (SOC, PCI, ISO) e aceitar acordos.
- **Audit Manager**: coleta **evidências** de forma contínua e gera relatórios de auditoria.
- Dica: relatório da AWS = **Artifact**. Evidência dos **seus** recursos = **Audit Manager**.

## I Abuso e teste de invasão {span=3}
| Situação | Regra |
|---|---|
| Suspeita de **abuso** (spam, malware, ataque a partir de recursos AWS) | Reporte ao **contato de abuso** da AWS (**AWS Abuse**) |
| Teste de invasão nos **seus** recursos, em serviços permitidos (EC2, RDS, Aurora, CloudFront, API Gateway, Lambda, Lightsail, Elastic Beanstalk) | ok Permitido **sem aprovação prévia** |
| **DoS**, DDoS, flooding de portas, de protocolo ou de requisições | no **Proibido** |
| **Zone walking** em zonas hospedadas do Route 53 | no **Proibido** |

- A lista de serviços permitidos cresce com o tempo. Confira a política de **penetration testing** da AWS.

## J Resumo

```callout warn Pegadinhas de prova
- "Segurança **DA** nuvem" = AWS. "Segurança **NA** nuvem" = você.
- "Patch do SO da EC2" = **você**. "Patch do RDS" = **AWS**.
- "DDoS grátis" = **Shield Standard**. "Proteção de custo e DRT" = **Shield Advanced**.
- "SQL injection e XSS" = **WAF**. "Firewall da VPC" = **Network Firewall**. "Regras em várias contas" = **Firewall Manager**.
- "AWS gerencia as chaves" = **KMS**. "Você gerencia, hardware dedicado" = **CloudHSM**.
- "Certificado TLS" = **ACM**. "Rotacionar senhas" = **Secrets Manager**.
- "Ameaças com ML" = **GuardDuty**. "Vulnerabilidades" = **Inspector**. "PII no S3" = **Macie**.
- "Histórico de configuração" = **Config**. "Painel central" = **Security Hub**. "Causa raiz" = **Detective**.
- "Relatórios de conformidade" = **Artifact**. "Evidências de auditoria" = **Audit Manager**.
- Teste de invasão em serviços permitidos **não** exige aprovação. DoS é **proibido**.
```
