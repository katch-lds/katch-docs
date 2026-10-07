# Documentação de Arquitetura — Katch

**Unidade curricular:** Laboratório de Desenvolvimento de Software — LDS
**Milestone:** `m2`
**Ficheiro:** `m2-documentacao-arquitetura-v01.md`
**Pasta de arquivo:** `04-artefactos-tecnicos/04.05-arquitetura`

---

## 1. Identificação do documento

| Campo | Informação |
| --- | --- |
| Grupo | G11 |
| Sistema | Katch |
| Ano letivo | 2026/2027 |
| Curso | LEI — Licenciatura em Engenharia Informática |
| Turma | LEI3T2 |
| Versão do documento | v01 |
| Documentos de origem | `m1-proposta-sistema-v02.pdf`, `m1-declaracao-ambito-v02.pdf`, Regulamento Interno do Grupo v01 (`m1-regulamento-grupo-v01.pdf`, secções 5, 10 e 11), `m2-backlog-projeto-v02.xlsx` (Issues `I033`, `I034` e `I051`), `m2-modelo-de-dados-postgresql-v01.md` e `m2-especificacao-requisitos-v01.md` |

### 1.1. Responsáveis por secção

O documento é consolidado por várias Issues do Sprint 02. Cada secção indica a Issue que a produz, para que a revisão e a auditoria de cada tarefa incidam apenas sobre a parte que lhe corresponde.

| Secção | Issue | Executor | Revisor | Auditor |
| --- | --- | --- | --- | --- |
| 2. Visão geral, componentes, responsabilidades, relações, fronteiras, interfaces, restrições, padrões, atributos de qualidade e evolução prevista | `I033` | João Coelho | João Borguem | Roberto Baptista |
| 3. Tecnologias e decisões tecnológicas | `I034` | Roberto Baptista | João Borguem | Miguel Santos |

### 1.2. Histórico de versões

| Versão | Data | Descrição das alterações | Issue |
| --- | --- | --- | --- |
| v01 | 2026-10-01 | Criação do documento e da secção 3 (tecnologias e decisões tecnológicas). | `I034` |
| v01 | 2026-10-03 | Acrescento da secção 2 (arquitetura do sistema). | `I033` |
| v01 | 2026-10-07 | Correção da secção 2 após revisão: entrega de mensagens e notificações por ligação bidirecional persistente (DA v02), com a consulta periódica como alternativa proposta e pendente de decisão; AD-01 identificada como proposta; referência à especificação de requisitos v02. | `I033` |

Cada alteração posterior acrescenta uma linha. As versões anteriores são conservadas, nos termos da secção 18.2 do Regulamento de Funcionamento da Unidade Curricular.

---

## 2. Arquitetura do sistema

### 2.1. Visão geral

O Katch tem dois clientes e um servidor. O Candidato usa a aplicação móvel e o Recrutador e o Administrador usam a área de gestão web. Os dois clientes comunicam com um único backend, que é o único componente com acesso à base de dados e aos ficheiros. O sistema não tem integração com serviços externos.

Esta secção descreve a estrutura do sistema: componentes, ligações, fronteiras, padrões e atributos de qualidade. As tecnologias e a sua justificação estão na secção 3. As classes, as tabelas e os fluxos de negócio são tratados nos modelos de classes, de dados e de comportamento.

### 2.2. Diagrama de arquitetura

Como o Mermaid não tem um tipo de diagrama de arquitetura, o diagrama usa um `flowchart`. Os atores são retângulos fora da fronteira, a fronteira é o retângulo «Sistema Katch» e o retângulo «Backend» agrupa os componentes internos do servidor. As setas contínuas são pedidos dos clientes e as setas a tracejado são a ligação bidirecional persistente que entrega mensagens e notificações em tempo real (tecnologia por confirmar, proposta D-08). As cores estão fixadas no próprio diagrama, para que se leia da mesma forma em tema claro e em tema escuro.

