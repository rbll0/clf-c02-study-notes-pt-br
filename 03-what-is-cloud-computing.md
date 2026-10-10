---
title: O que é Cloud Computing?
subtitle: Seção 3 do CLF-C02 — do datacenter próprio à nuvem AWS
cols: 3
exam: 1 · Conceitos de nuvem (24%)
---
Cloud computing é a **entrega sob demanda** de computação, armazenamento, banco de dados e apps pela internet, com **pagamento pelo uso** (pay-as-you-go).

## A Como funcionava a TI tradicional {span=2}
```flow LR
(Cliente / navegador) -> Rede e roteador: pede pelo IP
Rede e roteador -> *Servidor: entrega o pedido
Servidor -> [(Banco de dados)]
Servidor --> (Cliente / navegador): resposta
group Datacenter próprio: Rede e roteador, Servidor, Banco de dados
```
Um servidor tem CPU, RAM, storage, banco de dados e rede. A empresa compra, instala e mantém tudo isso.

## B Problemas da TI tradicional
- Você paga aluguel, energia, refrigeração e manutenção.
- Trocar ou adicionar hardware demora.
- A capacidade é fixa. Escalar é difícil.
- Uma equipe monitora a infraestrutura 24/7.
- Desastres (incêndio, terremoto) derrubam tudo.

**Pergunta da prova**: dá para terceirizar isso? A resposta é a nuvem.

## C As 5 características da nuvem {span=2}
| Característica | Significado |
|---|---|
| Autosserviço sob demanda | Você cria recursos sem falar com ninguém da AWS |
| Amplo acesso à rede | Recursos acessíveis pela rede, de vários dispositivos |
| Multi-tenancy e pool de recursos | Vários clientes dividem a mesma infraestrutura com segurança e privacidade |
| Elasticidade e escalabilidade rápidas | Recursos crescem e diminuem com a demanda, automaticamente |
| Serviço medido | Você paga exatamente o que usa |

## D As 6 vantagens (cai muito!)
1. Troque **CapEx** por **OpEx**: pague sob demanda, sem comprar hardware.
2. Ganhe com a **economia de escala**: a AWS é eficiente e baixa preços.
3. **Pare de adivinhar capacidade**: escale pelo uso real.
4. Aumente **velocidade e agilidade**.
5. **Pare de gastar com datacenter**.
6. **Fique global em minutos**.

## E Tipos de serviço: o que você gerencia {span=3}
| Camada | On-premises | IaaS | PaaS | SaaS |
|---|---|---|---|---|
| Aplicação | warn Você | warn Você | warn Você | ok AWS |
| Dados | warn Você | warn Você | warn Você | ok AWS |
| Runtime e middleware | warn Você | warn Você | ok AWS | ok AWS |
| Sistema operacional | warn Você | warn Você | ok AWS | ok AWS |
| Virtualização | warn Você | ok AWS | ok AWS | ok AWS |
| Servidores, storage, rede | warn Você | ok AWS | ok AWS | ok AWS |
| **Exemplo AWS** | — | EC2 | Elastic Beanstalk | Rekognition, Gmail (fora da AWS) |

Fora da AWS: IaaS = Azure VMs, Google Compute Engine · PaaS = Heroku · SaaS = Zoom, Dropbox.

## F Modelos de implantação
| Modelo | Quem usa | Ponto-chave |
|---|---|---|
| Privada | Uma organização | Controle total, segurança para apps sensíveis |
| Pública | Vários clientes | Recursos de terceiros (AWS, Azure, GCP) pela internet |
| Híbrida | Mistura | Parte on-premises, parte na nuvem |

## G Fundamentos de preço
| Você paga por | Exemplo |
|---|---|
| Compute | Tempo de execução do servidor |
| Storage | Dados guardados |
| Saída de dados | ok Tráfego que **sai** da nuvem |
| Entrada de dados | ok **Grátis** |

Isso resolve o custo da TI tradicional: você não paga por capacidade parada.

## H Infraestrutura global da AWS {span=2}
```tree
AWS Global Infrastructure
  Região | Cluster de datacenters com nome. Ex.: us-east-1, sa-east-1 (São Paulo)
    Availability Zone | Um ou mais datacenters isolados. Normalmente 3 (mín. 3, máx. 6) por região
      Datacenter | Energia, rede e refrigeração redundantes
  Edge Location / PoP | Centenas de pontos que entregam conteúdo com baixa latência
```
- AZs ficam separadas para resistir a desastres e se ligam por rede de alta banda e baixa latência.
- A maioria dos serviços é **regional**. Alguns são **globais**: IAM, Route 53, CloudFront, WAF.

## I Como escolher uma região
| Fator | Exemplo |
|---|---|
| Compliance | Lei exige que dados fiquem no país |
| Proximidade | Usuários perto = menos latência |
| Serviços disponíveis | Nem todo serviço existe em toda região |
| Preço | O preço muda por região |

## J Responsabilidade compartilhada e AUP {span=3}
| Quem | Responsável por | Exemplo |
|---|---|---|
| Cliente | Segurança **NA** nuvem | Dados, IAM, SO do EC2, Security Groups, criptografia |
| AWS | Segurança **DA** nuvem | Hardware, rede global, datacenters, software dos serviços gerenciados |

**Acceptable Use Policy (AUP)**: é proibido usar a AWS para atividade ilegal, nociva ou ofensiva, violar segurança, abusar da rede ou enviar spam.

```callout info Para fixar
Pergunta com "pagar só pelo que usa" = pay-as-you-go / serviço medido. "Não adivinhar capacidade" = elasticidade. "Trocar investimento por despesa" = CapEx → OpEx. "Dados não podem sair do país" = escolha da região por compliance.
```
