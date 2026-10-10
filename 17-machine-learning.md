---
title: Machine Learning
subtitle: Seção 17 do CLF-C02 — serviços de IA, SageMaker e IA generativa
cols: 3
exam: Domínio 3 · Tecnologia e serviços (34%)
---
A AWS oferece IA em **três níveis**. Os serviços **prontos** têm uma API. O **SageMaker** constrói o seu modelo. A **IA generativa** usa Bedrock e Amazon Q. A prova pede só **o gatilho**. Saiba qual serviço resolve qual problema.

## A Três níveis de IA {span=3}
| Nível | Serviços | Quando usar | Esforço |
|---|---|---|---|
| **IA pronta** (APIs) | Rekognition, Transcribe, Polly, Translate, Comprehend, Textract, Lex e outros | O problema é comum e você não quer treinar modelo | Baixo |
| **ML personalizado** | **SageMaker** | Você precisa de um modelo próprio | Alto |
| **IA generativa** | **Bedrock**, **Amazon Q** | Gerar texto, imagem e código, ou conversar com dados | Baixo a médio |

## B Imagem, vídeo e documento {span=3}
| Serviço | O que faz | Gatilho de prova |
|---|---|---|
| **Rekognition** | Analisa **imagens e vídeos**: objetos, pessoas, texto, rostos, celebridades e moderação de conteúdo | "Reconhecimento facial", "detectar objetos", "moderar conteúdo" |
| **Textract** | Extrai **texto, escrita à mão e dados** de documentos escaneados, formulários e tabelas | "Extrair texto de PDF ou imagem", "formulário", "nota fiscal" |

## C Voz e idioma {span=3}
| Serviço | O que faz | Gatilho de prova |
|---|---|---|
| **Transcribe** | **Voz em texto** (reconhecimento automático de fala). Pode ocultar dados sensíveis | "Legenda", "transcrição de áudio" |
| **Polly** | **Texto em voz** | "Falar o texto", "voz natural" |
| **Translate** | **Traduz** textos com idiomas naturais | "Localização", "traduzir" |
| **Comprehend** | **Processamento de linguagem natural**: sentimento, entidades, frases-chave e idioma | "Sentimento", "analisar texto", "NLP" |

## D Conversa e busca {span=3}
| Serviço | O que faz | Gatilho de prova |
|---|---|---|
| **Lex** | Cria **chatbots** de voz e texto. Usa a mesma tecnologia da **Alexa** | "Chatbot", "assistente virtual" |
| **Connect** | **Central de atendimento** virtual na nuvem | "Call center", "contact center" |
| **Kendra** | **Busca inteligente** em documentos. Responde perguntas em linguagem natural | "Busca em documentos corporativos" |

## E Previsão, recomendação e anomalia {span=3}
| Serviço | O que faz | Gatilho de prova |
|---|---|---|
| **Forecast** | Prevê **séries temporais**: demanda, estoque, receita | "Previsão de demanda" |
| **Personalize** | **Recomendações** personalizadas em tempo real | "Recomendar produtos", "personalização" |
| **Fraud Detector** | Detecta **fraude** online com ML | "Fraude", "contas falsas" |
| **Lookout** | Detecta **anomalias** em métricas, equipamentos e imagens industriais | "Defeito em peça", "anomalia" |

## F SageMaker {span=2}
```flow LR
Dados no S3 -> *SageMaker: prepara e treina
SageMaker -> Modelo: avalia
Modelo -> Endpoint: implanta
Endpoint -> Aplicação: devolve previsões
```
O **SageMaker** é a plataforma para **construir, treinar e implantar** modelos de ML. Ele atende **cientistas de dados** e **desenvolvedores**.

## G Recursos do SageMaker {span=1}
- **Ground Truth**: rotula dados.
- **Canvas**: ML **sem código**.
- **Clarify**: detecta **viés** nos dados e no modelo.
- **JumpStart**: modelos **pré-treinados**.

## H Bedrock {span=2}
- **Bedrock** dá acesso a **modelos de fundação** (FMs) por **API**, sem gerenciar servidores.
- Os modelos vêm de várias empresas, como **Amazon**, **Anthropic**, **Meta** e **Mistral**.
- Você **personaliza** os modelos com seus dados, sem treinar do zero.
- **Knowledge Bases** liga o modelo aos **seus documentos** (RAG).
- **Agents** executam tarefas em várias etapas.
- **Guardrails** filtram conteúdo indesejado.
- Gatilho: "IA generativa", "modelo de fundação", "chamar um LLM por API".

## I Amazon Q {span=1}
- **Amazon Q Business**: assistente de IA que responde sobre os **dados da empresa**.
- **Amazon Q Developer**: assistente para **código**. Sugere, explica e depura. Incorporou o **CodeWhisperer**.
- Gatilho: "assistente de código" ou "assistente corporativo".

```callout warn Pegadinhas de prova
- "Imagem ou vídeo" = **Rekognition**. "Documento escaneado" = **Textract**.
- "Voz em texto" = **Transcribe**. "Texto em voz" = **Polly**. "Traduzir" = **Translate**.
- "Sentimento e NLP" = **Comprehend**. "Chatbot" = **Lex**. "Call center" = **Connect**.
- "Busca inteligente em documentos" = **Kendra**. "Recomendações" = **Personalize**.
- "Previsão de séries temporais" = **Forecast**. "Fraude" = **Fraud Detector**.
- "Construir, treinar e implantar modelo próprio" = **SageMaker**.
- "Modelos de fundação por API" = **Bedrock**. "Assistente de código" = **Amazon Q Developer** (antes, CodeWhisperer).
```