```mermaid
flowchart TB
    Cand["Candidato"]
    Rec["Recrutador"]
    Adm["Administrador"]

    subgraph Katch["Sistema Katch"]
        Mobile["Aplicação móvel<br/>Flutter 3.47 · Dart 3.13"]
        Web["Área de gestão web<br/>React 19 · TypeScript 7 · Vite"]

        subgraph Backend["Backend — ASP.NET Core 10 · C# 14"]
            API["API REST<br/>Controllers · autenticação JWT<br/>Swagger/OpenAPI"]
            Serv["Serviços de negócio<br/>regras de F001 a F011"]
            Jobs["Tarefas periódicas<br/>encerrar vagas expiradas<br/>repor quota de interesses"]
            Rep["Acesso a dados<br/>Repositories · Entity Framework Core"]
            FS["Armazenamento de ficheiros<br/>fotografias, logótipos e CV"]
        end

        DB[("PostgreSQL 18<br/>base de dados relacional")]
    end

    Cand --- Mobile
    Rec --- Web
    Adm --- Web

    Mobile -->|"HTTPS · JSON"| API
    Web -->|"HTTPS · JSON"| API
    Mobile -.->|"ligação bidirecional persistente<br/>mensagens e notificações em tempo real<br/>(tecnologia por confirmar · proposta D-08)"| API
    Web -.->|"ligação bidirecional persistente<br/>mensagens e notificações em tempo real<br/>(tecnologia por confirmar · proposta D-08)"| API

    API --> Serv
    Jobs --> Serv
    Serv --> Rep
    Serv --> FS
    Rep -->|"SQL"| DB

    classDef ator fill:#ffffff,stroke:#1f2937,stroke-width:2px,color:#111827
    classDef cliente fill:#eef2ff,stroke:#3730a3,stroke-width:1.5px,color:#111827
    classDef servidor fill:#ecfdf5,stroke:#047857,stroke-width:1.5px,color:#111827
    classDef dados fill:#fff7ed,stroke:#c2410c,stroke-width:1.5px,color:#111827
    class Cand,Rec,Adm ator
    class Mobile,Web cliente
    class API,Serv,Jobs,Rep,FS servidor
    class DB dados
    style Katch fill:#ffffff,stroke:#1f2937,stroke-width:2px,color:#111827
    style Backend fill:#f9fafb,stroke:#6b7280,stroke-width:1.5px,color:#111827
    linkStyle default stroke:#374151,stroke-width:1.5px
```

### 2.3. Componentes e responsabilidades

| Componente | Responsabilidade | Tecnologia | Repositório | Funcionalidades |
| --- | --- | --- | --- | --- |
| Aplicação móvel | Interface exclusiva do Candidato: registo, perfil, exploração de vagas, matches, conversas e notificações. | Flutter 3.47, Dart 3.13 | `katch-frontend-mobile` | F001, F002, F004, F006, F008, F010, F011 |
| Área de gestão web | Interface do Recrutador (Empresa, vagas, candidatos em espera, matches, conversas e notificações) e do Administrador (Empresas, contas, indicadores e listas pré-definidas). | React 19, TypeScript 7, Vite 8 | `katch-frontend-web` | F001, F002, F003, F005, F007, F008, F009, F010, F011 |
| API REST | Ponto de entrada dos clientes. Recebe os pedidos, autentica-os com JWT, verifica o tipo de conta e devolve as respostas. Aceita também a ligação bidirecional persistente por onde segue a entrega, em tempo real, das mensagens e das notificações (tecnologia por confirmar, proposta D-08). Documentada com Swagger/OpenAPI. | ASP.NET Core 10 (Web API), Swashbuckle | `katch-backend` | F001 a F011 |
| Serviços de negócio | Regras do domínio: validação automática, quota de interesses, filtragem de vagas e cálculo de distância, confirmação do match, estados da Empresa, da vaga e da conta, e geração de notificações. | C# 14 | `katch-backend` | F001 a F011 |
| Tarefas periódicas | Encerrar as vagas que atingiram a data-limite e repor a quota de interesses, com a notificação respetiva. | Serviços em segundo plano do ASP.NET Core | `katch-backend` | F005, F006, F011 |
| Acesso a dados | Persistência das entidades e consultas, com migrations e controlo de concorrência otimista. | Entity Framework Core (code-first), Npgsql | `katch-backend` | F001 a F011 |
| Armazenamento de ficheiros | Guarda as fotografias, os logótipos e os CV. A base de dados guarda apenas o caminho de cada ficheiro. | Sistema de ficheiros do servidor do backend | `katch-backend` | F003, F004, F005 |
| Base de dados | Guarda os dados do sistema, segundo o modelo de dados. | PostgreSQL 18 | `katch-backend` (migrations) | F001 a F011 |

### 2.4. Interfaces e relações

