# Modelagem do Banco de Dados

## Visao Geral

O schema atual do projeto e baseado nos models SQLAlchemy em `src/animus/database/sqlalchemy/models/`.

- Todas as tabelas herdam `created_at` e `updated_at` de `Model`.
- Os identificadores primarios usados hoje sao `string(26)`, nao `uuid`.
- Algumas relacoes de ownership sao mantidas por ID sem `ForeignKey` fisica no banco, como `account_id` e `folder_id` em `analyses`.
- A tabela `analysies_precedent_legal_features` preserva o nome atual do projeto, inclusive a grafia `analysies`.

## Diagrama ER

```mermaid
erDiagram
  accounts {
    string_26 id PK
    string name
    string email UK
    string password_hash NULL
    boolean is_verified
    boolean is_active
    timestamp created_at
    timestamp updated_at
  }

  folders {
    string_26 id PK
    string name
    string_26 account_id
    boolean is_archived
    timestamp created_at
    timestamp updated_at
  }

  analyses {
    string_26 id PK
    string name
    string_26 folder_id NULL
    string_26 account_id
    string status
    boolean is_archived
    json precedents_search_courts NULL
    json precedents_search_precedent_kinds NULL
    integer precedents_search_limit NULL
    timestamp created_at
    timestamp updated_at
  }

  petitions {
    string_26 id PK
    string_26 analysis_id FK
    timestamp uploaded_at
    string document_file_path
    string document_name
    timestamp created_at
    timestamp updated_at
  }

  petition_summaries {
    string_26 petition_id PK, FK
    text case_summary
    text legal_issue
    text central_question
    json relevant_laws
    json key_facts
    json search_terms
    text type_of_action NULL
    json secondary_legal_issues
    json alternative_questions
    text jurisdiction_issue NULL
    text standing_issue NULL
    json requested_relief
    json procedural_issues
    json excluded_or_accessory_topics
    timestamp created_at
    timestamp updated_at
  }

  precedents {
    string_26 id PK
    string_10 court
    string_10 kind
    integer number
    string_120 status
    text enunciation
    text thesis
    timestamp last_updated_in_pangea_at
    timestamp created_at
    timestamp updated_at
  }

  analysis_precedents {
    string_26 analysis_id PK, FK
    string_26 precedent_id PK, FK
    boolean is_chosen
    float similarity_score NULL
    float thesis_similarity_score
    float enunciation_similarity_score
    integer total_search_hits
    integer similarity_rank
    integer final_rank
    integer applicability_level
    text synthesis NULL
    timestamp created_at
    timestamp updated_at
  }

  analysies_precedent_legal_features {
    string_26 analysis_id PK, FK
    string_26 precedent_id PK, FK
    integer central_issue_match
    integer structural_issue_match
    integer context_compatibility
    integer is_lateral_topic
    integer is_accessory_topic
    timestamp created_at
    timestamp updated_at
  }

  analysis_precedent_applicability_feedbacks {
    string_26 analysis_id PK, FK
    string_26 precedent_id PK, FK
    integer applicability_level
    boolean is_from_human
    timestamp created_at
    timestamp updated_at
  }

  analysis_precedent_dataset_rows {
    string_26 analysis_id PK, FK
    string_26 precedent_id PK, FK
    integer applicability_level
    boolean is_from_human
    float thesis_similarity_score
    float enunciation_similarity_score
    integer total_search_hits
    integer similarity_rank
    string_10 identifier_court
    string_10 identifier_kind
    integer identifier_number
    string_120 precedent_status
    timestamp last_updated_in_pangea_at
    integer central_issue_match
    integer structural_issue_match
    integer context_compatibility
    integer is_lateral_topic
    integer is_accessory_topic
    timestamp created_at
    timestamp updated_at
  }

  accounts ||--o{ folders : "owns (logical)"
  accounts ||--o{ analyses : "owns (logical)"
  folders ||--o{ analyses : "groups (logical)"
  analyses ||--o{ petitions : "has"
  petitions ||--|| petition_summaries : "summarized by"
  analyses ||--o{ analysis_precedents : "classifies"
  precedents ||--o{ analysis_precedents : "appears in"
  analysis_precedents ||--|| analysies_precedent_legal_features : "legal features"
  analysis_precedents ||--o{ analysis_precedent_applicability_feedbacks : "feedbacks"
  analysis_precedents ||--o{ analysis_precedent_dataset_rows : "dataset rows"
```

