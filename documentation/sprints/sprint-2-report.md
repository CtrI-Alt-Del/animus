# Sprint - 2️⃣

> **Período:** 07/04/2026 – 27/04/2026

---

## Requisitos realizados nessa sprint ✨

### Armazenamento e Organização de Análises:
- Domínio de biblioteca (`core/library`) extraído e desacoplado de `intake`
- Endpoints completos do domínio de biblioteca: criar, consultar, renomear, arquivar e listar pastas
- Endpoint de movimentação em lote de análises entre pastas (incluindo retorno para "Sem pasta")
- Operação de arquivamento em lote de análises com semântica atômica
- Tela principal de Biblioteca no mobile (substituindo o placeholder)
- Tela individual de Pasta com listagem, seleção múltipla, mover e arquivar em lote
- Modal de criação de pasta a partir da Biblioteca
- Domínio mobile de biblioteca (`core/library`) com `LibraryService` e DTOs próprios

### Síntese Explicativa e Aplicabilidade de Precedentes:
- Classificação de aplicabilidade de precedentes na pipeline de busca (synthesizer com `applicability_level`, `legal_features` e Consistency Layer)
- Legal Rank Score e reranking por relevância jurídica (preservando `similarity_rank` original)
- Persistência de filtros aplicados (tribunais, tipos de precedente e quantidade) na análise
- Endpoint de análises em processamento (`GET /intake/analyses/processing`) para polling pelo mobile
- Novo status `ANALYZING_PRECEDENTS_SIMILARITY` adicionado ao fluxo assíncrono
- Card de processamento na tela de análise com mensagens dinâmicas por etapa

### Exportação de Relatório:
- Exibição dos filtros aplicados (quantidade, tribunais, tipos) no PDF da análise
- Exibição do nível de aplicabilidade de cada precedente no PDF e na tela de análise

### Gerenciamento de Sessão do Usuário:
- Migração completa do reset de senha para fluxo OTP (substituindo link/token legado)
- Endpoints `forgot`, `resend-reset-otp`, `verify-reset-otp` e `reset` refatorados com contexto temporário seguro
- Tela mobile de redefinição de senha refatorada para o novo fluxo OTP

### Notificações:
- Domínio `notification` no backend estruturado (Ports and Adapters) com `PushNotificationProvider` e `PushNotificationMessage`
- Adapter concreto via OneSignal e jobs de notificação no Inngest
- Eventos de domínio `PetitionSummaryFinishedEvent` e `PrecedentsSearchFinishedEvent` publicados no final dos jobs de intake
- Setup do OneSignal no app Flutter (driver, providers, identificação por conta autenticada)

### Infraestrutura, ML e DevOps:
- Setup do projeto `animus-ml`
- Pipeline de CD do `animus-ml`
- Infraestrutura GCP para Vertex AI provisionada (IaC)

---

## User Stories realizadas nessa sprint 📖

| # | Chave | User Story | Status |
|---|-------|------------|--------|
| US04 | ANI-5 | Como Juiz, quero **rever análises passadas feitas no aplicativo**, para revisar informações importantes sobre processos que eu já julguei | ✅ Concluído |
| US05 | ANI-6 | Como Juiz, quero **organizar as análises do aplicativo em pastas**, para que eu possa buscar as análises mais relevantes de acordo com um tema específico que eu desejo rever | ✅ Concluído |
| US06 | ANI-7 | Como Juiz, quero ter acesso às **análises mais recentes** que eu fiz no aplicativo, para que eu possa rever rapidamente informações relacionadas a esses casos que eu julguei | ✅ Concluído |
| US07 | ANI-2 | Como Juiz, quero **receber um resumo sobre a petição** que irei julgar, para entender melhor o tema e verificar informações importantes sobre o caso (refatoração com aplicabilidade) | ✅ Concluído |
| US08 | ANI-10 | Como Juiz, quero receber um **PDF contendo todo o relatório de análise** e síntese de precedentes, para documentar a escolha do precedente que utilizarei no caso (evolução com filtros e aplicabilidade) | ✅ Concluído |

---

## Critérios de aceitação para cada User Story 📒

### US04 — Rever análises passadas

- **Dado** que o usuário possui análises salvas, **quando** ele acessar a listagem de análises, **então** deve ver cada análise com nome, data de criação e pasta associada (quando houver), ordenadas da mais recente para a mais antiga.
- **Dado** que o usuário toca em uma análise salva, **quando** a tela de detalhes for aberta, **então** deve ser possível rever o conteúdo completo: resumo da petição, lista de precedentes com classificações e sínteses, e o precedente escolhido.
- **Dado** que o usuário não possui nenhuma análise salva, **quando** ele acessar a listagem, **então** um estado vazio com mensagem amigável deve ser exibido.
- **Dado** que uma análise foi salva com nome gerado automaticamente, **quando** o usuário acessar a tela de detalhes, **então** deve ser possível editar o nome da análise.

