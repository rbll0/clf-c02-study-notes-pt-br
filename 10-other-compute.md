---
title: Outros serviços de computação
subtitle: Seção 10 do CLF-C02 — ECS, Fargate, ECR, EKS, Lambda, Batch e Lightsail
cols: 3
exam: Domínio 3 · Tecnologia e serviços (34%)
---
Além do EC2, a AWS oferece **containers**, **funções serverless** e **serviços prontos**. A prova pede que você escolha o serviço certo para cada cenário. Quanto mais **serverless**, menos você gerencia.

## A Containers e Docker {span=2}
O **Docker** empacota o app e suas dependências em uma **imagem**. A imagem roda igual em qualquer máquina.

```flow LR
Imagem Docker -> *Container: roda
Container -> Servidor: vários containers por servidor
```
- Containers são **leves** e iniciam rápido.
- Vários containers dividem o **mesmo servidor**. Isso aproveita melhor o hardware.
- A AWS oferece **ECS** e **EKS** para orquestrar containers.
- O **ECR** guarda as imagens.

## B ECS, Fargate, ECR e EKS {span=3}
| Serviço | Função | Ponto-chave |
|---|---|---|
| **ECS** (Elastic Container Service) | Executa containers Docker | Orquestrador **da AWS**. Integra com ALB, IAM e CloudWatch |
| **EKS** (Elastic Kubernetes Service) | Executa containers com **Kubernetes** | Kubernetes é **open source** e **portável** entre nuvens |
| **Fargate** | Motor **serverless** para ECS e EKS | Sem servidores para gerenciar. Você paga por **vCPU e memória** |
| **ECR** (Elastic Container Registry) | **Registro** de imagens Docker | Repositórios privados e públicos |

- No ECS com **EC2**, você cria e gerencia as instâncias.
- No ECS com **Fargate**, você só define **CPU e memória** do container.
- O ECS e o EKS aceitam **Fargate** ou **EC2**.

## C ECS: EC2 vs Fargate {span=1}
| | EC2 | Fargate |
|---|---|---|
| Servidores | Você gerencia | **A AWS gerencia** |
| Controle | Alto | Menor |
| Cobrança | Instâncias | CPU e memória |

## D AWS Lambda {span=2}
O **Lambda** executa **funções** em resposta a **eventos**. Você não vê nem gerencia servidores.

```flow LR
Evento S3 -> *Lambda: dispara
API Gateway -> Lambda: requisição HTTP
Lambda -> DynamoDB: grava
Lambda --> Cliente: resposta
```
- **Serverless**: sem servidor para provisionar.
- **Event-driven**: roda quando algo acontece (upload no S3, chamada de API, agendamento).
- Você paga **só pelo tempo de execução** e pelo **número de requisições**.
- O tempo máximo é de **15 minutos** por execução.
- Escala **automaticamente**, de zero a milhares de execuções.
- Suporta Node.js, Python, Java, C#, Go, Ruby e **imagens de container**.

## E Lambda: preço e uso
- Há um **nível gratuito** mensal de requisições e de tempo de computação.
- O **API Gateway** cria APIs REST e HTTP e chama o Lambda. Ele também é **serverless**.
- Use o **EventBridge** para rodar funções em horários agendados.
- Casos comuns: thumbnails de imagens, processamento de arquivos, back-end de APIs, automações.

## F Lambda vs EC2 {span=3}
| | **Lambda** | **EC2** |
|---|---|---|
| Servidor | **Nenhum** para gerenciar | Instância que você gerencia |
| Duração | Até **15 minutos** | Sem limite |
| Execução | Sob demanda, por evento | Contínua |
| Escala | Automática | Manual ou com ASG |
| Cobrança | Por **requisição e tempo** | Por **tempo de instância** |
| Melhor para | Tarefas curtas e orientadas a eventos | Apps contínuos e de longa duração |

## G AWS Batch {span=2}
O **Batch** roda **jobs em lote** de qualquer tamanho. Ele cria os recursos de computação **dinamicamente**.

```flow LR
Job em lote -> *Batch: envia
Batch -> Instâncias EC2 ou Spot: provisiona sozinho
Batch -> Container Docker: executa o job
```
- Cada job roda como **imagem Docker**, em **EC2**, **Spot** ou **Fargate**.
- Ele escolhe o **tamanho** e a **quantidade** de instâncias conforme a fila.
- O Batch em si é **gratuito**. Você paga pelos recursos usados.
- Casos comuns: processamento de imagens, simulações, análises em lote.

## H Batch vs Lambda {span=1}
| | Batch | Lambda |
|---|---|---|
| Limite de tempo | **Nenhum** | 15 min |
| Execução | **Docker** em instâncias | Código em runtimes |
| Armazenamento | Disco EBS ou instance store | Efêmero e limitado |
| Servidor | Provisionado pelo Batch | Nenhum |

## I Lightsail e App Runner {span=3}
| Serviço | Para quem | O que oferece |
|---|---|---|
| **Lightsail** | Iniciantes com **pouca experiência em nuvem** | Servidor virtual, banco, armazenamento e rede com **preço baixo e previsível** |
| **App Runner** | Equipes que querem **publicar apps web e APIs** sem infraestrutura | Implanta a partir de **código ou imagem de container**. Escala e balanceia sozinho |

- O **Lightsail** inclui instâncias, bancos de dados, **load balancers** e DNS em um painel simples.
- Os recursos do Lightsail têm **integração limitada** com outros serviços AWS.
- Casos de uso do Lightsail: sites simples, blogs WordPress, ambientes de teste e protótipos.
- Não use o Lightsail para **alta escala** ou arquiteturas complexas.

## J Qual serviço de computação usar {span=3}
| Cenário | Serviço |
|---|---|
| Servidor completo, controle total | **EC2** |
| Containers Docker, orquestração da AWS | **ECS** |
| Containers sem gerenciar servidores | **ECS ou EKS com Fargate** |
| Kubernetes | **EKS** |
| Armazenar imagens Docker | **ECR** |
| Código curto disparado por evento | **Lambda** |
| Job em lote de longa duração | **Batch** |
| Site ou blog simples, preço previsível | **Lightsail** |
| Publicar app web a partir do código, sem infraestrutura | **App Runner** |

```callout warn Pegadinhas de prova
- "Containers **sem** gerenciar servidores" = **Fargate**.
- "**Kubernetes** gerenciado" = **EKS**. "Orquestrador próprio da AWS" = **ECS**.
- "Registro de imagens Docker" = **ECR**.
- "Função serverless por evento, até 15 minutos" = **Lambda**.
- "Job em lote **sem limite de tempo**" = **Batch**. O Batch **não** é serverless como o Lambda.
- "Pouca experiência em nuvem, preço previsível" = **Lightsail**.
- Lambda **não** serve para processos de **longa duração**.
```