| Ligação | Protocolo e formato | Notas |
| --- | --- | --- |
| Aplicação móvel → API REST | HTTPS, JSON | Pedidos autenticados com o token JWT. Só o tipo de conta Candidato acede por aqui. |
| Área de gestão web → API REST | HTTPS, JSON | Pedidos autenticados com o token JWT. Só os tipos de conta Recrutador e Administrador acedem por aqui. |
| Aplicações → backend (ligação bidirecional persistente) | Ligação persistente entre cada cliente e o backend; tecnologia por confirmar (proposta D-08) | A DA v02 exige, na F010, uma ligação bidirecional persistente entre cliente e servidor, e a F011 reutiliza esse mecanismo. Entrega as mensagens e as notificações em tempo real, dentro das aplicações. A especificação de requisitos (parâmetro P17, RF029, RF073, RF110 e RF111) fixa o limite de 5 segundos para a entrega. Não há notificações nativas do sistema operativo nem envio por e-mail ou SMS. A tecnologia é a proposta D-08 (ASP.NET Core SignalR), por confirmar em reunião formal do grupo (Regulamento da UC, secção 10.1). |
| API REST e Tarefas periódicas → Serviços de negócio | Chamadas internas ao backend | As regras de negócio são executadas nos serviços. |
| Serviços de negócio → Acesso a dados → Base de dados | SQL, via Entity Framework Core | Única via de acesso à base de dados. |
| Serviços de negócio → Armazenamento de ficheiros | Leitura e escrita de ficheiros | Os clientes nunca acedem diretamente aos ficheiros nem à base de dados. |

O que está aprovado é o mecanismo: a ligação bidirecional persistente da DA v02. A tecnologia que o concretiza ainda não está decidida. A decisão D-08 da secção 3.5 propõe o ASP.NET Core SignalR e está por ratificar em reunião formal do grupo (Regulamento da UC, secção 10.1). A consulta periódica à API REST é uma alternativa proposta e pendente, descrita em `m2-decisao-consulta-periodica-mensagens-notificacoes-v01.md`; só substitui a ligação persistente se for aprovada em reunião formal e registada em ata.

### 2.5. Fronteiras e restrições

A fronteira segue a Declaração de Âmbito v02.

| Dentro da fronteira | Fora da fronteira (excluído pela DA v02) |
| --- | --- |
| Aplicação móvel, área de gestão web, backend e base de dados, desenvolvidos pelo grupo. | Fornecedores externos de identidade; portais de emprego, redes profissionais e sistemas de recursos humanos; serviços de pagamento. |
| Conversas e notificações, entregues apenas dentro das aplicações. | Serviços externos de mapas ou de geolocalização (a distância usa as coordenadas da lista pré-definida de localidades); envio de e-mail, SMS ou notificações para fora da plataforma; serviços comerciais de comunicação. |

Restrições que condicionam a arquitetura:

- O sistema usa apenas a língua portuguesa e dados fictícios, em ambiente académico.
- A base de dados de entrega é PostgreSQL. Em desenvolvimento corre em `localhost:6000` e é publicada depois por port forwarding no router do grupo. O SQLite em memória só é permitido nos testes automáticos.
- Cada componente tem o seu repositório e a sua pipeline CI/CD no GitHub Actions, com análise estática no SonarQube.

### 2.6. Padrões arquiteturais

| Padrão | Aplicação |
| --- | --- |
| Cliente–servidor com interface REST única | Os dois clientes usam a mesma interface do servidor, em HTTPS e JSON. |
| Backend em camadas | Controllers, Services, Repositories e Domain, como no âmbito de cobertura de testes do Regulamento Interno. Os Controllers não acedem diretamente à base de dados. |
| Autenticação sem estado de sessão no servidor | Token JWT em cada pedido. O estado da conta e da Empresa é confirmado na base de dados em cada operação reservada (secção 3.5, D-04). |
| Persistência code-first | Entity Framework Core, com migrations para criar e evoluir o esquema. |
| Concorrência otimista | Coluna de sistema `xmin` na tabela `match`, para que duas alterações simultâneas ao mesmo registo não se sobreponham. |
| Ligação bidirecional persistente | Entrega de mensagens e notificações em tempo real, como exige a DA v02 (F010 e F011). Tecnologia por confirmar (proposta D-08, ASP.NET Core SignalR, por ratificar em reunião formal). |
| Tarefas periódicas em segundo plano | Encerramento das vagas expiradas e reposição da quota de interesses. |
| Base de dados única | Uma só base de dados PostgreSQL, acedida apenas pelo backend. |

