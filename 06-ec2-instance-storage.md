---
title: EC2 Instance Storage
subtitle: Seção 6 do CLF-C02 — onde a instância guarda dados (EBS, EFS, Instance Store)
cols: 3
exam: Domínio 3 · Tecnologia e serviços (34%)
---
O EC2 guarda dados em **três tipos de disco**. A prova pede que você escolha o certo para cada caso: persistência, compartilhamento e desempenho.

## A Visão geral dos 3 tipos {span=3}
| | EBS | Instance Store | EFS |
|---|---|---|---|
| Tipo | Bloco (disco de rede) | Bloco (disco do hardware) | Arquivos (NFS de rede) |
| Persistência | ok Sobrevive ao stop e ao terminate (configurável) | no Perde tudo ao parar ou terminar | ok Persistente |
| Quantas instâncias | Uma por vez (CCP) | Uma | Centenas, ao mesmo tempo |
| Escopo | Uma AZ | A instância | Várias AZs |
| Desempenho | Bom | Muito alto (IOPS) | Bom, escala sozinho |
| Melhor para | Disco do SO e dados de banco | Cache e dados temporários | Arquivos compartilhados entre instâncias |

## B EBS — Elastic Block Store {span=2}
```flow LR
(Instância EC2) -> *Volume EBS: anexa
Volume EBS -> [(Snapshot)]: backup
Snapshot -> Volume EBS: restaura em outra AZ
group Mesma AZ: Instância EC2, Volume EBS
```
- O EBS é um **disco de rede**: se liga e desliga da instância rapidamente.
- Os dados **persistem** depois que a instância é terminada, se você desativar "Delete on Termination".
- O volume fica preso a **uma AZ**. Para mover, use um **snapshot**.
- Você provisiona **capacidade** (GB) e **IOPS** e paga pelo que provisionou.
- Dá para crescer o volume depois.

## C Delete on Termination
| Volume | Valor padrão | Efeito ao terminar a instância |
|---|---|---|
| Raiz (root) | ok Ativado | O volume é **apagado** |
| Extra (dados) | no Desativado | O volume **permanece** |

Desative a opção no volume raiz quando precisar preservar os dados.

## D Snapshots do EBS
- Um snapshot é um **backup** do volume em um ponto no tempo.
- Você pode copiá-lo para **outra AZ ou região**.
- Você **não** precisa desanexar o volume para tirar o snapshot, mas é recomendado.
- Os recursos do snapshot:
  - **Archive**: armazenamento até 75% mais barato, com restauração em 24 a 72 horas.
  - **Recycle Bin**: guarda snapshots apagados por um período e permite restaurar. Você define a retenção, de 1 dia a 1 ano.
  - **Fast Snapshot Restore**: restauração sem latência, com custo extra.

## E AMI — Amazon Machine Image {span=2}
Uma AMI é a **imagem** de uma instância: SO, software e configuração. Ela acelera lançar várias instâncias iguais.

```flow LR
Instância configurada -> *AMI personalizada: cria
AMI personalizada -> Nova instância: lança
AMI personalizada -> Outra região: copia
```
| Origem da AMI | Exemplo |
|---|---|
| Pública (AWS) | Amazon Linux 2 |
| Sua (privada) | Imagem com seu software |
| Marketplace | Imagem vendida por terceiros |

- A AMI é **regional**. Para usar em outra região, copie.
- A AMI usa **snapshots** do EBS por baixo.

## F EC2 Image Builder
Serviço que **automatiza** a criação, o teste e a distribuição de AMIs.
- Ele cria uma instância, instala o software, testa e gera a AMI.
- O resultado vai para várias regiões e contas.
- Ele roda por **agenda**, e o serviço em si é gratuito (você paga pelos recursos).

## G Instance Store {span=2}
Disco **fisicamente ligado** ao servidor. É muito rápido, mas **efêmero**.

| Característica | Detalhe |
|---|---|
| Desempenho | Altíssimo, com IOPS maiores que o EBS |
| Persistência | no Os dados somem se a instância parar, for terminada ou o hardware falhar |
| Backup | Responsabilidade **sua** |
| Uso | Buffer, cache e dados temporários |

Nunca guarde dados que você precise manter.

## H EFS — Elastic File System {span=3}
É um sistema de arquivos de rede (**NFS**) gerenciado, que **várias instâncias** montam ao mesmo tempo, em **várias AZs**.

| Característica | Detalhe |
|---|---|
| Compatibilidade | Só **Linux** (POSIX) |
| Escala | Cresce e diminui sozinho. Você não provisiona capacidade |
| Preço | Pague pelo que usa. Custa cerca de 3× o EBS gp2 |
| Segurança | Criptografia com KMS e Security Groups |
| Casos de uso | Gerenciamento de conteúdo, web serving, compartilhamento de dados |

| Classe de armazenamento | Uso |
|---|---|
| EFS Standard | Arquivos acessados com frequência |
| EFS-IA (acesso infrequente) | Arquivos raros, com economia de até 92% |
| Lifecycle policy | Move o arquivo para IA após N dias sem acesso |

## I Outros serviços de arquivo
| Serviço | Para quê |
|---|---|
| Amazon FSx for Windows | Sistema de arquivos **Windows** (SMB, NTFS, Active Directory) |
| Amazon FSx for Lustre | **HPC** e machine learning (alta performance, Linux) |
| Amazon FSx for NetApp ONTAP | Compatível com NFS, SMB e iSCSI |
| Amazon FSx for OpenZFS | Compatível com NFS |

## J Responsabilidade compartilhada no storage {span=2}
| Quem | Cuida de |
|---|---|
| AWS | Replicar dados do EBS e do EFS entre AZs, trocar hardware com falha, manter a infraestrutura |
| Você | Fazer backup e snapshots, criptografar os dados, monitorar o uso e **não confiar** no Instance Store |

```callout warn Pegadinhas de prova
- "Disco que sobrevive à instância" = **EBS**.
- "Disco mais rápido, dados temporários" = **Instance Store**.
- "Vários servidores Linux no mesmo sistema de arquivos" = **EFS**.
- "Compartilhar arquivos no Windows" = **FSx for Windows**.
- "HPC e machine learning" = **FSx for Lustre**.
- "Backup do EBS" = **snapshot**.
- "Mover o EBS para outra AZ" = snapshot + novo volume.
```
