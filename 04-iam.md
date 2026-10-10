---
title: IAM — Identity and Access Management
subtitle: Seção 4 do CLF-C02 — quem pode fazer o quê na sua conta AWS
cols: 3
exam: Domínio 2 · Segurança e compliance (30%)
---
IAM controla **quem** acessa a AWS e **o que** essa pessoa ou serviço pode fazer. É um serviço **global** e **gratuito**.

## A Os 4 blocos do IAM {span=2}
```flow LR
Usuário -> Grupo: pertence a
Grupo -> *Política: recebe
Role -> Política: recebe
(Serviço AWS) -> Role: assume
```
- **Usuário**: uma pessoa, com senha e/ou chaves de acesso.
- **Grupo**: um conjunto de usuários. Contém só usuários, nunca outros grupos.
- **Política**: um documento JSON com as permissões.
- **Role**: uma identidade temporária para serviços da AWS, como o EC2.

## B Conta root vs usuário IAM
| | Root | Usuário IAM |
|---|---|---|
| Criação | Automática, ao abrir a conta | Você cria |
| Poder | Total, sem limites | Só o que as políticas liberam |
| Uso diário | no Nunca | ok Sim |
| Quando usar | Tarefas raras, como fechar a conta ou mudar o plano de suporte | Todo o resto |

Proteja o root com **MFA** e não crie chaves de acesso para ele.

## C Usuários e grupos {span=2}
- Um usuário pode estar em **vários grupos**.
- Um usuário pode **não ter grupo** (não recomendado).
- Os grupos organizam permissões por função: Devs, Operações, Auditoria.
- Anexe a política **ao grupo**, não a cada usuário.

```callout info Exemplo
Alice e Bob estão no grupo Devs. Charles está em Operações. David não tem grupo. Alice também está em Auditoria. Ela recebe as permissões dos dois grupos.
```

## D Políticas IAM: a estrutura JSON {span=2}
| Campo | Significado |
|---|---|
| Version | Versão da linguagem de política (`2012-10-17`) |
| Statement | Lista de regras |
| Effect | `Allow` ou `Deny` |
| Principal | Conta, usuário ou role a que a regra se aplica |
| Action | Chamadas de API permitidas ou negadas (`s3:GetObject`) |
| Resource | Recursos afetados (`arn:aws:s3:::meu-bucket/*`) |
| Condition | Quando a regra vale (IP, hora, MFA) |

## E Princípio do menor privilégio
Dê **só** as permissões que a pessoa precisa para o trabalho. Nada além disso.

| Situação | Avaliação |
|---|---|
| Dev recebe `AdministratorAccess` por conveniência | no Viola o menor privilégio |
| Dev recebe acesso só ao bucket do projeto | ok Correto |
| Um `Deny` explícito entra em conflito com um `Allow` | warn O Deny sempre vence |

## F Proteção de conta {span=3}
| Ferramenta | O que faz |
|---|---|
| Política de senha | Exige tamanho mínimo, tipos de caractere, expiração e proíbe reuso |
| MFA (Multi-Factor Authentication) | Senha **+** dispositivo de segurança. Se a senha vazar, a conta continua protegida |
| Virtual MFA | App no celular: Google Authenticator, Authy |
| U2F Security Key | Chave física: YubiKey |
| Hardware Key Fob MFA | Token físico: Gemalto |

## G Formas de acessar a AWS {span=2}
| Forma | Protegida por | Para que serve |
|---|---|---|
| Management Console | Senha + MFA | Uso interativo no navegador |
| AWS CLI | Chaves de acesso | Comandos no terminal e scripts |
| AWS SDK | Chaves de acesso | Chamadas de API dentro do código (Python, JS, Java…) |
| CloudShell | Login no console | Terminal no navegador, já autenticado |

**Chaves de acesso**: Access Key ID (parece um usuário) + Secret Access Key (parece uma senha). Elas são **segredo pessoal**: não compartilhe e não suba para o Git.

## H Roles do IAM
Uma role dá permissões a **serviços da AWS**, sem senha ou chave guardada.

| Caso | Role |
|---|---|
| Instância EC2 acessa um bucket S3 | Role de EC2 com política de S3 |
| Função Lambda grava no DynamoDB | Role da Lambda |
| Serviço da AWS age em seu nome | Role de serviço |

## I Ferramentas de segurança do IAM {span=3}
| Ferramenta | Função |
|---|---|
| Credential Report | Lista **todos** os usuários da conta e o estado das credenciais (nível de conta) |
| Access Advisor | Mostra os serviços que um usuário usou e quando (nível de usuário). Serve para reduzir permissões |
| Access Analyzer | Encontra recursos compartilhados com entidades externas |
| IAM Identity Center | Login único (SSO) para várias contas |

## J Responsabilidade compartilhada no IAM {span=2}
| Quem | Cuida de |
|---|---|
| AWS | Infraestrutura do IAM, rede global e software dos serviços |
| Você | Criar usuários, grupos e roles. Atualizar permissões. Girar chaves. Ativar MFA. Usar o menor privilégio. Auditar a conta |

```callout warn Pegadinhas de prova
Root = só para tarefas raras. Grupo = só usuários. Role = para serviços, sem credencial fixa. MFA = senha + dispositivo. Access Advisor = permissões usadas por **usuário**. Credential Report = estado das credenciais da **conta**.
```
