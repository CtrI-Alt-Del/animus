<div align="center">
  <h1>Sistema de IA para Análise de Precedentes Jurídicos: Animus ⚖️</h1>
</div>

## 🎯 Descrição do desafio

No cenário jurídico atual, a busca por precedentes relevantes é essencial para fundamentar decisões e estratégias processuais, porém o grande volume de decisões distribuídas entre diferentes tribunais torna essa tarefa complexa e demorada. O desafio consiste em desenvolver uma aplicação móvel integrada à nuvem que utilize técnicas de aprendizagem de máquina para analisar o texto de petições iniciais, gerar um resumo das informações principais e identificar precedentes relevantes em bases nacionais. A solução deve apresentar dados estruturados como tribunal de origem, tema, enunciado, status, tese firmada e um indicador de similaridade ou probabilidade de aplicabilidade ao caso, além de uma síntese explicativa da relação com o processo, auxiliando profissionais do Direito a realizar pesquisas mais rápidas e precisas.

## 📖 Backlog do Produto

| RF | Rank | Prioridade | User story | Estimativa | Sprint |
|-----|------|------------|------------|------------|--------|
| 02 | 1 | Alta | Como Juiz, quero analisar uma petição judicial, para escolher qual precedente se aplica melhor ao caso que eu irei julgar. | 21 | 1 |
| 03 | 2 | Alta | Como Juiz, quero ter acesso à uma lista de precedentes judiciais que são aplicáveis à petição que estou analisando, para decidir em qual delas irei me basear para julgar o caso. | 21 | 1 |
| 03 | 3 | Alta | Como Juiz, quero ter acesso à um relatório sobre a análise da petição e dos precedentes, para entender como eles se relacionam e verificar o quão aplicáveis são os precedentes que eu recebi. | 21 | 1 |
| 03 | 4 | Média | Como Juiz, quero receber um resumo sobre a petição que irei julgar, para entender melhor o tema e verificar informações importantes sobre o caso que eu irei julgar. | 13 | 2 |
| 04 | 5 | Média | Como Juiz, quero poder rever análises passadas feitas no aplicativo, para revisar informações importantes sobre processos que eu já julguei. | 8 | 2 |
| 04 | 6 | Baixa | Como Juiz, quero organizar as análises do aplicativo em pastas, para que eu possa buscar as análises mais relevantes de acordo com um tema específico que eu desejo rever. | 5 | 2 |
| 04 | 7 | Média | Como Juiz, quero ter acesso às análises mais recentes que eu fiz no aplicativo, para que eu possa rever rapidamente informações relacionadas à esses casos que eu julguei quando eu necessitar. | 8 | 2 |
| 02 | 8 | Baixa | Como Advogado, quero receber uma análise resumida sobre uma petição escrita por mim, para que eu possa revisar o conteúdo e garantir que estou passando todas as informações que julgo serem necessárias. | 8 | 3 |
| 03 | 9 | Baixa | Como Advogado, quero ter acesso à uma lista de precedentes aplicáveis a minha petição, para tentar prever qual pode ser a possível decisão que o juiz terá sobre o caso que eu estou advogando. | 8 | 3 |
| 06 | 10 | Média | Como Juiz, quero receber um PDF contendo todo o relatório de análise e síntese de precedentes referente a petição que irei julgar, para documentar a escolha do precedente que utilizarei no caso atual junto à outros documentos referentes ao processo. | 13 | 3 |

---

## 🗓️ Cronograma e Sprints do projeto

