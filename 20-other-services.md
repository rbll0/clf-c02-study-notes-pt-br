---
title: Other Services
subtitle: Seção 20 do CLF-C02 — migração de dados, arquivos, backup, desktops, apps e mídia
cols: 3
exam: Domínio 3 · Tecnologia e serviços (34%)
---
Esta seção reúne serviços **de nicho**. A prova pergunta o **caso de uso** de cada um. Grave a **frase-gatilho** de cada serviço.

## A Mover dados para a AWS {span=3}
| Serviço | O que faz | Gatilho |
|---|---|---|
| **Snowcone** | Dispositivo **pequeno** (cerca de 8 TB em HDD) para borda e transferência | "Dispositivo pequeno e portátil" |
| **Snowball Edge** | Dispositivo para **terabytes a petabytes**: Storage Optimized e Compute Optimized | "Muitos TB, sem rede boa" |
| **Snowmobile** | **Caminhão** com até **100 PB** | "Exabytes", "centros de dados inteiros" |
| **DataSync** | Transfere dados **online** e agendados, de local para AWS ou entre serviços | "Mover dados pela rede, agendado" |
| **Transfer Family** | **SFTP, FTPS e FTP** para S3 ou EFS | "FTP" |
| **Storage Gateway** | Ponte **híbrida** entre local e nuvem | "Armazenamento híbrido" |

- Use **Snow Family** quando a rede leva **semanas** ou não existe.
- O **Snowball Edge** também **processa** dados na borda, com instâncias EC2 e Lambda.
- Prefira **DataSync** quando a rede aguenta a carga.
- Dispositivos Snow e capacidades variam por versão. Confira os números atuais na documentação.

## B Storage Gateway {span=2}
```flow LR
Datacenter local -> *Storage Gateway: protocolos locais
Storage Gateway -> Amazon S3: arquivos e fitas
Storage Gateway -> Amazon EBS: volumes
Storage Gateway --> Amazon S3 Glacier: tapes
```
| Tipo | Uso |
|---|---|
| **File Gateway** | Arquivos (NFS, SMB) com backup no **S3** |
| **Volume Gateway** | Volumes de bloco (iSCSI) com snapshots no **S3** |
| **Tape Gateway** | Biblioteca de **fitas virtuais** (VTL) para backup |

## C Sistemas de arquivos {span=3}
| Serviço | Tipo | Quando usar |
|---|---|---|
| **EFS** | **NFS** gerenciado, Linux, multi-AZ, cresce sozinho | Arquivos compartilhados entre **várias instâncias EC2** Linux |
| **FSx for Windows File Server** | **SMB** e Active Directory | Workloads **Windows** |
| **FSx for Lustre** | Sistema de arquivos **paralelo**, de alto desempenho | HPC, machine learning e **processamento rápido** |
| **FSx for NetApp ONTAP** | Compatível com NetApp | Migrar sistemas **NetApp** |
| **FSx for OpenZFS** | Compatível com ZFS | Migrar sistemas **ZFS** |

- **EBS** é disco de **uma instância** (uma AZ). **EFS** é arquivo **compartilhado** entre várias.

## D Backup e recuperação de desastre {span=2}
| Serviço | Função |
|---|---|
| **AWS Backup** | Faz backup **centralizado** de vários serviços, com **políticas** e agendamento |
| **Elastic Disaster Recovery** | Replica servidores **continuamente** e recupera em **minutos** |

| Estratégia de DR | Custo | Recuperação |
|---|---|---|
| **Backup and restore** | Mais baixo | Mais lenta |
| **Pilot light** | Baixo | Núcleo mínimo ligado |
| **Warm standby** | Médio | Versão reduzida rodando |
| **Multi-site (ativo-ativo)** | Mais alto | Quase imediata |

## E Desktops e apps remotos
- **WorkSpaces**: **desktop virtual** (Windows ou Linux) na nuvem. Use no lugar de PCs físicos e para trabalho remoto.
- **AppStream 2.0**: **transmite um app** para o navegador, sem instalar nada.
- **WorkSpaces** entrega o **desktop inteiro**. **AppStream** entrega **um aplicativo**.

## F Apps web e mobile {span=3}
| Serviço | Função | Gatilho |
|---|---|---|
| **API Gateway** | Cria e publica **APIs** REST, HTTP e WebSocket, serverless | "Criar API" |
| **Amplify** | Cria apps **web e mobile** com backend, hospedagem e CI/CD | "Full-stack web e mobile rápido" |
| **AppSync** | API **GraphQL** gerenciada, com dados em tempo real | "GraphQL" |
| **Device Farm** | Testa apps em **dispositivos reais** e navegadores | "Testar em celulares reais" |
| **Step Functions** | Orquestra **fluxos** com vários serviços (workflows) | "Orquestrar passos" |

## G IoT, mídia e comunicação {span=2}
| Serviço | Função |
|---|---|
| **IoT Core** | Conecta **dispositivos IoT** à nuvem com segurança |
| **MediaConvert** | **Transcodifica** vídeo para vários formatos |
| **Elastic Transcoder** | Serviço **antigo** de transcodificação. O MediaConvert o substitui |
| **SES** | Envia **e-mail** em massa (marketing e transacional) |
| **Pinpoint** | Campanhas por **e-mail, SMS e push** |

- A AWS **anunciou o fim** do Elastic Transcoder e do Pinpoint. Datas e substitutos: **verifique** na documentação antes da prova.
- **Sumerian** (realidade virtual) foi **descontinuado**.

## H Mapa rápido
```tree
Dados e arquivos
  Mover | Snow Family, DataSync, Transfer Family
  Híbrido | Storage Gateway
  Arquivos | EFS, FSx
  Proteger | AWS Backup, Elastic Disaster Recovery
Apps
  Desktop | WorkSpaces, AppStream 2.0
  Web e API | Amplify, API Gateway, AppSync
  Teste e fluxo | Device Farm, Step Functions
Dispositivos e mídia
  IoT | IoT Core
  Vídeo | MediaConvert
  Mensagens | SES, Pinpoint
```

```callout warn Pegadinhas de prova
- "Petabytes sem rede" = **Snowball Edge**. "Exabytes" = **Snowmobile**.
- "Transferência online agendada" = **DataSync**. "FTP" = **Transfer Family**.
- "Armazenamento híbrido" = **Storage Gateway**.
- "Arquivo compartilhado Linux" = **EFS**. "Windows e AD" = **FSx for Windows**. "HPC" = **FSx for Lustre**.
- "Backup centralizado" = **AWS Backup**. "Recuperar servidores rápido" = **Elastic Disaster Recovery**.
- "Desktop na nuvem" = **WorkSpaces**. "Um app no navegador" = **AppStream 2.0**.
- "Criar uma API" = **API Gateway**. "GraphQL" = **AppSync**. "Dispositivos IoT" = **IoT Core**.
- "Testar em celulares reais" = **Device Farm**.
```