### 2.7. Atributos de qualidade

A tabela indica, para cada atributo, os requisitos não funcionais da especificação de requisitos (secção 5) e o componente que os suporta.

| Atributo | Requisitos | Componentes |
| --- | --- | --- |
| Desempenho | RNF001 (operações do servidor em 2 segundos), RNF002 (cartão de vaga seguinte em 2 segundos), RNF015 (ficheiro de 5 MB em 5 segundos); parâmetro P17 (mensagens e notificações em 5 segundos) | API REST, Serviços de negócio, Acesso a dados, Armazenamento de ficheiros |
| Segurança | RNF003 (palavras-passe em resumo irreversível), RNF004 a RNF006 e RNF009 (credencial de sessão, expiração e autorização de cada pedido), RNF007 e RNF016 (CV e conteúdo dos ficheiros), RNF008 (validação automática no servidor), RNF017 (dados confidenciais fora dos registos de diagnóstico) | API REST, Serviços de negócio, Armazenamento de ficheiros |
| Usabilidade | RNF010 a RNF012 (interações na aplicação móvel e área de gestão web sem deslocamento horizontal) | Aplicação móvel, Área de gestão web |
| Fiabilidade e disponibilidade | RNF013 (dados confirmados conservados após reinício do servidor), RNF014 e RNF018 (indicação de falha de ligação em 10 segundos) | Base de dados, Aplicação móvel, Área de gestão web |

### 2.8. Decisões de arquitetura

| N.º | Decisão | Alternativas | Justificação |
| --- | --- | --- | --- |
| AD-01 (proposta) | Propõe-se guardar os ficheiros (fotografias, logótipos e CV) no sistema de ficheiros do servidor do backend, com a base de dados a guardar apenas o caminho. | Guardar os ficheiros na base de dados; usar um serviço externo de armazenamento. | O modelo de dados mantém os ficheiros fora da base de dados. A DA exclui os serviços externos. A solução não acrescenta tecnologia à stack. Se for aprovada, resolve o ponto 3 da secção 3.6. **Estado:** proposta, por confirmar em reunião formal do grupo (Plano de Qualidade, secção 7). Ata: a indicar quando existir. |
| AD-02 | As mensagens e as notificações são entregues por ligação bidirecional persistente entre cliente e servidor, como aprovado na DA v02 (F010; a F011 reutiliza o mecanismo). A tecnologia está por confirmar: a proposta D-08 é o ASP.NET Core SignalR. | Consulta periódica à API REST a cada 3 segundos (proposta pendente de decisão, ver `m2-decisao-consulta-periodica-mensagens-notificacoes-v01.md`); WebSockets diretos; serviços comerciais de notificação. | A DA exige ligação persistente em tempo real (F010, F011) e exclui os serviços comerciais externos (limites transversais). O SignalR é parte do ASP.NET Core e não acrescenta tecnologia ao backend. A consulta periódica é só uma alternativa proposta: até ser aprovada em reunião formal e registada em ata, vale o que está aprovado na DA. **Estado:** a tecnologia (D-08) e a alternativa de consulta periódica estão por decidir (Regulamento da UC, secção 10.1). Ata: a indicar quando existir. |

### 2.9. Evolução prevista

- **Entrega de mensagens e notificações.** A entrega usa a ligação bidirecional persistente aprovada na DA v02. A consulta periódica à API REST é apenas uma alternativa proposta e pendente de decisão (`m2-decisao-consulta-periodica-mensagens-notificacoes-v01.md`). Se for aprovada em reunião formal e registada em ata, a mudança afeta a API REST e os clientes, e não os Serviços de negócio nem o modelo de dados.
- **Armazenamento de ficheiros.** O acesso aos ficheiros fica concentrado nos Serviços de negócio, o que permite mudar o local de armazenamento sem alterar as regras de negócio.

---

## 3. Tecnologias e decisões tecnológicas

### 3.1. Objetivo e regra de coerência

Esta secção identifica a tecnologia de cada componente do Katch e explica porque foi escolhida. As versões são as fixadas na secção 11.2 do Regulamento Interno do Grupo v01. Quando o Regulamento Interno não fixa uma versão, isso é dito expressamente e a versão fica fixada no ficheiro de dependências do repositório respetivo. As tecnologias que o Regulamento Interno não menciona (JWT, canal em tempo real, tarefas em background, armazenamento de ficheiros) têm a origem indicada na coluna «Origem da versão» da secção 3.3.