## Descricao Das Tabelas

### `accounts`

Armazena as contas autenticaveis do sistema.

| Campo | Tipo | Descricao |
|---|---|---|
| `id` | string(26) | Identificador primario da conta |
| `name` | string | Nome exibido do usuario |
| `email` | string | E-mail unico do usuario |
| `password_hash` | string \\| null | Hash da senha, opcional |
| `is_verified` | boolean | Indica se o e-mail ja foi verificado |
| `is_active` | boolean | Indica se a conta esta ativa |
| `created_at` | timestamp | Criacao do registro |
| `updated_at` | timestamp | Ultima atualizacao do registro |

### `folders`

Agrupa analises na biblioteca do usuario.

| Campo | Tipo | Descricao |
|---|---|---|
| `id` | string(26) | Identificador primario da pasta |
| `name` | string | Nome da pasta |
| `account_id` | string(26) | Dono da pasta; relacao logica com `accounts` |
| `is_archived` | boolean | Indica se a pasta foi arquivada |
| `created_at` | timestamp | Criacao do registro |
| `updated_at` | timestamp | Ultima atualizacao do registro |

### `analyses`

Representa o agregado principal do intake. Hoje ela guarda ownership, estado do fluxo e os filtros usados na busca de precedentes.

| Campo | Tipo | Descricao |
|---|---|---|
| `id` | string(26) | Identificador primario da analise |
| `name` | string | Nome da analise |
| `folder_id` | string(26) \\| null | Pasta associada; relacao logica com `folders` |
| `account_id` | string(26) | Dono da analise; relacao logica com `accounts` |
| `status` | string | Status atual do fluxo |
| `is_archived` | boolean | Indica se a analise foi arquivada |
| `precedents_search_courts` | json \\| null | Lista de tribunais filtrados na busca |
| `precedents_search_precedent_kinds` | json \\| null | Lista de tipos de precedente filtrados |
| `precedents_search_limit` | integer \\| null | Limite solicitado para busca de precedentes |
| `created_at` | timestamp | Criacao do registro |
| `updated_at` | timestamp | Ultima atualizacao do registro |

Status atualmente usados no dominio:

- `WAITING_PETITION`
- `PETITION_UPLOADED`
- `ANALYZING_PETITION`
- `PETITION_ANALYZED`
- `SEARCHING_PRECEDENTS`
- `ANALYZING_PRECEDENTS_SIMILARITY`
- `ANALYZING_PRECEDENTS_APPLICABILITY`
- `GENERATING_SYNTHESIS`
- `WAITING_PRECEDENT_CHOISE`
- `PRECEDENT_CHOSED`
- `FAILED`

### `petitions`

Armazena o documento de peticao submetido para uma analise.

| Campo | Tipo | Descricao |
|---|---|---|
| `id` | string(26) | Identificador primario da peticao |
| `analysis_id` | string(26) | FK para `analyses.id` com `on delete cascade` |
| `uploaded_at` | timestamp | Momento do upload |
| `document_file_path` | string | Caminho do arquivo no storage |
| `document_name` | string | Nome original do documento |
| `created_at` | timestamp | Criacao do registro |
| `updated_at` | timestamp | Ultima atualizacao do registro |

Observacao: o modelo permite 1:N entre `analyses` e `petitions`, mas o fluxo da aplicacao substitui a peticao anterior quando uma nova e enviada para a mesma analise.

### `petition_summaries`

Guarda a saida estruturada da IA para uma peticao, em relacao 1:1 com `petitions`.

