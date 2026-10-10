---
title: Deployments e infraestrutura em escala
subtitle: Seção 11 do CLF-C02 — CloudFormation, CDK, Beanstalk, CodeSuite e Systems Manager
cols: 3
exam: Domínio 3 · Tecnologia e serviços (34%)
---
Clicar no console não escala. Para repetir ambientes e implantar código com segurança, use **infraestrutura como código (IaC)**, **CI/CD** e **gerenciamento de frota**. Esta seção mostra qual serviço resolve cada parte.

## A Visão geral {span=3}
| Necessidade | Serviço | Em uma frase |
|---|---|---|
| Criar infraestrutura com **templates** | **CloudFormation** | IaC declarativa em JSON ou YAML |
| Criar infraestrutura com **linguagem de programação** | **CDK** | Gera templates do CloudFormation |
| Publicar um app sem gerenciar a infraestrutura | **Elastic Beanstalk** | PaaS para desenvolvedores |
| Automatizar build, teste e deploy | **CodeBuild, CodeDeploy, CodePipeline** | CI/CD gerenciado |
| Guardar **código-fonte** | **CodeCommit** | Repositório Git |
| Guardar **dependências** | **CodeArtifact** | Repositório de pacotes |
| Gerenciar e operar a **frota** de servidores | **Systems Manager** | Patches, comandos e acesso |

## B CloudFormation {span=2}
O **CloudFormation** cria recursos a partir de um **template**. O template descreve o **estado desejado**.

```flow LR
Template YAML ou JSON -> *CloudFormation: lê
CloudFormation -> Stack: cria e atualiza
Stack -> Recursos AWS: EC2, S3, RDS e mais
```
- O estilo é **declarativo**: você diz **o quê**, não **como**.
- Um conjunto de recursos criados juntos forma uma **stack**.
- Apagar a stack **apaga todos os recursos** dela.
- O serviço é **gratuito**. Você paga pelos **recursos** criados.
- Você **reutiliza** o mesmo template em outras regiões e contas.

## C Benefícios do CloudFormation
- **Repetível**: o mesmo template gera o mesmo ambiente.
- **Versionável**: guarde os templates no Git.
- **Controle de custo**: tags e estimativa de custo dos recursos.
- **Produtividade**: crie e destrua ambientes de teste em minutos.
- **Visual**: o **Infrastructure Composer** desenha o template em diagrama.
- **Desfaz sozinho** se a criação falhar (rollback).

## D CDK — Cloud Development Kit {span=2}
```flow LR
Código TypeScript, Python ou Java -> *CDK: define a infraestrutura
CDK -> Template CloudFormation: compila
Template CloudFormation -> Recursos AWS: implanta
```
- Você usa **linguagens de programação**: JavaScript, TypeScript, Python, Java e .NET.
- O CDK **gera** o template do CloudFormation.
- Ele permite **loops, condições e funções**, algo difícil em YAML.
- Você também implanta o **código do app** junto com a infraestrutura.

## E CloudFormation vs CDK {span=1}
| | CloudFormation | CDK |
|---|---|---|
| Entrada | JSON ou YAML | Linguagem de programação |
| Estilo | Declarativo | Código |
| Saída | Stack | Template do CloudFormation |

## F Elastic Beanstalk {span=2}
O **Beanstalk** publica o app e **cria a infraestrutura sozinho**. É um **PaaS** voltado ao desenvolvedor.

| Item | Detalhe |
|---|---|
| Cria | EC2, **Auto Scaling**, **ELB**, RDS e monitoramento |
| Você envia | Só o **código** |
| Plataformas | Go, Java, .NET, Node.js, PHP, Python, Ruby, Docker e outras |
| Custo | O serviço é **gratuito**. Você paga pelos **recursos** |
| Camadas | **Web server** e **worker** |

- Você **mantém o controle** dos recursos criados.
- O Beanstalk **usa o CloudFormation** por baixo.
- O foco é **rapidez** e **simplicidade** na publicação.

## G CI/CD: os serviços Code {span=3}
```flow LR
CodeCommit -> *CodePipeline: dispara
CodePipeline -> CodeBuild: compila e testa
CodeBuild -> CodeDeploy: aprovado
CodeDeploy -> Servidores: implanta
CodeArtifact --> CodeBuild: fornece dependências
```
| Serviço | Etapa | O que faz |
|---|---|---|
| **CodeCommit** | Fonte | Repositório **Git** gerenciado. warn A AWS **fechou** o serviço a novos clientes em 2024. Confirme o status atual |
| **CodeBuild** | Build | Compila, roda **testes** e gera artefatos. Cobra por **minuto** |
| **CodeDeploy** | Deploy | Implanta em **EC2, on-premises, Lambda e ECS** |
| **CodePipeline** | Orquestração | Liga as etapas em um **pipeline** de ponta a ponta |
| **CodeArtifact** | Dependências | Guarda **pacotes** (npm, Maven, pip) e controla versões |

## H Systems Manager (SSM) {span=2}
O **SSM** gerencia **frotas** de servidores, na AWS e **on-premises**. Cada servidor roda o **SSM Agent**.

| Recurso | Função |
|---|---|
| **Session Manager** | Abre um **shell** no servidor **sem SSH**, sem porta 22 e sem bastion |
| **Patch Manager** | Aplica **patches** em toda a frota de forma automática |
| **Run Command** | Roda um comando em **vários servidores** de uma vez |
| **Parameter Store** | Guarda **configurações e segredos** |
| **Maintenance Windows** | Agenda tarefas de manutenção |

- Funciona com Linux, Windows, macOS e Raspberry Pi.
- O **Session Manager** registra as sessões no **S3** ou no **CloudWatch Logs**. Ele também usa **IAM** para o acesso.

## I OpsWorks {span=1}
- Serviço de **Chef** e **Puppet** gerenciados.
- Ele automatiza a **configuração** de servidores.
- warn Os serviços **OpsWorks** foram **encerrados** (Chef em 2024 e Puppet em 2025). Confirme as datas na AWS.
- Na prova, "**Chef ou Puppet**" aponta para o **OpsWorks**.

## J Qual usar {span=3}
| Cenário | Serviço |
|---|---|
| Recriar o mesmo ambiente em várias contas e regiões | **CloudFormation** |
| Definir a infraestrutura em **Python ou TypeScript** | **CDK** |
| Publicar um app web rapidamente, sem cuidar da infra | **Elastic Beanstalk** |
| Pipeline de ponta a ponta | **CodePipeline** |
| Compilar e testar o código | **CodeBuild** |
| Implantar em EC2 e on-premises | **CodeDeploy** |
| Repositório de pacotes e dependências | **CodeArtifact** |
| Shell seguro em servidor, sem SSH | **SSM Session Manager** |
| Atualizar patches de toda a frota | **SSM Patch Manager** |

```callout warn Pegadinhas de prova
- "**Infraestrutura como código**" = **CloudFormation**. "Com linguagem de programação" = **CDK**.
- O **CDK** gera um template do **CloudFormation**.
- "Só o código, a AWS cuida do resto" = **Elastic Beanstalk** (PaaS).
- "**CI/CD de ponta a ponta**" = **CodePipeline**. "Build" = **CodeBuild**. "Deploy" = **CodeDeploy**.
- "Repositório **Git**" = **CodeCommit**. "Pacotes e dependências" = **CodeArtifact**.
- "Acesso ao servidor **sem SSH**" = **Session Manager**.
- "Patch em lote" = **Patch Manager**. "Chef ou Puppet" = **OpsWorks**.
- **CloudFormation** e **Beanstalk** são gratuitos. Você paga pelos recursos.
```
