# Sprint - 1️⃣

> **Período:** 17/03/2026 – 06/04/2026  

---

## Requisitos realizados nessa sprint ✨

### Gerenciamento de Sessão do Usuário:
- Cadastro e login via e-mail e senha
- Login e cadastro com conta Google (OAuth)
- Redefinição de senha por e-mail
- Página de perfil para edição de dados da conta

### Análise de Texto de Petições Iniciais:
- Upload de petição em formato PDF ou DOCX (até 20MB)
- Processamento da petição pela IA com retorno de resumo estruturado

### Busca de Precedentes Judiciais:
- Vetorização dos dados do Pangea (base nacional de precedentes) via RAG
- Busca semântica de precedentes aplicáveis com base no conteúdo da petição
- Cálculo e categorização de aplicabilidade: *Aplicável*, *Possivelmente aplicável*, *Não aplicável*

### Armazenamento de Análises:
- CRUD completo de análises associadas à conta do usuário

### Home Interativa:
- Tela inicial com boas-vindas, hora atual e acesso rápido às análises recentes

### Exportação de Relatório PDF:
- Endpoint server para fornecimento dos dados necessários à geração do PDF *(UI mobile em andamento)*

### Infraestrutura e DevOps:
- Repositórios criados e configurados: server, mobile e IaC
- Pipelines CI completos (typecheck, lint, testes e build) para server, mobile e IaC
- Documentação da API via Swagger
- Versionamento automático configurado no server

### Design:
- Design completo no Figma das telas referentes aos PRDs 1, 2 e 3

---

## User Stories realizadas nessa sprint 📖

| # | Chave | User Story | SP | Status |
|---|-------|------------|----|--------|
| US01 | ANI-1 | Como Juiz, quero **analisar uma petição judicial**, para escolher qual precedente se aplica melhor ao caso que irei julgar | 21 | ✅ Concluído |
| US02 | ANI-3 | Como Juiz, quero ter acesso a uma **lista de precedentes judiciais aplicáveis** à petição enviada, para embasar minha decisão | 21 | ✅ Concluído |
| US03 | ANI-4 | Como Juiz, quero ter acesso a um **relatório sobre a análise da petição e dos precedentes relacionados**, para apoiar minha escolha de forma fundamentada | 21 | ✅ Concluído |

---

## Critérios de aceitação para cada User Story 📒

### US01 — Análise de Petição Judicial

- **Dado** que o usuário está na tela de nova análise, **quando** ele selecionar um arquivo PDF ou DOCX válido (até 20MB), **então** o nome e o tamanho do arquivo devem ser exibidos na tela antes de iniciar a análise.
- **Dado** que o usuário selecionou um arquivo válido, **quando** ele tocar no botão "Analisar", **então** o sistema deve exibir indicador de carregamento, desabilitar o botão para evitar acionamentos duplicados, processar o documento pela IA e exibir o resumo da petição ao final.
- **Dado** que o usuário tenta enviar um arquivo em formato não suportado ou acima de 20MB, **quando** o sistema validar o arquivo, **então** o envio deve ser bloqueado com mensagem clara indicando o problema (formato ou tamanho).
- **Dado** que a IA está processando a petição, **quando** o tempo de resposta ultrapassar 60 segundos, **então** o sistema deve cancelar a operação, notificar o usuário e oferecer opção de tentar novamente.
- **Dado** que a análise da IA falha por qualquer outro motivo, **quando** o erro for detectado, **então** o sistema deve exibir uma mensagem de erro amigável com opção de retentar.

### US02 — Lista de Precedentes Aplicáveis

- Os precedentes exibidos devem ter taxa de aplicabilidade categorizada como *Aplicável* (95–85%), *Possivelmente aplicável* (80–70%) ou *Não aplicável* (<60%).
- A lista deve ser ordenada por grau de aplicabilidade, do mais ao menos relevante.
- Cada item deve exibir: título do precedente, tribunal, tipo/espécie e taxa de aplicabilidade.
- Deve ser possível consultar o texto completo do precedente a partir da listagem.
- O usuário deve poder filtrar por tribunal (obrigatório se selecionado) e tipo/espécie (opcional) — *a partir da Sprint 2*.

### US03 — Relatório de Análise dos Precedentes

- O relatório deve conter: resumo da petição, lista de precedentes por aplicabilidade, síntese explicativa entre petição e precedentes aplicáveis, e o precedente escolhido pelo usuário.
- O usuário deve poder escolher o precedente que deseja aplicar ao caso — a IA **não** define automaticamente a escolha final.
- O relatório deve ser exportável em formato PDF com todas as seções listadas acima.
- O nome do relatório deve ser editável para identificação fácil nas análises salvas.
- O conteúdo gerado deve ser coerente com a petição enviada e não afirmar conclusões jurídicas definitivas.

---

## Tasks realizadas nessa sprint 🛠️

### `[server]`