| Sprint |    Período da Sprint    |                                       Link para a documentação                                       |      Status      |
| :----: | :---------------------: | :--------------------------------------------------------------------------------------------------: | :-------------: |
|   01   | 16/03/2026 à 05/04/2026 | [Relatório](https://github.com/CtrI-Alt-Del/animus/blob/main/documentation/sprints/sprint-1-report.md)  | Não Concluida |
|   02   | 13/04/2026 à 03/05/2026 | [Relatório](https://github.com/CtrI-Alt-Del/animus/blob/main/documentation/sprints/sprint-2-report.md)  | Não Concluida |
|   03   | 11/05/2026 à 31/05/2026 | [Relatório](https://github.com/CtrI-Alt-Del/animus/blob/main/documentation/sprints/sprint-3-report.md)  | Não Concluida |

## 🛠️ Tecnologias

O sistema foi desenvolvido utilizando um conjunto de tecnologias para garantir
análise, processamento, persistência e disponibilização dos dados jurídicos:

- **[Flutter](https://flutter.dev/)** → Framework multiplataforma para desenvolvimento
  de interfaces móveis nativas com alta performance e código reutilizável.

- **[Riverpod](https://riverpod.dev/)** → Gerenciador de estado reativo e robusto
  para Flutter, facilitando o gerenciamento de dependências e estado da aplicação.

- **[FastAPI](https://fastapi.tiangolo.com/)** → Framework web moderno e de alta
  performance para construir APIs REST em Python, com validação automática e documentação.

- **[SQLAlchemy](https://www.sqlalchemy.org/)** → ORM (Object-Relational Mapper) Python
  que facilita a interação com bancos de dados relacionais de forma segura e eficiente.

- **[PostgreSQL](https://www.postgresql.org/)** → Banco de dados relacional robusto
  e escalável utilizado para armazenar os dados processados e otimizados para consultas.

- **[Docker](https://www.docker.com/)** → Plataforma de contêineres utilizada
  para empacotar e executar a aplicação em ambientes isolados, facilitando o deploy.

- **[Inngest](https://www.inngest.com/)** → Plataforma para orquestração de fluxos
  de trabalho e processamento de eventos em background de forma confiável e escalável.

- **[OneSignal](https://onesignal.com/)** → Plataforma de notificações push que permite
  enviar mensagens personalizadas para usuários da aplicação móvel em tempo real.

- **[Google Gemini](https://gemini.google.com/)** → Modelo de IA generativa do Google
  utilizado para análise de petições e identificação de padrões jurídicos.

- **[Agno](https://www.agno.com/)** → Framework para construção de agentes de IA autônomos
  e inteligentes capazes de raciocínio e tomada de decisão complexa.

- **[Google Cloud Storage](https://cloud.google.com/storage)** → Serviço de armazenamento
  de objetos escalável e seguro para persistência de arquivos e documentos jurídicos.

- **[Vertex AI](https://cloud.google.com/vertex-ai)** → Plataforma Google para desenvolvimento
  e deploy de modelos de Machine Learning para análise preditiva de precedentes.

- **[Firebase Authentication](https://firebase.google.com/products/auth)** → Serviço de
  autenticação do Firebase que gerencia login seguro de usuários na aplicação.

<!-- ## 🏢 Arquitetura do Projeto

<img src="https://i.imgur.com/JonhNNm.png" /> -->

## 📁 Estrutura do Repositório

O projeto segue uma arquitetura de microserviços com separação clara de
responsabilidades:

- **`animus-server/`**: API REST desenvolvida em FastAPI que gerencia chats,
  processamento e armazenamento dos dados das petições
- **`animus-mobile/`**: Interface desenvolvida em Flutter que disponibiliza
  chats e dados para os usuários
- **`documentation/`**: Documentação técnica e relatórios de progresso do
  projeto

## ▶️ Como executar o projeto

Acesse os repositórios **`animus-server/`** e **`animus-mobile/`** e clone ambos na sua máquina, verifique no **`readme.md`** de cada repositório como rodar o projeto e quais rotas acessar.

<!-- ## 📚 Documentação do projeto -->

<!-- - [DoR (Definition of Ready)](https://github.com/CtrI-Alt-Del/animus/blob/main/documentation/dor.md) -->
<!-- - [DoD (Definition of Done)](https://github.com/CtrI-Alt-Del/animus/blob/main/documentation/dod.md) -->
<!-- - [Estratégia de branches](https://github.com/CtrI-Alt-Del/animus/blob/main/documentation/branch-stragery.md) -->
<!-- - [Padrão de commit](https://github.com/CtrI-Alt-Del/animus/blob/main/documentation/commit-pattern.md) -->
<!-- - [Gestão da automação](https://github.com/CtrI-Alt-Del/animus/blob/main/documentation/gestao-da-automação.md) -->
<!-- - [Gestão do processo](https://github.com/CtrI-Alt-Del/animus/blob/main/documentation/gestao-do-processo.md) -->
<!-- - [Gestão do conhecimento](https://github.com/CtrI-Alt-Del/animus/blob/main/documentation/gestao-do-conhecimento.md) -->

## 👷🏻 Equipe

|                                    Foto                                    |          Nome           |    Função     |                                                                            Github                                                                            |                                                                                              Linkedin                                                                                               |
| :------------------------------------------------------------------------: | :---------------------: | :-----------: | :----------------------------------------------------------------------------------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|       <img src="https://github.com/Tico1606.png?size=50" width="50">       |    Gabriel Oliveira     |  Scrum Master   |       <a href="https://github.com/Tico1606"><img src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white"></a>       |          <a href="https://www.linkedin.com/in/gabriel-oliveira-884ba5282/"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white"></a>           |
|       <img src="https://github.com/JoaoGabrielGarcia.png?size=50" width="50">   |  Joao Gabriel Oliveira  | Product Owner |  <a href="https://github.com/JoaoGabrielGarcia"><img src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white"></a>   |  <a href="https://www.linkedin.com/in/jo%C3%A3o-gabriel-oliveira-garcia-b2563a22a/"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white"></a>  |
|       <img src="https://github.com/Miojoguu.png?size=50" width="50"> |  Gustavo Borges Lima  |   Dev Team    | <a href="https://github.com/Miojoguu"><img src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white"></a> |               <a href="https://www.linkedin.com/in/gustavo-lima-904623295/"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white"></a>                |
|       <img src="https://github.com/JohnPetros.png?size=50" width="50">      |   Joao Pedro Carvalho   | Dev Team  |      <a href="https://github.com/JohnPetros"><img src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white"></a>      | <a href="https://www.linkedin.com/in/jo%C3%A3o-pedro-carvalho-dos-santos-42a0ab222/"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white"></a> |
|       <img src="https://github.com/kaufon.png?size=50" width="50">        |  Kauan Fonseca do Vale  |   Dev Team    |        <a href="https://github.com/kaufon"><img src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white"></a>        |            <a href="https://www.linkedin.com/in/kauan-fonseca-b62188300/"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white"></a>            |
|       <img src="https://github.com/0thigs.png?size=50" width="50">        |     Thiago Martins      |   Dev Team    |        <a href="https://github.com/0thigs"><img src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white"></a>        |            <a href="https://www.linkedin.com/in/thiago-martins-fullstack/"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white"></a>             |
