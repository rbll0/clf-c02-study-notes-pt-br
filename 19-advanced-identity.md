---
title: Advanced Identity
subtitle: Seção 19 do CLF-C02 — Identity Center, Cognito, Directory Services, STS e RAM
cols: 3
exam: Domínio 2 · Segurança e conformidade (30%)
---
O **IAM** cuida de usuários **dentro de uma conta**. Para **vários usuários, várias contas ou clientes do seu app**, a AWS oferece serviços de identidade **avançados**. Memorize **quem** cada serviço atende: funcionários, clientes ou um AD existente.

## A Qual serviço de identidade usar {span=3}
| Necessidade | Serviço | Quem usa |
|---|---|---|
| Login único (SSO) em **várias contas AWS** e apps | **IAM Identity Center** | Funcionários |
| Cadastro e login dos **clientes do seu app** web ou mobile | **Cognito** | Usuários finais |
| Active Directory na AWS | **Directory Services** | Funcionários e servidores Windows |
| Credenciais **temporárias** para acessar a AWS | **STS** | Roles, apps e usuários federados |
| Compartilhar um recurso entre contas | **RAM** | Contas da mesma organização |

## B IAM Identity Center {span=2}
O **IAM Identity Center** é o sucessor do **AWS SSO**. O usuário entra **uma vez** e acessa tudo o que tem permissão.

```flow LR
Usuário -> *IAM Identity Center: login único
IAM Identity Center -> Conta AWS A: permission set
IAM Identity Center -> Conta AWS B: permission set
IAM Identity Center -> Apps SaaS: Salesforce, Microsoft 365
Provedor de identidade --> IAM Identity Center: Active Directory, Okta
```
- Ele se integra ao **AWS Organizations** e cobre **todas as contas** da organização.
- O **permission set** define o que o usuário pode fazer em cada conta.
- Ele guarda usuários e grupos **próprios** ou usa um **provedor externo**.
- Não há custo adicional pelo serviço.

## C Amazon Cognito
- Dá **cadastro, login e acesso** a usuários de apps web e mobile.
- Os usuários são **clientes**, não funcionários.
- **User pools**: diretório de usuários, login e tokens (JWT).
- **Identity pools**: trocam o login por **credenciais temporárias da AWS**.
- Aceita login **social** (Google, Facebook) e **SAML**.

## D AWS Directory Services {span=3}
| Opção | O que faz | Use quando |
|---|---|---|
| **AWS Managed Microsoft AD** | Cria um **AD de verdade** na AWS | Você quer um AD na nuvem. Pode ter **relação de confiança** com o AD local |
| **AD Connector** | **Proxy** que encaminha pedidos ao AD local | Você mantém o AD **no datacenter** e não quer replicá-lo |
| **Simple AD** | AD **básico** compatível, sem o AD da Microsoft | Você precisa de poucos recursos de diretório e não tem AD local |

- O AD guarda **objetos** (usuários, contas, computadores) em uma estrutura central.
- O **AD Connector** não guarda dados. Ele só repassa as autenticações.

## E STS e federação {span=2}
```flow LR
Usuário federado -> *STS: AssumeRole
STS -> Usuário federado: credenciais temporárias
Usuário federado -> Serviço AWS: acessa com a role
```
- O **STS** (Security Token Service) emite credenciais **temporárias** e de **curta duração**.
- A chamada principal é **AssumeRole**.
- **Role cross-account**: uma conta assume uma role em **outra conta**.
- **Federação**: o usuário entra com uma identidade **externa** (SAML 2.0, OIDC) e recebe acesso sem ter um usuário IAM.
- Credenciais temporárias evitam **chaves de longo prazo**.

## F Resource Access Manager (RAM)
- Compartilha recursos com **outras contas** ou com a **organização inteira**.
- Evita **duplicar** recursos em cada conta.
- Exemplos: **subnets de VPC**, Transit Gateway, regras de Route 53 Resolver, License Manager.
- Os recursos continuam **na conta dona**. Os outros só os usam.
- Não há custo adicional pelo RAM.

## G Funcionários vs clientes {span=3}
| | IAM Identity Center | Cognito | IAM comum |
|---|---|---|---|
| Para quem | Funcionários | **Clientes** do seu app | Usuários e roles de **uma conta** |
| Escopo | **Várias contas** AWS e apps SaaS | Seu **app** | Uma conta |
| Resultado | Acesso SSO ao console e apps | Tokens e credenciais temporárias | Permissões diretas |
| Fonte de identidade | Próprio, AD ou IdP externo | Próprio, social ou SAML | IAM |

```callout warn Pegadinhas de prova
- "SSO para várias contas AWS e apps" = **IAM Identity Center**.
- "Login dos clientes do app web ou mobile" = **Cognito**.
- "Active Directory na AWS" = **Managed Microsoft AD**. "AD já existe no datacenter" = **AD Connector**.
- "Credenciais temporárias" = **STS**. "Acesso entre contas sem chaves" = **role** com **AssumeRole**.
- "Compartilhar subnet ou recurso entre contas" = **RAM**.
- O **Identity Center** substitui o nome antigo **AWS SSO**.
```
