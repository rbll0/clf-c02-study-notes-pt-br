---
title: Databases e Analytics
subtitle: Seção 9 do CLF-C02 — RDS, Aurora, DynamoDB, Redshift, Athena e mais
cols: 3
exam: Domínio 3 · Tecnologia e serviços (34%)
---
A AWS oferece um banco **gerenciado** para cada tipo de dado. A prova cobra a **combinação certa**: dado de cada tipo, serviço certo. Memorize o **caso de uso** de cada um.

## A Qual banco usar {span=3}
| Tipo de dado ou necessidade | Serviço | Modelo |
|---|---|---|
| Tabelas relacionais com SQL | **RDS**, **Aurora** | Relacional (OLTP) |
| Chave-valor com latência de milissegundos, escala enorme | **DynamoDB** | NoSQL, serverless |
| Cache em memória para reduzir a carga do banco | **ElastiCache** | Redis, Memcached |
| Documentos JSON compatíveis com MongoDB | **DocumentDB** | Documentos |
| Relações entre dados (redes sociais, fraude) | **Neptune** | Grafo |
| Séries temporais (IoT, métricas) | **Timestream** | Séries temporais |
| Análise de grandes volumes com SQL | **Redshift** | Data warehouse (OLAP) |

## B RDS e Aurora {span=2}
O **RDS** (Relational Database Service) roda bancos SQL **gerenciados**.

| Item | Detalhe |
|---|---|
| Engines | PostgreSQL, MySQL, MariaDB, Oracle, SQL Server, IBM Db2, Aurora |
| A AWS cuida de | Provisionamento, SO, correções, **backups automáticos**, restauração em um ponto no tempo |
| Escala | Vertical e horizontal (**read replicas**) e **storage auto scaling** |
| Alta disponibilidade | **Multi-AZ**: réplica em outra AZ com failover automático |
| Limite | **Sem acesso SSH** à instância |

- **Read replicas** (até 15 em MySQL, MariaDB e PostgreSQL) aliviam a **leitura**. **Multi-AZ** protege contra **falha**.
- Compare com um banco **dentro do EC2**: nele, você cuida de tudo.

## C Aurora
- Banco da AWS **compatível** com **MySQL** e **PostgreSQL**.
- Mais rápido que o MySQL e o PostgreSQL comuns no RDS.
- Guarda **6 cópias** dos dados em **3 AZs**. O armazenamento cresce sozinho.
- Custa cerca de **20% mais** que o RDS, mas rende mais.
- **Aurora Serverless**: ajusta a capacidade sozinho. Bom para carga imprevisível.
- **Aurora Global Database**: replica entre regiões, com latência baixa.

## D DynamoDB e cache {span=2}
```flow LR
Aplicação -> *DynamoDB: leitura e escrita em milissegundos
Aplicação -> DAX: leitura em microssegundos
DAX -> DynamoDB: cache miss
Aplicação -> ElastiCache: cache na frente de RDS
ElastiCache --> RDS: cache miss
```
- **DynamoDB**: NoSQL **serverless**, chave-valor, sem servidores para gerenciar.
- Escala para **milhões de requisições por segundo**. Itens de até **400 KB**.
- Modos: **sob demanda** ou **provisionado**. **Global Tables** replicam entre regiões.
- **DAX** é o cache em memória **exclusivo do DynamoDB**.

## E ElastiCache
- Cache **em memória** gerenciado com **Redis** ou **Memcached**.
- Reduz a carga do banco e baixa a latência de leitura.
- Serve para **sessões** de usuário e consultas repetidas.
- O app precisa **usar** o cache. Ele não é automático.

## F Outros bancos {span=3}
| Serviço | Para quê | Gatilho de prova |
|---|---|---|
| **DocumentDB** | Documentos JSON, compatível com **MongoDB** | "MongoDB" |
| **Neptune** | Banco de **grafo**, bilhões de relações | "Grafo", "redes sociais", "detecção de fraude" |
| **Timestream** | Séries temporais **serverless** | "IoT", "séries temporais" |
| **QLDB** | Livro-razão **imutável** | "Ledger". warn Serviço **encerrado** em jul/2025 |
| **Managed Blockchain** | Redes de blockchain | "Blockchain" |

## G DMS — migrar bancos {span=2}
```flow LR
Banco de origem -> *DMS: lê as mudanças
DMS -> Banco de destino: grava
```
- O **Database Migration Service** migra bancos **com segurança e rapidez**.
- O banco de origem **continua disponível** durante a migração.
- Migração **homogênea**: Oracle para Oracle.
- Migração **heterogênea**: SQL Server para Aurora. Use o **Schema Conversion Tool (SCT)** para converter o esquema.
- Ele também replica **continuamente** (CDC).

## H Analytics — visão geral {span=3}
| Serviço | Função | Característica |
|---|---|---|
| **Redshift** | **Data warehouse** | SQL em **petabytes**, armazenamento em **colunas**, OLAP. Existe versão serverless |
| **Athena** | Consultas SQL **direto no S3** | **Serverless**, sem carregar dados. Paga por **dado varrido** |
| **EMR** | Clusters **Hadoop** e Spark | Big data. Roda sobre instâncias EC2 |
| **QuickSight** | **BI** e dashboards | Serverless, com insights de **ML**. Paga por sessão |
| **Glue** | **ETL** serverless | Extrai, transforma e carrega. Tem o **Data Catalog** |
| **OpenSearch** | **Busca** e análise de logs | Sucessor do Elasticsearch |

- **OLTP** (RDS, Aurora): muitas transações pequenas. **OLAP** (Redshift): poucas consultas pesadas.
- Usar **Parquet** (colunar) no Athena reduz o custo.

## I Fluxo de analytics {span=2}
```flow LR
Dados no S3 -> *Glue: cataloga e transforma
Glue -> Athena: consulta SQL
Athena -> QuickSight: dashboards
Dados no S3 --> Redshift: carga em lote
Redshift --> QuickSight: dashboards
```
- O **S3** vira o **data lake** central.
- **Athena** serve para consulta rápida e eventual. **Redshift** serve para análise pesada e recorrente.

## J RDS vs Aurora vs DynamoDB {span=1}
| | RDS | Aurora | DynamoDB |
|---|---|---|---|
| Modelo | SQL | SQL | NoSQL |
| Servidor | Instância | Instância ou serverless | **Serverless** |
| Escala | Réplicas | Réplicas + storage | Automática |

```callout warn Pegadinhas de prova
- "SQL gerenciado" = **RDS**. "Mais rápido e compatível com MySQL ou PostgreSQL" = **Aurora**.
- "NoSQL serverless, milissegundos" = **DynamoDB**. "Cache do DynamoDB" = **DAX**.
- "Cache em memória" = **ElastiCache**. "MongoDB" = **DocumentDB**. "Grafo" = **Neptune**.
- "**Multi-AZ**" = recuperação de falha. "**Read replica**" = escala de leitura.
- "Migrar banco com origem ativa" = **DMS**. "Mudar de engine" = DMS + **SCT**.
- "Data warehouse e petabytes" = **Redshift**. "SQL direto no S3, serverless" = **Athena**.
- "Dashboards e BI" = **QuickSight**. "ETL" = **Glue**. "Hadoop e Spark" = **EMR**.
- "Busca em logs" = **OpenSearch**.
```