### US05 — Organização de análises em pastas

- **Dado** que o usuário está na tela de gerenciamento de pastas, **quando** ele criar uma nova pasta com nome válido, **então** a pasta deve ser criada e exibida na listagem com confirmação visual.
- **Dado** que o usuário deseja renomear uma pasta, **quando** alterar o nome e confirmar, **então** o novo nome deve ser salvo e refletido imediatamente na interface.
- **Dado** que o usuário exclui uma pasta, **quando** confirmar a exclusão, **então** a pasta deve ser removida e todas as análises contidas nela devem ser movidas automaticamente para "Sem pasta", sem perda de análises.
- **Dado** que o usuário deseja mover uma análise para uma pasta, **quando** selecionar a pasta no seletor, **então** a análise deve passar a estar associada à pasta escolhida (somente uma pasta por vez).
- **Dado** que o usuário tenta criar ou renomear uma pasta com nome em branco ou acima de 50 caracteres, **quando** o sistema validar o campo, **então** o salvamento deve ser bloqueado com mensagem de erro inline.

### US06 — Análises mais recentes na Home

- **Dado** que o usuário possui análises salvas, **quando** a Home for carregada, **então** as últimas 5 entradas devem ser exibidas ordenadas da mais recente para a mais antiga, com nome, data e pasta associada.
- **Dado** que a análise mais recente ainda está em processamento, **quando** a Home for carregada, **então** ela deve aparecer no topo da lista com indicador visual de progresso e não ser navegável.
- **Dado** que o usuário toca em um card de análise concluída, **quando** a navegação ocorrer, **então** o sistema deve direcionar para a tela de detalhes daquela análise.
- **Dado** que o usuário não possui nenhuma análise, **quando** a Home for carregada, **então** a seção de recentes deve exibir estado vazio com atalho para iniciar uma nova análise.

### US07 — Resumo da petição (evolução)

- **Dado** que o usuário enviou uma petição válida e acionou "Analisar", **quando** a IA concluir o processamento, **então** um resumo das principais informações deve ser exibido antes da listagem de precedentes.
- **Dado** que a busca de precedentes está em andamento, **quando** o usuário visualizar a tela de análise, **então** o card de processamento deve refletir a etapa corrente (analisando petição, buscando precedentes, analisando similaridade ou aplicabilidade).
- **Dado** que a análise é concluída, **quando** o usuário visualizar os precedentes, **então** cada item deve exibir o nível de aplicabilidade (*Aplicável*, *Possivelmente aplicável* ou *Não aplicável*) classificado pela pipeline de busca com Consistency Layer.

### US08 — Relatório PDF (evolução)

- **Dado** que a análise foi concluída com filtros aplicados na busca de precedentes, **quando** o usuário exportar o PDF, **então** o relatório deve incluir uma seção com a quantidade de precedentes, tribunais e tipos selecionados.
- **Dado** que a busca foi executada sem filtros opcionais, **quando** o PDF for gerado, **então** o relatório deve comunicar isso de forma clara (ex: "Sem filtros de tribunal").
- **Dado** que o relatório é exportado, **quando** o usuário visualizar a lista de precedentes, **então** cada item deve apresentar o nível de aplicabilidade calculado pela pipeline.
- **Dado** que a análise ainda está em andamento, **quando** o usuário acessar a tela de detalhes, **então** a opção de exportação não deve estar disponível.

---

## Tasks realizadas nessa sprint 🛠️

### `[server]`

| Chave | Task | Responsável | Status |
|-------|------|-------------|--------|
| ANI-63 | Adição de classificação de aplicabilidade de precedentes na pipeline de busca | João P. | ✅ Concluído |
| ANI-64 | Jobs para envio de notificação de análise de petição pronta e busca de precedentes finalizada | Kauan | ✅ Concluído |
| ANI-65 | Endpoints do domínio de biblioteca (criar, consultar, renomear, arquivar, listar pastas) | Thigz | ✅ Concluído |
| ANI-66 | Endpoint de movimentação de análises entre pastas (lote) e arquivamento em lote | Kauan | ✅ Concluído |
| ANI-70 | Refatoração de endpoint de redefinição de senha com verificação OTP | Thigz | ✅ Concluído |
| ANI-77 | Registro de filtros aplicados à análise (persistência) | Gustavo | ✅ Concluído |
| ANI-79 | Endpoint que retorna análises em processamento (`GET /intake/analyses/processing`) | Kauan | ✅ Concluído |
| ANI-83 | Refatorar domínio de pastas para `core/library` | João P. | ✅ Concluído |
| ANI-84 | Domínio de notification (Ports and Adapters + adapter OneSignal) | João P. | ✅ Concluído |
| ANI-76 | Corrigir operações em lote para arquivar e mover análises | João P. | ✅ Concluído |