Nos termos da secção 11.1 do Regulamento Interno, qualquer alteração a uma tecnologia, versão, regra ou limiar fixado na secção 11 exige uma nova versão do Regulamento Interno. Esta secção não altera nenhuma dessas escolhas; limita-se a documentá-las e a justificá-las.

### 3.2. Mapa da stack

```mermaid
flowchart LR
    subgraph Mobile["Frontend mobile — Candidato"]
        M1["Flutter 3.47.x<br/>Dart 3.13.x"]
    end
    subgraph Web["Frontend web — Recrutador e Administrador"]
        W1["React 19.3.x<br/>TypeScript 7.0.x<br/>Vite 8.3.x · Node.js 24 LTS"]
    end
    subgraph Backend["Backend — API"]
        B1["ASP.NET Core 10 Web API<br/>C# 14 · .NET SDK 10.0 LTS"]
        B2["Autenticação JWT Bearer"]
        B3["Swagger/OpenAPI (Swashbuckle)"]
        B4["Entity Framework Core<br/>code-first + migrations<br/>fornecedor Npgsql"]
        B5["Canal em tempo real<br/>ligação bidirecional persistente<br/>(proposta: ASP.NET Core SignalR)"]
        B6["Tarefas periódicas em background<br/>(ASP.NET Core hosted services)"]
    end
    subgraph Dados["Persistência"]
        D1[("PostgreSQL 18")]
        D2[("Armazenamento de ficheiros<br/>fotos, CV, logótipos<br/>Sistema de ficheiros do servidor do backend — secção 3.6")]
    end
    M1 -- "HTTPS / JSON + token JWT" --> B1
    W1 -- "HTTPS / JSON + token JWT" --> B1
    M1 -. "mensagens e notificações" .-> B5
    W1 -. "mensagens e notificações" .-> B5
    B1 --- B2
    B1 --- B3
    B1 --- B5
    B1 --- B6
    B1 --> B4 --> D1
    B1 --> D2
    B6 --> B4
```

O diagrama mostra apenas a correspondência entre componentes e tecnologias. A estrutura interna, as interfaces e os padrões arquiteturais são tratados na secção 2 (`I033`).

### 3.3. Tecnologias por componente