| Campo | Tipo | Descricao |
|---|---|---|
| `petition_id` | string(26) | PK e FK para `petitions.id` com `on delete cascade` |
| `case_summary` | text | Resumo geral do caso |
| `legal_issue` | text | Tese ou problema juridico principal |
| `central_question` | text | Pergunta juridica central |
| `relevant_laws` | json | Lista de normas relevantes |
| `key_facts` | json | Lista de fatos-chave |
| `search_terms` | json | Termos usados para busca de precedentes |
| `type_of_action` | text \\| null | Tipo de acao, quando identificado |
| `secondary_legal_issues` | json | Teses secundarias |
| `alternative_questions` | json | Perguntas juridicas alternativas |
| `jurisdiction_issue` | text \\| null | Questao de competencia, se houver |
| `standing_issue` | text \\| null | Questao de legitimidade, se houver |
| `requested_relief` | json | Pedidos identificados |
| `procedural_issues` | json | Pontos processuais relevantes |
| `excluded_or_accessory_topics` | json | Temas laterais ou excluidos |
| `created_at` | timestamp | Criacao do registro |
| `updated_at` | timestamp | Ultima atualizacao do registro |

### `precedents`

Catalogo local de precedentes hidratados a partir do Pangea.

| Campo | Tipo | Descricao |
|---|---|---|
| `id` | string(26) | Identificador primario do precedente |
| `court` | string(10) | Tribunal de origem |
| `kind` | string(10) | Tipo do precedente |
| `number` | integer | Numero identificador |
| `status` | string(120) | Status retornado pela fonte externa |
| `enunciation` | text | Enunciado do precedente |
| `thesis` | text | Tese do precedente |
| `last_updated_in_pangea_at` | timestamp | Ultima sincronizacao conhecida no Pangea |
| `created_at` | timestamp | Criacao do registro |
| `updated_at` | timestamp | Ultima atualizacao do registro |

Restricao atual:

- `UniqueConstraint(court, kind, number)` com nome `uq_precedent_identifier`

### `analysis_precedents`

Materializa o relacionamento entre analise e precedente, incluindo sinais de ranqueamento, classificacao e sintese final.

| Campo | Tipo | Descricao |
|---|---|---|
| `analysis_id` | string(26) | PK composta e FK para `analyses.id` |
| `precedent_id` | string(26) | PK composta e FK para `precedents.id` |
| `is_chosen` | boolean | Indica se o precedente foi escolhido pelo usuario |
| `similarity_score` | float \\| null | Score agregado legado/opcional |
| `thesis_similarity_score` | float | Similaridade com a tese |
| `enunciation_similarity_score` | float | Similaridade com o enunciado |
| `total_search_hits` | integer | Quantidade total de ocorrencias encontradas |
| `similarity_rank` | integer | Posicao no ranking de similaridade |
| `final_rank` | integer | Posicao final apos reranqueamento |
| `applicability_level` | integer | Nivel ordinal de aplicabilidade |
| `synthesis` | text \\| null | Sintese gerada para o par analise-precedente |
| `created_at` | timestamp | Criacao do registro |
| `updated_at` | timestamp | Ultima atualizacao do registro |

Valores atuais de `applicability_level`:

- `0`: `NOT_APPLICABLE`
- `1`: `POSSIBLY_APPLICABLE`
- `2`: `APPLICABLE`

### `analysies_precedent_legal_features`

Persistencia 1:1 dos sinais juridicos extraidos para um item de `analysis_precedents`.

| Campo | Tipo | Descricao |
|---|---|---|
| `analysis_id` | string(26) | PK composta e FK composta para `analysis_precedents` |
| `precedent_id` | string(26) | PK composta e FK composta para `analysis_precedents` |
| `central_issue_match` | integer | Grau de aderencia ao problema central |
| `structural_issue_match` | integer | Grau de aderencia estrutural |
| `context_compatibility` | integer | Compatibilidade de contexto |
| `is_lateral_topic` | integer | Sinaliza tema lateral |
| `is_accessory_topic` | integer | Sinaliza tema acessorio |
| `created_at` | timestamp | Criacao do registro |
| `updated_at` | timestamp | Ultima atualizacao do registro |

