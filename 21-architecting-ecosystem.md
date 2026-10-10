---
title: Architecting e Ecossistema
subtitle: Seção 21 do CLF-C02 — Well-Architected, CAF, migração e recursos de apoio
cols: 3
exam: Domínios 1 e 3 · Conceitos de nuvem (24%) e Tecnologia e serviços (34%)
---
Esta seção responde a duas perguntas: **como projetar bem** (Well-Architected) e **como migrar e adotar a nuvem** (CAF e 7 Rs). A prova cobra os **nomes** e as **ideias-chave**.

## A Well-Architected Framework: os 6 pilares {span=3}
| Pilar | Ideia central | Pergunta de design |
|---|---|---|
| **Excelência operacional** | Executar e **melhorar** operações de forma contínua | Você automatiza e usa infraestrutura como código? |
| **Segurança** | Proteger **dados, sistemas e ativos** | Você usa o menor privilégio e criptografa os dados? |
| **Confiabilidade** | **Recuperar-se** de falhas e atender à demanda | O sistema sobrevive à queda de uma AZ? |
| **Eficiência de desempenho** | Usar recursos **de forma eficiente** | Você escolheu o tipo de recurso certo para a carga? |
| **Otimização de custos** | Evitar **gastos desnecessários** | Você paga só pelo que usa? |
| **Sustentabilidade** | Reduzir o **impacto ambiental** | Você usa o mínimo de recursos para o trabalho? |

- Antes eram **5 pilares**. A **Sustentabilidade** é o sexto.
- O framework **não é uma auditoria**. É um guia de **boas práticas**.

## B Princípios de design na nuvem {span=2}
- **Pare de adivinhar a capacidade**: escale sob demanda.
- **Teste em escala de produção** e descarte depois.
- **Automatize** para facilitar a experimentação.
- Use **arquiteturas evolutivas**.
- Deixe a arquitetura **orientada a dados**.
- Faça **game days** para treinar falhas.
- Use **serviços gerenciados** para reduzir a carga operacional.

## C Ferramentas de arquitetura
- **Well-Architected Tool**: console para **revisar** suas cargas contra os 6 pilares. Ele gera **recomendações**.
- **Architecture Center**: **diagramas de referência** e boas práticas.
- **Prescriptive Guidance**: guias para **migração** e modernização.
- **Trusted Advisor**: **verifica sua conta** e recomenda ações. Ele cobre custo, desempenho, segurança, tolerância a falhas, limites de serviço e excelência operacional.

## D Cloud Adoption Framework (CAF) {span=3}
O **CAF** orienta a **transformação para a nuvem** com **6 perspectivas**.

| Perspectiva | Foco | Exemplos |
|---|---|---|
| **Negócio** | Alinhar tecnologia e **estratégia** | Gestão de estratégia, portfólio, inovação |
| **Pessoas** | **Cultura** e habilidades | Treinamento, gestão de mudança |
| **Governança** | **Gerenciar** a TI e **riscos** | Gestão de programa, de risco, de custo |
| **Plataforma** | **Arquitetura** e infraestrutura técnica | Projeto de arquitetura, provisionamento |
| **Segurança** | **Proteger** dados e cargas | Gestão de identidade, prevenção e resposta a incidentes |
| **Operações** | **Operar** serviços na nuvem | Monitoramento, gestão de incidentes, gestão de mudança |

- As perspectivas de **Negócio, Pessoas e Governança** são **de negócio**.
- As de **Plataforma, Segurança e Operações** são **técnicas**.

## E Os 7 Rs da migração {span=3}
| Estratégia | O que significa | Esforço |
|---|---|---|
| **Retire** | **Desligar** o que não precisa mais | Baixo |
| **Retain** | **Manter** no ambiente atual por enquanto | Nenhum |
| **Relocate** | Mover para a nuvem **sem mudar nada** (por exemplo, VMware Cloud on AWS) | Baixo |
| **Rehost** | **Lift and shift**: mover para EC2 sem alterar | Baixo |
| **Repurchase** | Trocar por **SaaS** (por exemplo, um CRM) | Médio |
| **Replatform** | **Lift, tinker and shift**: pequenas mudanças (por exemplo, usar RDS) | Médio |
| **Refactor** | **Reprojetar** a app para a nuvem (microsserviços, serverless) | Alto |

## F Ferramentas de migração {span=2}
```flow LR
Ambiente atual -> *Application Discovery Service: descobre servidores
Application Discovery Service -> Migration Hub: acompanha o progresso
Migration Hub -> Application Migration Service: executa o rehost
Application Migration Service -> Instâncias EC2: destino
```
- **Application Discovery Service**: **coleta dados** dos servidores locais para planejar.
- **Migration Hub**: **painel único** para acompanhar migrações.
- **Application Migration Service (MGN)**: faz o **lift and shift** de servidores para a AWS.
- **DMS** migra bancos. **Snow Family** e **DataSync** movem dados.

## G Ecossistema e suporte {span=3}
| Recurso | O que oferece |
|---|---|
| **AWS Marketplace** | **Catálogo digital** de software de terceiros. A cobrança entra na **fatura da AWS** |
| **AWS re:Post** | **Comunidade** de perguntas e respostas técnicas |
| **Professional Services** | **Consultores da AWS** para projetos de adoção da nuvem |
| **Partner Network (APN)** | **Parceiros** que constroem e vendem soluções na AWS |
| **Well-Architected Review** | Revisão guiada de uma carga contra os pilares |
| **Trusted Advisor** | Verificações automáticas de **boas práticas** na sua conta |

```callout warn Pegadinhas de prova
- "6 pilares" = excelência operacional, segurança, confiabilidade, eficiência de desempenho, custos, **sustentabilidade**.
- "Revisar a carga contra os pilares" = **Well-Architected Tool**.
- "6 perspectivas" = **CAF**: negócio, pessoas, governança, plataforma, segurança, operações.
- "Lift and shift" = **Rehost**. "Trocar por SaaS" = **Repurchase**. "Reprojetar" = **Refactor**.
- "Desligar o que não serve" = **Retire**. "Manter por ora" = **Retain**.
- "Software de terceiros com cobrança na fatura AWS" = **Marketplace**.
- "Consultoria da AWS" = **Professional Services**. "Comunidade técnica" = **re:Post**.
- "Verificar a conta e recomendar" = **Trusted Advisor**.
```