| Componente | Tecnologia e versão | Origem da versão | Justificação |
| --- | --- | --- | --- |
| Backend — linguagem e framework | C# 14 com ASP.NET Core 10 (Web API) | RI v01, secção 11.2 | Framework maduro para APIs REST, com injeção de dependências, validação, autenticação e documentação OpenAPI integradas. Cumpre o requisito da UC de um backend funcional com testes, cobertura, análise estática e collection Postman. |
| Backend — SDK e execução | .NET SDK 10.0 (LTS) | RI v01, secção 11.2 | Versão com suporte de longo prazo (até novembro de 2028), o que garante atualizações de segurança durante todo o semestre e depois dele. |
| Backend — documentação da API | Swagger/OpenAPI com Swashbuckle, disponível em `/swagger` | RI v01, secções 5 e 11.2 | Permite explorar e testar os endpoints sem ferramentas externas e serve de referência à documentação da API (`04.11`). |
| Backend — acesso a dados | Entity Framework Core, abordagem code-first com migrations (`dotnet ef migrations`), com o fornecedor Npgsql e o pacote `EFCore.NamingConventions` (`UseSnakeCaseNamingConvention()`) | EF Core: RI v01, secção 11.2 (o RI não fixa a versão). Npgsql e `EFCore.NamingConventions`: modelo de dados (`m2-modelo-de-dados-katch-v01.md`); versão não fixada no RI | O modelo de dados fica descrito em C# e versionado com o código; as migrations tornam reproduzível a criação da base de dados em qualquer máquina do grupo e na pipeline. O Npgsql é necessário para o `xmin` como controlo de concorrência otimista em `match`, para os arrays e para os tipos enumerados do esquema. |
| Backend — canal em tempo real | Ligação bidirecional persistente entre cliente e servidor; proposta: ASP.NET Core SignalR (parte do framework) | DA v02 (F010, F011); proposta de tecnologia em D-08 — não fixada no RI | A DA impõe que as mensagens (F010) e as notificações (F011) sejam entregues em tempo real «através de uma ligação bidirecional persistente entre cliente e servidor», assegurada pelo próprio sistema. A consulta periódica à API não cumpre esta exigência. |
| Backend — tarefas periódicas | Hosted services do ASP.NET Core (`BackgroundService`) | Modelo de dados (`m2-modelo-de-dados-katch-v01.md`); framework do v01, secção 11.2 | Encerrar vagas cuja data-limite expirou (F005) e repor a quota de interesses com a respetiva notificação (F006, F011). Não exige nenhuma tecnologia adicional ao backend. |
| Armazenamento de ficheiros | Sistema de ficheiros do servidor do backend | Modelo de dados (`m2-modelo-de-dados-katch-v01.md`: ficheiros fora da base de dados) | Fotos de perfil, CV, logótipos e galerias ficam fora da base de dados, que guarda apenas o caminho ou URL (`varchar(500)`). O local de armazenamento ainda não está decidido. |
| Base de dados | PostgreSQL 18 | RI v01, secções 5 e 11.2 | Base de dados relacional robusta e gratuita, adequada às relações do domínio (Candidato, Empresa, vaga, interesse, match). Em desenvolvimento corre em `localhost`, porta 6000 (RI, secção 5). |
| Base de dados — testes isolados | SQLite in-memory | RI v01, secções 5 e 11.2 | Usada apenas nos testes de integração do backend, para isolar cada execução. A base de dados de produção e entrega continua a ser PostgreSQL. Limitação: o SQLite não reproduz funcionalidades PostgreSQL usadas pelo esquema v01 (ver secção 3.6, ponto 4). |
| Autenticação e autorização | JSON Web Token (JWT) com o esquema JWT Bearer do ASP.NET Core 10 (pacote oficial `Microsoft.AspNetCore.Authentication.JwtBearer`, na versão correspondente ao .NET 10) | Backlog de Projeto (`I033`, `I034`, `I051`); não fixada no RI — ver decisão D-04 | Autenticação sem estado de sessão, adequada a dois clientes distintos (aplicação móvel e área de gestão web) que consomem a mesma API. O tipo de conta (Candidato, Recrutador ou Administrador) segue no token, que tem expiração curta. Como a F001 determina que uma conta bloqueada ou desativada, ou associada a uma empresa não aprovada, não tem acesso às operações reservadas, o backend confirma na base de dados o estado da conta (`app_user.status`) e da empresa (`company.status`) em cada operação reservada, e não confia apenas no conteúdo do token. O segredo de assinatura é guardado em variável de ambiente (desenvolvimento) ou GitHub Secrets (pipeline), nos termos do RI, secção 14.2. |
| Frontend web — linguagem e framework | TypeScript 7.0.x com React 19.3.x | RI v01, secção 11.2 | React é a biblioteca de interfaces mais usada e com mais documentação; a tipagem estática do TypeScript reduz erros na integração com a API. |
| Frontend web — construção | Vite 8.3.x, npm, Node.js 24 LTS | RI v01, secção 11.2 | Arranque e recompilação rápidos em desenvolvimento e build de produção simples, executável na pipeline. Node.js LTS garante suporte durante o semestre. |
| Frontend mobile | Flutter 3.47.x com Dart 3.13.x (Flutter SDK, pub) | RI v01, secção 11.2 | Um único código para Android e iOS, com widgets próprios que facilitam a interação por cartões da exploração de vagas (F006). |
| Integração contínua | GitHub Actions (`.github/workflows/ci.yml` em `katch-backend`, `katch-frontend-web` e `katch-frontend-mobile`) | RI v01, secção 11.6 | Integrado no GitHub, onde já estão os repositórios e as regras de proteção da `main`; executa os quality gates QG-01 a QG-07 em cada Merge Request. |
| Análise estática | SonarQube Server 2026.1 LTA (Community Build, self-hosted), com o Quality Profile «Sonar way» de cada linguagem | RI v01, secções 5 e 11.4 | Uma única ferramenta para os três componentes, com Quality Gate que bloqueia a pipeline (QG-05). A edição LTA mantém-se estável durante todo o projeto. |
| Linters e formatação — backend | Roslyn Analyzers + StyleCop.Analyzers; `dotnet format` | RI v01, secções 11.3 e 11.4 | Aplicam as Microsoft C# Coding Conventions, com severidade `Error` a bloquear a pipeline. |
| Linters e formatação — web | ESLint (TypeScript + React) e Prettier | RI v01, secções 11.3 e 11.4 | O ESLint, com `eslint:recommended`, `react-hooks/recommended` e `@typescript-eslint/recommended`, e o Prettier verificam a convenção adotada (Airbnb TypeScript/React Style Guide), com severidade `error` a bloquear a pipeline. |
| Linters e formatação — mobile | Dart analyzer com `flutter_lints`; `dart format` | RI v01, secções 11.3 e 11.4 | Conjunto de regras recomendado pela equipa do Flutter, alinhado com o Effective Dart. |
| Testes — backend | xUnit (projetos `Katch.Backend.UnitTests` e `Katch.Backend.IntegrationTests`, com `WebApplicationFactory`); cobertura com coverlet + reportgenerator | RI v01, secções 11.2 e 11.5 | Framework de testes de referência no .NET; a cobertura de linhas é medida para cumprir o limiar obrigatório de 70% (QG-04). |
| Testes de API | Postman (aplicação desktop), com a collection versionada no repositório | RI v01, secção 5; versão não fixada no RI | Exigência da secção 17 do Regulamento da UC para `m3` (collection Postman importável). A collection é guardada no repositório para que qualquer elemento a execute da mesma forma. |
| Testes — web | Jest (unitários, cobertura mínima de 40%) e Playwright (sistema) | RI v01, secções 5, 11.2 e 11.5; versão não fixada no RI | Jest testa a lógica de componentes e hooks; Playwright automatiza os testes de sistema no browser (QG-06). |
| Testes — mobile | `flutter_test` (unitários e de widget, cobertura mínima de 40%) e `integration_test` (sistema) | RI v01, secções 5, 11.2 e 11.5; integrados no Flutter SDK 3.47.x | Ferramentas oficiais do Flutter; `integration_test` executa os testes de sistema da aplicação (QG-07). |
| Diagramas | Mermaid, com a fonte incluída nos ficheiros Markdown | RI v01, secção 5; Regulamento da UC, secção 17 | Diagramas em texto, editáveis e versionáveis, como exige o Regulamento da UC. |