### `analysis_precedent_applicability_feedbacks`

Registra feedback rotulado sobre a aplicabilidade atribuida a um item de `analysis_precedents`.

| Campo | Tipo | Descricao |
|---|---|---|
| `analysis_id` | string(26) | PK composta e FK composta para `analysis_precedents` |
| `precedent_id` | string(26) | PK composta e FK composta para `analysis_precedents` |
| `applicability_level` | integer | Rotulo atribuido ao item |
| `is_from_human` | boolean | Distingue feedback humano de feedback derivado |
| `created_at` | timestamp | Criacao do registro |
| `updated_at` | timestamp | Ultima atualizacao do registro |

### `analysis_precedent_dataset_rows`

Snapshot denormalizado usado para montagem de dataset de treinamento/avaliacao sobre aplicabilidade de precedentes.

| Campo | Tipo | Descricao |
|---|---|---|
| `analysis_id` | string(26) | PK composta e FK composta para `analysis_precedents` |
| `precedent_id` | string(26) | PK composta e FK composta para `analysis_precedents` |
| `applicability_level` | integer | Rotulo final armazenado na linha |
| `is_from_human` | boolean | Indica se o rotulo veio de humano |
| `thesis_similarity_score` | float | Similaridade com a tese |
| `enunciation_similarity_score` | float | Similaridade com o enunciado |
| `total_search_hits` | integer | Total de ocorrencias encontradas |
| `similarity_rank` | integer | Posicao no ranking |
| `identifier_court` | string(10) | Tribunal do precedente |
| `identifier_kind` | string(10) | Tipo do precedente |
| `identifier_number` | integer | Numero do precedente |
| `precedent_status` | string(120) | Status do precedente no momento do snapshot |
| `last_updated_in_pangea_at` | timestamp | Ultima atualizacao conhecida no Pangea |
| `central_issue_match` | integer | Sinal juridico de aderencia central |
| `structural_issue_match` | integer | Sinal juridico estrutural |
| `context_compatibility` | integer | Compatibilidade contextual |
| `is_lateral_topic` | integer | Sinal de tema lateral |
| `is_accessory_topic` | integer | Sinal de tema acessorio |
| `created_at` | timestamp | Criacao do registro |
| `updated_at` | timestamp | Ultima atualizacao do registro |

## Relacionamentos

| Relacao | Cardinalidade | Observacao |
|---|---|---|
| `accounts` -> `folders` | 1:N | Ownership logico por `account_id`, sem FK fisica |
| `accounts` -> `analyses` | 1:N | Ownership logico por `account_id`, sem FK fisica |
| `folders` -> `analyses` | 1:N | Agrupamento logico por `folder_id`, sem FK fisica |
| `analyses` -> `petitions` | 1:N | FK fisica com cascade delete |
| `petitions` -> `petition_summaries` | 1:1 | PK tambem e FK com cascade delete |
| `analyses` <-> `precedents` | N:N | Mediado por `analysis_precedents` |
| `analysis_precedents` -> `analysies_precedent_legal_features` | 1:1 | FK composta com cascade delete |
| `analysis_precedents` -> `analysis_precedent_applicability_feedbacks` | 1:N | FK composta com cascade delete |
| `analysis_precedents` -> `analysis_precedent_dataset_rows` | 1:N | FK composta com cascade delete |

## Observacoes Importantes

- `created_at` e `updated_at` nao aparecem explicitamente em todos os models porque sao herdados da classe base `Model`.
- O banco hoje usa IDs curtos em `string(26)` de forma consistente nas tabelas principais.
- `analyses.account_id`, `folders.account_id` e `analyses.folder_id` sao indexados, mas nao possuem `ForeignKey` declarada no model atual.
- O nome `WAITING_PRECEDENT_CHOISE` e `PRECEDENT_CHOSED` reflete exatamente os valores atuais do dominio.