| Chave | Task | Responsável | SP | Status |
|-------|------|-------------|-----|--------|
| ANI-21 | Criar repositório e setup do projeto com testes e Docker Compose de PostgreSQL | João P. | 3 | ✅ Concluído |
| ANI-22 | CI (typecheck, codecheck, testes e build) | Gabriel | 3 | ✅ Concluído |
| ANI-52 | Domínio base dos PRDs 1, 2 e 3 | João P. | 5 | ✅ Concluído |
| ANI-34 | Endpoint de Sign In | João P. | 3 | ✅ Concluído |
| ANI-35 | Endpoints de Sign Up, Verificação e Reenvio de E-mail | João P. | 3 | ✅ Concluído |
| ANI-57 | Refatorar verificação de e-mail: substituir token JWT por OTP de 6 dígitos no Redis | João P. | — | ✅ Concluído |
| ANI-36 | Endpoints de pedido de redefinição de senha | Kauan | 5 | ✅ Concluído |
| ANI-37 | Endpoint de Sign In/Sign Up com Google | Kauan | 5 | ✅ Concluído |
| ANI-38 | Endpoint de atualização de conta de usuário | Gustavo | 3 | ✅ Concluído |
| ANI-53 | Endpoint que retorna dados da conta do usuário | Gustavo | 3 | ✅ Concluído |
| ANI-54 | Endpoint para retornar conta do usuário | Gustavo | 3 | ✅ Concluído |
| ANI-29 | Endpoint para CRUD de análises | Thigz | 1 | ✅ Concluído |
| ANI-44 | Endpoint de geração de Signed URL para upload de petições | Kauan | 3 | ✅ Concluído |
| ANI-45 | Endpoint de análise da petição (processamento IA) | Gustavo | 5 | ✅ Concluído |
| ANI-47 | Job de vetorização dos dados do Pangea (RAG) | Kauan | 8 | ✅ Concluído |
| ANI-48 | Endpoint de busca de precedentes da análise | João P. | 5 | ✅ Concluído |
| ANI-50 | Endpoint que retorna dados para geração do PDF da análise | Gustavo | 3 | ✅ Concluído |
| ANI-55 | Adicionar versionamento automático | Kauan | 3 | ✅ Concluído |
| ANI-56 | Adicionar Swagger (documentação da API) | Kauan | 1 | ✅ Concluído |
| ANI-23 | CD para o GCP em staging | Gabriel | 5 | ✅ Concluído |

### `[mobile]`

| Chave | Task | Responsável | SP | Status |
|-------|------|-------------|-----|--------|
| ANI-25 | Criar repositório e setup do projeto e testes | João P. | 3 | ✅ Concluído |
| ANI-26 | CI (typecheck, codecheck, testes e build) | Gabriel | 3 | ✅ Concluído |
| ANI-33 | Criar design das telas referentes aos PRDs (PRD 1, PRD 2, PRD 3) | Thigz | 8 | ✅ Concluído |
| ANI-39 | Tela de Sign In | Thigz | 3 | ✅ Concluído |
| ANI-40 | Tela de Sign Up — Cadastro via E-mail e Senha | João P. | 3 | ✅ Concluído |
| ANI-41 | UI de Sign Up/Sign In com Google | Thigz | 5 | ✅ Concluído |
| ANI-42 | Tela de redefinição de senha | Thigz | 3 | ✅ Concluído |
| ANI-43 | Tela de perfil | Thigz | 3 | ✅ Concluído |
| ANI-46 | UI de upload e de análise de petição | João P. | 5 | ✅ Concluído |
| ANI-49 | UI de busca de precedentes da análise | João P. | 5 | ✅ Concluído |
| ANI-60 | Tela Home | Thigz | — | ✅ Concluído |
| ANI-51 | UI da geração e compartilhamento de PDF da análise | Gustavo | 3 | ✅ Concluído |
| ANI-27 | CD para gerar APK | Gabriel | 3 | ✅ Concluído |

### `[iac]`

| Chave | Task | Responsável | SP | Status |
|-------|------|-------------|-----|--------|
| ANI-28 | Criar repositório e setup do projeto | João P. | 1 | ✅ Concluído |
| ANI-30 | CI para staging | Gabriel | 3 | ✅ Concluído |
| ANI-32 | Montar infraestrutura GCP staging e conta no Inngest | Gabriel | 8 | ✅ Concluído |
| ANI-31 | CD para staging | Gabriel | 5 | ✅ Concluído |

### `[bugs]`

| Chave | Task | Responsável | SP | Status |
|-------|------|-------------|-----|--------|
| ANI-61 | Corrigir geração de Signed URL para upload de petições e substituição de petição | João P. | — | ✅ Concluído |
| ANI-62 | Melhorar cálculo de aplicabilidade de precedentes | João P. | — | ✅ Concluído |

---

## Gráfico Burndown 📈

[Imagem doo gráfico](documentation/media/burndown-sprint-1.png)

| Métrica | Valor |
|---------|-------|
| Total de Story Points | 134 SP |
| SP Concluídos | **110 SP** |
| SP Restantes | 24 SP |
| Tasks Concluídas | **34 de 42** (81%) |
| Tasks Em Andamento | 5 |
| Tasks Pendentes | 3 |

---
