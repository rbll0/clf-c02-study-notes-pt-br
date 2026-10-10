---
title: Infraestrutura global da AWS
subtitle: Seção 12 do CLF-C02 — Regiões, AZs, edge, Route 53, CloudFront e híbrido
cols: 3
exam: Domínio 3 · Tecnologia e serviços (34%)
---
A AWS espalha datacenters pelo mundo. Isso dá **baixa latência**, **alta disponibilidade** e **conformidade**. A prova cobra a hierarquia **Região → AZ → edge** e os serviços que usam cada nível.

## A Hierarquia global {span=2}
```tree
Região (grupo de datacenters em uma área geográfica)
  Zona de Disponibilidade (AZ) | 1 ou mais datacenters isolados
    `3 a 6` AZs por região | normalmente no mínimo 3
  Edge location | ponto de presença perto do usuário
  Local Zone | extensão de uma região
  Wavelength Zone | borda dentro da rede 5G
```
- Exemplos de regiões: `us-east-1` (Norte da Virgínia) e `sa-east-1` (São Paulo).
- Cada região é **independente** das outras.
- As AZs de uma região ficam **isoladas** entre si, mas ligadas por **rede de baixa latência**.
- Cada AZ tem **energia, rede e refrigeração** próprias.

## B Como escolher uma região {span=1}
| Critério | Por quê |
|---|---|
| **Conformidade** | A lei pode exigir que os dados fiquem em um país |
| **Latência** | Escolha a região **perto dos usuários** |
| **Serviços disponíveis** | Nem toda região tem todos os serviços |
| **Preço** | O custo muda de região para região |

## C Zonas de Disponibilidade
- Uma **AZ** tem um ou mais datacenters **separados** fisicamente.
- Use **várias AZs** para **alta disponibilidade** e recuperação de desastre.
- Uma falha em uma AZ **não derruba** as outras.
- O **ELB**, o **ASG** e o **RDS Multi-AZ** usam essa separação.

## D Edge locations {span=2}
Uma **edge location** fica **perto do usuário final**. Ela entrega e guarda conteúdo em **cache**.

```flow LR
Usuário -> *Edge location: requisição mais próxima
Edge location -> Região AWS: só se faltar conteúdo
Região AWS --> Edge location: devolve e guarda no cache
```
- A AWS tem **centenas** de edge locations em vários países.
- **CloudFront**, **Route 53** e **Global Accelerator** usam essa rede.
- O tráfego entre as edge locations e as regiões usa a **rede global da AWS**.

## E Route 53 {span=3}
O **Route 53** é o **DNS gerenciado** da AWS. Ele também **registra domínios** e faz **health checks**. A AWS promete **100% de disponibilidade** do serviço.

| Política de roteamento | Como decide | Caso de uso |
|---|---|---|
| **Simple** | Devolve um ou mais valores | Um único recurso |
| **Weighted** | Divide por **pesos** | Teste A/B e migração gradual |
| **Latency** | Escolhe a **menor latência** | Usuários em várias regiões |
| **Failover** | Usa um **secundário** se o primário falhar | Recuperação de desastre |
| **Geolocation** | Escolhe pela **localização** do usuário | Idioma e conformidade |
| **Geoproximity** | Escolhe pela **distância**, com ajuste de **bias** | Mover tráfego entre regiões |
| **IP-based** | Escolhe pelo **IP** do cliente | Rotas por provedor de internet |
| **Multi-value** | Devolve **vários valores saudáveis** | Distribuição simples com health checks |

## F CloudFront {span=2}
O **CloudFront** é a **CDN** da AWS. Ele guarda conteúdo em **cache** nas edge locations.

| Item | Detalhe |
|---|---|
| Efeito | Menos **latência** e menos carga na origem |
| Origens | **S3**, **ALB**, **EC2** ou um servidor HTTP |
| Proteção | Funciona com **Shield** (DDoS) e **WAF** |
| Conteúdo | Estático (imagens, vídeo, JS e CSS) e também dinâmico |
| Cache | Cada objeto tem um **TTL** |

- O **S3** pode ser a origem. O **CloudFront** entrega o objeto no mundo todo.

## G CloudFront vs S3 CRR {span=1}
| | CloudFront | S3 CRR |
|---|---|---|
| Alcance | **Rede global** de edge | **Regiões** escolhidas |
| Conteúdo | **Cache** com TTL | **Cópia** em tempo quase real |
| Melhor para | Estático em escala global | Dados lidos em poucas regiões |

## H S3 Transfer Acceleration e Global Accelerator {span=3}
| Serviço | Para quê | Como funciona |
|---|---|---|
| **S3 Transfer Acceleration** | **Upload** mais rápido para o S3 | Envia ao edge mais próximo. A rede da AWS leva ao bucket |
| **Global Accelerator** | Melhorar **desempenho e disponibilidade** de apps globais | Dá **2 IPs anycast** fixos. Leva o tráfego pela rede da AWS ao endpoint mais próximo e saudável |

- O **Global Accelerator** **não faz cache**. O **CloudFront** faz.
- Use o **Global Accelerator** para **TCP e UDP**: jogos, IoT e VoIP.
- Use o **Global Accelerator** quando precisar de **IPs estáticos** e **failover rápido**.
- Os dois usam a **rede global da AWS**, e não a internet pública, na maior parte do caminho.

## I Soluções de borda e híbridas {span=3}
| Serviço | O que é | Caso de uso |
|---|---|---|
| **Outposts** | **Racks da AWS** instalados no **seu datacenter** | Nuvem híbrida, baixa latência local, dados que devem ficar on-premises |
| **Local Zones** | Extensão de uma região **perto de uma cidade grande** | Latência de milissegundos para jogos, vídeo e mídia |
| **Wavelength** | Serviços AWS **dentro da rede 5G** das operadoras | Apps móveis de latência ultrabaixa |

- No **Outposts**, a AWS instala e mantém o hardware. Você cuida da **segurança física** e da energia do local.
- A **nuvem híbrida** mistura on-premises e AWS. A conexão pode usar **VPN** ou **Direct Connect**.

## J Modelos de arquitetura {span=3}
| Necessidade | Solução |
|---|---|
| Sobreviver à queda de um datacenter | **Várias AZs** |
| Sobreviver à queda de uma região | **Várias regiões** |
| Entregar conteúdo estático ao mundo | **CloudFront** |
| Rotear o DNS pelo usuário mais próximo | **Route 53** (latency ou geolocation) |
| Rodar AWS dentro do seu datacenter | **Outposts** |
| Latência mínima em uma cidade grande | **Local Zones** |

```callout warn Pegadinhas de prova
- Hierarquia: **Região** > **AZ** > **edge location**.
- "**Conformidade e soberania**" = escolha a região pela **lei**. O preço vem depois.
- "DNS gerenciado" = **Route 53**. "Failover" = política **Failover**. "Menor latência" = **Latency**.
- "CDN e cache" = **CloudFront**. "**IP estático** e sem cache, TCP ou UDP" = **Global Accelerator**.
- "Upload mais rápido ao S3" = **Transfer Acceleration**.
- "Hardware da AWS no seu datacenter" = **Outposts**.
- "Rede **5G**" = **Wavelength**. "Perto de uma cidade grande" = **Local Zones**.
- Uma **AZ** pode ter **mais de um** datacenter.
```
