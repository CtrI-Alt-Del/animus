# Infraestrutura — Animus

## Visão Geral

A infraestrutura do Animus é provisionada via **IaC com Pulumi** e hospedada majoritariamente no **Google Cloud Platform (GCP)**. Ela é composta por um cliente mobile, uma API central em Cloud Run, serviços de armazenamento, banco de dados relacional e vetorial, jobs assíncronos e um serviço de modelo de IA.

[Diagrama da Infraestrutura](documentation/media/animus-infrastructure.png)

---

## Componentes

### 📱 Mobile Client
O aplicativo Flutter rodando no dispositivo do usuário. É o ponto de entrada de todas as interações: faz upload de petições para o File Storage e se comunica com a API Server via HTTP.

---

### ☁️ IaC — Pulumi
Toda a infraestrutura é declarada e provisionada como código usando **Pulumi**. Garante rastreabilidade, reproducibilidade e automação do ambiente de staging e produção no GCP.

---

### 🖥️ Cloud Run — API Server
Núcleo da aplicação. Serviço serverless que executa o backend FastAPI em container. Responsável por:
- Autenticar usuários e gerenciar sessões
- Orquestrar o fluxo de análise de petições
- Consultar o banco relacional e o banco vetorial
- Disparar jobs assíncronos no Inngest
- Expor todos os endpoints consumidos pelo Mobile Client

---

### 🗂️ File Storage — Google Cloud Storage
Armazena os arquivos de petição (PDF/DOCX) enviados pelo usuário. O Mobile Client faz upload direto via **Signed URL** gerada pela API Server, evitando trafegar o arquivo pelo servidor.

---

### 🗃️ Artifact Registry
Repositório de imagens Docker no GCP. Armazena as imagens de container geradas pelo pipeline de CD, que são deployadas no Cloud Run.

---

### 🐘 Postgres — Google Cloud SQL
Banco de dados relacional gerenciado. Armazena todos os dados estruturados da aplicação: usuários, análises, precedentes selecionados e relatórios gerados.

---

### 🔍 Vector Database — Qdrant
Banco de dados vetorial responsável por armazenar os embeddings dos precedentes judiciais do **Pangea**. Utilizado na etapa de busca semântica (RAG) para recuperar os precedentes mais similares ao texto da petição enviada.

---

### ⚙️ Background Jobs — Inngest
Plataforma de orquestração de jobs assíncronos. Disparado pela API Server para executar tarefas de longa duração fora do ciclo de request/response, como:
- Vetorização e indexação dos dados do Pangea
- Processamento da análise da petição pela IA

---

### 🤖 MLflow — Model Service e Registry
Serviço responsável pelo gerenciamento e serving dos modelos de IA utilizados na análise de precedentes. Atua como:
- **Model Registry**: versionamento e rastreamento dos modelos treinados
- **Model Service**: exposição do modelo para inferência, consumido pela API Server

---

### 🔔 Firebase Cloud Messaging — One Signal
Serviço de notificações push. Acionado pela API Server para enviar notificações assíncronas ao Mobile Client — por exemplo, ao concluir o processamento de uma análise em background.

---

## Fluxo Principal

```
Mobile Client
  │
  ├──► File Storage (upload da petição via Signed URL)
  │
  └──► Cloud Run (API Server)
         │
         ├──► Postgres (leitura/escrita de dados)
         ├──► Qdrant (busca vetorial de precedentes)
         ├──► Inngest (dispara job de análise assíncrona)
         │       └──► MLflow (inferência do modelo de IA)
         └──► Firebase / One Signal (notifica o usuário ao concluir)
```

---

## Resumo dos Serviços

| Componente | Tecnologia | Responsabilidade |
|---|---|---|
| IaC | Pulumi | Provisionamento da infraestrutura como código |
| API Server | GCP Cloud Run | Backend principal (FastAPI em container) |
| File Storage | Google Cloud Storage | Armazenamento de petições PDF/DOCX |
| Artifact Registry | GCP Artifact Registry | Repositório de imagens Docker |
| Banco Relacional | Google Cloud SQL (Postgres) | Persistência dos dados da aplicação |
| Banco Vetorial | Qdrant | Busca semântica de precedentes (RAG) |
| Jobs Assíncronos | Inngest | Processamento de tarefas longas |
| Modelo de IA | MLflow | Serving e registry dos modelos |
| Notificações | Firebase / One Signal | Push notifications para o mobile |