### 3.4. Ferramentas de desenvolvimento recomendadas

| Componente | IDE ou editor recomendado (RI v01, secção 11.2) |
| --- | --- |
| Backend | Visual Studio 2022, JetBrains Rider ou VS Code (C# Dev Kit) |
| Frontend web | VS Code |
| Frontend mobile | VS Code (extensão Flutter) ou Android Studio |
| Base de dados | pgAdmin ou DBeaver |

As versões das dependências que o RI não fixa ficam fixadas no momento da configuração base de cada repositório: Jest e Playwright no `package.json` e no `package-lock.json` do `katch-frontend-web`; pacote JWT Bearer, Swashbuckle, Npgsql, `EFCore.NamingConventions`, xUnit e coverlet nos ficheiros `.csproj`/`Directory.Build.props` do `katch-backend`; `flutter_lints` e as restantes dependências do mobile no `pubspec.yaml` e no `pubspec.lock` do `katch-frontend-mobile`. O Postman é uma aplicação desktop sem ficheiro de dependências; a versão usada fica registada junto da collection versionada no repositório do backend. A versão instalada passa a ser a referência e só muda através de um Merge Request revisto e auditado.

### 3.5. Decisões tecnológicas

| ID | Decisão | Alternativas consideradas | Fundamentação | Origem |
| --- | --- | --- | --- | --- |
| D-01 | Três componentes autónomos (API, área de gestão web e aplicação móvel), cada um no seu repositório. | Monorepo único; aplicação web responsiva em vez de app móvel. | A DA atribui a aplicação móvel exclusivamente ao Candidato e a área de gestão web ao Recrutador e ao Administrador; repositórios separados permitem pipelines e responsáveis próprios por tecnologia. | DA v02; RI v01, secção 10.1 |
| D-02 | Backend em ASP.NET Core 10 sobre .NET 10 LTS. | Node.js/Express; Java/Spring Boot. | Suporte de longo prazo, desempenho e suporte OpenAPI/Swagger com Swashbuckle. | RI v01, secção 11.2 |
| D-03 | PostgreSQL 18 com EF Core code-first; SQLite in-memory apenas nos testes de integração. | SQL Server; MySQL; abordagem database-first. | O esquema fica versionado com o código; os testes correm isolados e depressa sem depender de um servidor de base de dados. O SQLite não reproduz tudo o que o esquema v01 usa (limitação registada na secção 3.6, ponto 4). | RI v01, secções 5 e 11.2 |
| D-04 | Autenticação por JWT com o esquema JWT Bearer nativo do ASP.NET Core. | Sessões com cookies no servidor; fornecedor externo de identidade. | Os dois clientes (móvel e web) usam a mesma API sem estado de sessão no servidor; o tipo de conta segue no token, mas o estado da conta e da empresa é confirmado na base de dados em cada operação reservada, para que um bloqueio ou suspensão (F001, F003) produza efeito imediato. A secção 3 da DA (limites transversais) exclui expressamente «fornecedores externos de identidade», pelo que a autenticação tem de ser assegurada pela própria API. O JWT está previsto no Backlog de Projeto (`I033`, `I034`, `I051`). O RI não fixa esta tecnologia, pelo que esta decisão a documenta sem alterar nada do que o RI fixa. | Backlog de Projeto (`I033`, `I034`, `I051`); DA v02 (limites transversais); F001 |
| D-05 | Frontend web em React 19 + TypeScript + Vite. | Angular; Vue. | Ecossistema mais amplo, tipagem estática e build rápido; ferramentas de teste (Jest, Playwright) compatíveis. | RI v01, secção 11.2 |
| D-06 | Aplicação móvel em Flutter. | React Native; Android nativo (Kotlin). | Código único para Android e iOS; o Candidato acede apenas pela aplicação móvel (DA). | RI v01, secção 11.2; DA v02 |
| D-07 | GitHub Actions + SonarQube Server 2026.1 LTA para os quality gates. | GitLab CI; SonarCloud. | Integração direta com os repositórios GitHub e Quality Gate bloqueante em todos os componentes. | RI v01, secções 11.4 e 11.6 |
| D-08 | Mensagens (F010) e notificações (F011) entregues por uma ligação bidirecional persistente, com ASP.NET Core SignalR como tecnologia proposta, reutilizando a autenticação JWT. | Consulta periódica à API (polling); WebSockets diretos sem biblioteca; serviço externo de envio de notificações. | A DA exige a ligação bidirecional persistente, o que exclui a consulta periódica, e exclui os serviços comerciais de comunicação e de notificação, pelo que a solução tem de ser interna. O SignalR faz parte do ASP.NET Core, pelo que não acrescenta tecnologia ao backend. O token JWT tem de ser aceite também na ligação persistente. Proposta sujeita a confirmação em reunião formal, com decisão registada em ata (Regulamento da UC, secção 10.1), porque os clientes web e móvel precisam de uma biblioteca cliente. | DA v02 (F010, F011 e limites transversais) |

### 3.6. Pontos a acompanhar

| N.º | Ponto | Encaminhamento |
| --- | --- | --- |
| 1 | A receção de mensagens e notificações em tempo real (F010 e F011) exige uma ligação bidirecional persistente entre cliente e servidor, imposta pela DA. O RI não fixa nenhuma tecnologia para isso. | Proposta D-08 (ASP.NET Core SignalR). A DA determina que a comunicação em tempo real e as notificações são «asseguradas pelo próprio sistema», o que exclui serviços externos de envio de notificações. A decisão é confirmada em reunião formal e registada em ata (Regulamento da UC, secção 10.1). O tempo máximo de entrega fixado na especificação de requisitos (parâmetro P17) é verificado contra esta escolha quando esse documento for consolidado. |
| 2 | O RI não fixa versões para Jest, Playwright, Postman, Npgsql, `EFCore.NamingConventions` nem para o pacote JWT Bearer. | Versões fixadas nos ficheiros de dependências de cada repositório (secção 3.4). |
| 4 | O SQLite in-memory dos testes de integração não reproduz o `xmin` (concorrência otimista em `match`), os tipos enumerados nativos, nem as restrições `CHECK` com expressão regular do esquema v01. Estes aspetos não ficam cobertos pelos testes de integração. | Limitação aceite pelo RI (secções 5 e 11.2). Qualquer alternativa, como testar contra PostgreSQL, exige uma nova versão do RI. |
| 5 | O RI prevê o Quality Profile «Sonar way» para Dart no SonarQube Server Community Build. Está por confirmar que este perfil existe para essa edição; se não existir, o QG-05 do mobile não é cumprível como está definido. | Confirmar na instalação do SonarQube antes de `m3`. Uma eventual alteração exige uma nova versão do RI (secção 11.1). |