### `[mobile]`

| Chave | Task | Responsável | Status |
|-------|------|-------------|--------|
| ANI-67 | Adição de nível de aplicabilidade de precedente na tela de análise e no relatório em PDF | João P. | ✅ Concluído |
| ANI-71 | Refatoração da tela de redefinição de senha com verificação OTP | Thigz | ✅ Concluído |
| ANI-72 | Tela de Biblioteca (listagem de pastas, "Sem pasta" e modal de criação) | Thigz | ✅ Concluído |
| ANI-73 | Tela de Pasta (listagem, seleção múltipla, mover e arquivar em lote) | Gustavo | ✅ Concluído |
| ANI-74 | UI de análises em processamento (card com polling e mensagens por etapa) | Thigz | ✅ Concluído |
| ANI-75 | Domínio mobile de biblioteca (DTOs e `LibraryService`) | João P. | ✅ Concluído |
| ANI-81 | Adição de filtros aplicados na análise na geração de PDF | Gustavo | ✅ Concluído |
| ANI-80 | Setup do OneSignal no app | Gustavo | ✅ Concluído |
| ANI-82 | Correção do design da tela de Biblioteca | João P. | ✅ Concluído |

### `[ml]`

| Chave | Task | Responsável | Status |
|-------|------|-------------|--------|
| ANI-78 | Setup do projeto `animus-ml` | João P. | ✅ Concluído |
| ANI-69 | Pipeline de CD do `animus-ml` | Gabriel | ✅ Concluído |

### `[iac]`

| Chave | Task | Responsável | Status |
|-------|------|-------------|--------|
| ANI-68 | Infraestrutura para Vertex AI | Gabriel | ✅ Concluído |

### `[bugs]`

| Chave | Task | Responsável | Status |
|-------|------|-------------|--------|
| ANI-85 | Arrumar o timeout do CD do server | Gabriel | ✅ Concluído |
| ANI-86 | Arrumar CD do server | Gabriel | ✅ Concluído |

---

## Métricas da Sprint 📈

| Métrica | Valor |
|---------|-------|
| Total de tickets na sprint | 29 |
| Tickets concluídos | **29** |
| Tickets em andamento | 0 |
| Tasks técnicas concluídas | 25 |
| User Stories concluídas | 5 (ANI-2, ANI-5, ANI-6, ANI-7, ANI-10) |
| Bugs corrigidos | 2 |

### Distribuição por área

| Área | Concluídos | Em andamento |
|------|------------|--------------|
| `server` | 10 | 0 |
| `mobile` | 9 | 0 |
| `ml` | 2 | 0 |
| `iac` | 1 | 0 |
| `bugs` | 2 | 0 |
| `histórias (US)` | 5 | 0 |

---

## Destaques técnicos da sprint 🌟

- **Separação de bounded contexts**: o domínio de pastas foi extraído de `intake` para um novo contexto `core/library`, tanto no backend quanto no mobile, reduzindo acoplamento e preparando a evolução da feature de organização de análises.
- **Pipeline de classificação jurídica**: o `AnalysisPrecedentsSynthesizerAgent` (gpt-5.4-mini) passou a emitir, em chamada única, a síntese, as 5 features jurídicas e o `applicability_level`, alimentando uma Consistency Layer determinística (regras R1–R5) e o Legal Rank Score que rerankeia precedentes por relevância jurídica.
- **Operações em lote**: novos endpoints atômicos para mover e arquivar múltiplas análises em uma única requisição, com validação de ownership e tratamento idempotente para `Sem pasta`.
- **Migração para OTP no reset de senha**: eliminação completa do fluxo legado por link/token, com novo modelo baseado em OTP de 6 dígitos, contexto temporário seguro pós-verificação e proteção contra enumeração de contas.
- **Fundação de push notifications**: o domínio `notification` foi consolidado no backend com Ports and Adapters (OneSignal como adapter concreto) e o app Flutter recebeu o setup do OneSignal SDK, deixando a base pronta para notificar conclusão de análises e buscas de precedentes.