# Modelo de Dados — Base de dados `katch`

**Unidade curricular:** Laboratório de Desenvolvimento de Software — LDS
**Milestone:** `m3`
**Ficheiro:** `m3-modelo-de-dados-katch-v02.md`
**Pasta de arquivo:** `04-artefactos-tecnicos/04.06-modelos-de-dados`

---

## 1. Identificação do documento

| Campo | Informação |
| --- | --- |
| Grupo | G11 |
| Sistema | Katch |
| Ano letivo | 2026/2027 |
| Curso | LEI — Licenciatura em Engenharia Informática |
| Turma | LEI3T2 |
| Versão do documento | v02 |
| Base de dados | `katch` (PostgreSQL 18) |
| Issue | `I102` — Produzir o modelo de dados v02 (versão anterior: `I035`) |
| Executor / Revisor / Auditor | João Coelho / João Borguem / Roberto Baptista |
| Documentos de origem | `m3-proposta-sistema-v03.pdf`, `m3-declaracao-ambito-v03.pdf`, `m2-modelo-de-dados-katch-v01.md`, `m2-documentacao-api-v01.md` (PA-01 e PA-10), `m2-especificacao-requisitos-v01.md` (numeração atual, RF001 a RF118, RNF001 a RNF018, parâmetros P01 a P21), `m1-regulamento-grupo-v01.pdf` (secções 5, 11.2, 14.2 e 14.3), `m2-backlog-projeto-v02.xlsx` (Issue `I035`), `m2-documentacao-arquitetura-v01.md` (secção 2, decisão AD-01), `m2-diagrama-estados-interesse-match-v01.md` |

O documento segue a secção 19 do Regulamento de Funcionamento da Unidade Curricular para `04.06-modelos-de-dados`: um ficheiro por base de dados, com finalidade, tecnologia, modelos conceptual, lógico e físico e dicionário de dados. Corresponde à linha `OF-M3-023` — «Modelo de dados — katch v2» da Checklist de Controlo de Artefactos (v04), gerida em code-first com o Entity Framework Core.

### 1.1. Histórico de versões

| Versão | Data | Descrição das alterações | Issue |
| --- | --- | --- | --- |
| v01 | 2026-10-03 | Criação do documento: modelos conceptual, lógico e físico, dicionário de dados, índices, integridade, dados sensíveis, dados iniciais, decisões e limitações. | `I035` |
| v01 | 2026-10-06 | Correções da revisão da `I035` (ciclo 1): transição T7, ainda por ratificar, retirada das regras em vigor e passada a evolução prevista (D01); `ELSE false` em `ck_notification_target` (M01); concorrência da quota de interesses (M02); fim de sessão com credencial não persistida (M03). | `I035` |
| v02 | 2026-10-15 | Alinhamento com a redução de âmbito (PS e DA v03): retirada da funcionalidade F011 e, com ela, da tabela `notification`, do tipo `notification_type`, dos índices, das restrições e das regras associadas e da decisão DM-07. Relação Candidato–competência separada em `candidate_skill` (lista pré-definida) e `candidate_custom_skill` (opção «Outro»). Curriculum vitae acessível também ao próprio Candidato (PA-10). Vagas expiradas excluídas das vagas apresentadas (N2). PA-01 na opção (a): sem tabela de ficheiros. Simplificação: retiradas as colunas `status_changed_at` e `status_changed_by` (`app_user`, `company`), `operation_log.entity_type` e `match.last_message_at`, cuja informação consta do `operation_log` ou se calcula (DM-18 a DM-20). Segunda simplificação: retirados `created_at` e `updated_at` (exceto em `app_user`), `job.closed_at`, `job.close_reason`, `match.matched_at` e `match.profile_opened_by`; a `match` passa a ter um único `status` de quatro valores (DM-21 a DM-24). | `I102` |

Cada alteração posterior acrescenta uma linha. As versões anteriores são conservadas, nos termos da secção 18.2 do Regulamento de Funcionamento da Unidade Curricular.

---

## 2. Finalidade e tecnologia

| Campo | Informação |
| --- | --- |
| Finalidade | Guardar as contas dos três tipos de utilizador, o perfil profissional do Candidato, as Empresas e as suas vagas, as listas pré-definidas, as respostas dos Candidatos às vagas, as decisões dos Recrutadores, os matches e as conversas e o registo de autoria das operações relevantes (F001 a F010). |
| Sistema de gestão | PostgreSQL 18 (RI, secção 11.2). Em desenvolvimento corre em `localhost`, porta 6000 (RI, secção 5). |
| Administração | João Coelho, administrador da base de dados de produção (RI, secção 14.2). |
| Acesso | Apenas o backend acede à base de dados, através do Entity Framework Core (code-first, migrations) com o fornecedor Npgsql (RI, secção 11.2; arquitetura, secção 2). |
| Ficheiros | Fotografias, logótipos e curriculum vitae ficam no sistema de ficheiros do servidor do backend. A base de dados guarda apenas o caminho de cada ficheiro (arquitetura, decisão AD-01). |
| Testes | Os testes de integração usam SQLite in-memory (RI, secções 5 e 11.2). As limitações dessa opção estão na secção 11. |
| Dados | Apenas dados fictícios, criados pelos scripts de dados iniciais do backend; é proibida a cópia de dados reais (RI, secção 14.3; DA, secção 3). |
| Codificação e fuso | Base de dados em UTF-8. Todas as datas e horas são `timestamptz`, guardadas em UTC. |
| Convenções | Tabelas em inglês, no singular e em minúsculas; colunas em `snake_case`; tipos enumerados com valores em maiúsculas. Chaves primárias `uuid` com `gen_random_uuid()`, exceto nas tabelas de especialização da conta (`admin`, `candidate`, `recruiter`), cuja chave primária é também chave estrangeira para `app_user`, e no `operation_log`, que usa uma sequência. A tabela de contas chama-se `app_user` porque `user` é palavra reservada do PostgreSQL. |

---

## 3. Modelo conceptual

O modelo conceptual mostra as entidades do domínio, com os nomes usados na especificação de requisitos, e as relações entre elas, sem atributos.

```mermaid
erDiagram
    CONTA ||--o| CANDIDATO : "é"
    CONTA ||--o| RECRUTADOR : "é"
    CONTA ||--o| ADMINISTRADOR : "é"
    RECRUTADOR ||--o| EMPRESA : "representa"
    LOCALIDADE ||--o{ CANDIDATO : "localiza"
    LOCALIDADE ||--o{ EMPRESA : "localiza"
    LOCALIDADE ||--o{ VAGA : "localiza"
    EMPRESA ||--o{ VAGA : "publica"
    CANDIDATO ||--o{ HIPERLIGACAO : "indica"
    CANDIDATO }o--o{ COMPETENCIA : "possui (da lista)"
    CANDIDATO ||--o{ COMPETENCIA_OUTRO : "indica"
    VAGA }o--o{ COMPETENCIA : "pretende"
    VAGA }o--o{ BENEFICIO : "oferece"
    CANDIDATO ||--o{ RESPOSTA_A_VAGA : "dá"
    VAGA ||--o{ RESPOSTA_A_VAGA : "recebe"
    RECRUTADOR |o--o{ RESPOSTA_A_VAGA : "abre o perfil e decide"
    RESPOSTA_A_VAGA ||--o{ MENSAGEM : "contém (conversa do match)"
    CONTA ||--o{ MENSAGEM : "envia"
    CONTA |o--o{ REGISTO_DE_OPERACAO : "realiza"
```

| Entidade | Significado | Tabela |
| --- | --- | --- |
| Conta | Credenciais, tipo de conta e estado da conta de qualquer utilizador. | `app_user` |
| Candidato, Recrutador, Administrador | Especializações da conta, uma por tipo de conta. | `candidate`, `recruiter`, `admin` |
| Localidade | Lista pré-definida de localidades com coordenadas, carregada inicialmente e não editável na plataforma. | `location` |
| Empresa | Empresa representada por um único Recrutador, com estado de aprovação e página de apresentação. | `company` |
| Vaga | Oportunidade de trabalho de uma Empresa. | `job` |
| Hiperligação | Hiperligação profissional externa do Candidato (até 3). | `candidate_link` |
| Competência, Benefício | Listas pré-definidas mantidas pelo Administrador. | `skill`, `candidate_skill`, `job_skill`, `benefit`, `job_benefit` |
| Competência «Outro» | Competência indicada livremente pelo Candidato na opção «Outro», que não pertence à lista pré-definida. | `candidate_custom_skill` |
| Resposta a vaga | Recusa ou interesse do Candidato numa vaga, abertura do perfil e decisão do Recrutador, match e conversa associada. | `match` |
| Mensagem | Texto enviado na conversa de um match. | `message` |
| Registo de operação | Autoria e data e hora das operações relevantes (RF112). | `operation_log` |

A conversa não é uma entidade autónoma: existe exatamente uma por match, criada no instante da confirmação e associada ao match e à vaga de origem (RF068; DA, F010). Os seus atributos ficam na tabela `match` (decisão DM-05).

---

## 4. Modelo lógico

O modelo lógico mostra as tabelas, as colunas, as chaves e as cardinalidades. Os tipos são os do PostgreSQL. Os sufixos `_array` representam colunas do tipo array (por exemplo, `work_mode[]`), porque a sintaxe do Mermaid não aceita parênteses retos nos tipos.

```mermaid
erDiagram
    app_user ||--o| candidate : "id"
    app_user ||--o| recruiter : "id"
    app_user ||--o| admin : "id"
    recruiter ||--o| company : "recruiter_id"
    location ||--o{ candidate : "location_id"
    location ||--o{ company : "location_id"
    location ||--o{ job : "location_id"
    company ||--o{ job : "company_id"
    candidate ||--o{ candidate_link : "candidate_id"
    candidate ||--o{ candidate_skill : "candidate_id"
    skill ||--o{ candidate_skill : "skill_id"
    candidate ||--o{ candidate_custom_skill : "candidate_id"
    job ||--o{ job_skill : "job_id"
    skill ||--o{ job_skill : "skill_id"
    job ||--o{ job_benefit : "job_id"
    benefit ||--o{ job_benefit : "benefit_id"
    candidate ||--o{ match : "candidate_id"
    job ||--o{ match : "job_id"
    recruiter |o--o{ match : "decided_by"
    app_user |o--o{ match : "conversation_closed_by"
    match ||--o{ message : "match_id"
    app_user ||--o{ message : "sender_id"
    app_user |o--o{ operation_log : "user_id"

    app_user {
        uuid id PK
        varchar email UK "único sem distinção de maiúsculas"
        varchar password_hash "resumo irreversível"
        user_type user_type
        account_status status
        timestamptz terms_accepted_at
        timestamptz created_at
        timestamptz updated_at
    }
    admin {
        uuid id PK, FK
    }
    recruiter {
        uuid id PK, FK
    }
    candidate {
        uuid id PK, FK
        varchar full_name
        varchar phone_number
        uuid location_id FK
        varchar desired_role
        varchar experience_summary
        availability availability
        varchar profile_photo_path
        varchar cv_path
        integer max_distance_km
        numeric min_salary_expectation
        work_mode_array preferred_work_modes
        contract_type_array preferred_contract_types
        smallint swipe_rights_remaining
        timestamptz next_swipe_available_at
    }
    location {
        uuid id PK
        varchar name
        varchar district
        numeric latitude
        numeric longitude
    }
    candidate_link {
        uuid id PK
        uuid candidate_id FK
        smallint position
        varchar url
    }
    skill {
        uuid id PK
        varchar name UK
    }
    candidate_skill {
        uuid candidate_id PK, FK
        uuid skill_id PK, FK
    }
    candidate_custom_skill {
        uuid id PK
        uuid candidate_id FK
        varchar label
    }
    benefit {
        uuid id PK
        varchar name UK
    }
    company {
        uuid id PK
        uuid recruiter_id FK, UK
        varchar company_name
        varchar tax_id UK
        industry industry
        varchar address
        uuid location_id FK
        varchar contact_email
        varchar contact_phone
        varchar responsible_name
        varchar website
        varchar description
        varchar logo_path
        varchar_array photo_paths
        company_status status
        varchar status_reason
        timestamptz submitted_at
    }
    job {
        uuid id PK
        uuid company_id FK
        varchar title
        text description
        numeric min_salary
        numeric max_salary
        uuid location_id FK
        contract_type contract_type
        work_mode work_mode
        boolean is_urgent
        varchar_array photo_paths
        timestamptz expires_at
        job_status status
        timestamptz first_published_at
        timestamptz published_at
        timestamptz suspended_at
        timestamptz deleted_at
    }
    job_skill {
        uuid job_id PK, FK
        uuid skill_id PK, FK
    }
    job_benefit {
        uuid job_id PK, FK
        uuid benefit_id PK, FK
    }
    match {
        uuid id PK
        uuid candidate_id FK
        uuid job_id FK
        match_status status
        timestamptz candidate_action_at
        timestamptz profile_opened_at
        timestamptz recruiter_action_at
        uuid decided_by FK
        conversation_status conversation_status
        timestamptz conversation_closed_at
        uuid conversation_closed_by FK
        conversation_close_reason conversation_close_reason
    }
    message {
        uuid id PK
        uuid match_id FK
        uuid sender_id FK
        varchar content
        timestamptz sent_at
        timestamptz read_at
    }
    operation_log {
        bigint id PK
        uuid user_id FK
        operation_type operation
        uuid entity_id
        timestamptz occurred_at
    }
```

### 4.1. Cardinalidades

| Relação | Cardinalidade | Regra |
| --- | --- | --- |
| `app_user` — `candidate` / `recruiter` / `admin` | 1 : 0..1 | Cada conta tem exatamente uma especialização, a do seu `user_type` (validado no backend). |
| `recruiter` — `company` | 1 : 0..1 | Cada Recrutador representa no máximo uma Empresa e cada Empresa tem exatamente um Recrutador (`company.recruiter_id` obrigatório e único). A conta de Recrutador existe antes do registo da Empresa (RF037, RF041). |
| `company` — `job` | 1 : 0..N | As vagas pertencem à Empresa. O Recrutador responsável pela vaga é o Recrutador da Empresa (especificação de requisitos, secção 2.2). |
| `location` — `candidate` / `company` / `job` | 1 : 0..N | Localidade obrigatória nas três tabelas. |
| `candidate` — `candidate_link` | 1 : 0..3 | Máximo de 3 hiperligações (P10), garantido pela coluna `position` (1 a 3) única por Candidato. |
| `candidate` — `skill` | N : M | Através de `candidate_skill`, com chave primária composta (`candidate_id`, `skill_id`): cada competência da lista aparece no máximo uma vez por Candidato. |
| `candidate` — `candidate_custom_skill` | 1 : 0..N | Competências da opção «Outro», fora da lista pré-definida e pertencentes a um único Candidato. A designação é única por Candidato, sem distinção de maiúsculas. |
| `job` — `skill` | N : M (1..N por vaga) | Através de `job_skill`. Cada vaga tem pelo menos uma competência pretendida (RF050, validado no backend). |
| `job` — `benefit` | N : M (0..N por vaga) | Através de `job_benefit`. |
| `candidate` — `job` (resposta) | N : M | Através de `match`, com no máximo uma resposta por par Candidato–vaga (RF019). |
| `recruiter` — `match` (`decided_by`) | 0..1 : 0..N | Preenchida na decisão. O Recrutador que abre o perfil é o da Empresa da vaga (DM-03). |
| `match` — `message` | 1 : 0..N | Só existem mensagens em matches confirmados (validado no backend, RF070). |
| `app_user` — `message` | 1 : 0..N | Remetente da mensagem. |
| `app_user` — `operation_log` | 0..1 : 0..N | O autor é obrigatório, exceto no encerramento automático de vaga (RF057), feito pelo sistema. |

---

## 5. Modelo físico

Script de criação para PostgreSQL 18. Em `m3`, o esquema é criado pelas migrations do Entity Framework Core (`OF-M3-003`), que têm de produzir o mesmo resultado.

### 5.1. Tipos enumerados

```sql
CREATE TYPE user_type                 AS ENUM ('CANDIDATE', 'RECRUITER', 'ADMIN');
CREATE TYPE account_status            AS ENUM ('ACTIVE', 'BLOCKED', 'SUSPENDED');
CREATE TYPE work_mode                 AS ENUM ('ON_SITE', 'HYBRID', 'REMOTE');
CREATE TYPE contract_type             AS ENUM ('PERMANENT', 'FIXED_TERM', 'INTERNSHIP', 'SERVICE_PROVISION');
CREATE TYPE availability              AS ENUM ('IMMEDIATE', 'FIFTEEN_DAYS', 'ONE_MONTH', 'TO_BE_AGREED');
CREATE TYPE industry                  AS ENUM ('TECHNOLOGY', 'HEALTHCARE', 'RETAIL', 'HOSPITALITY', 'CONSTRUCTION',
                                               'MANUFACTURING', 'EDUCATION', 'FINANCE', 'LOGISTICS', 'SERVICES', 'OTHER');
CREATE TYPE company_status            AS ENUM ('PENDING', 'APPROVED', 'REJECTED', 'SUSPENDED');
CREATE TYPE job_status                AS ENUM ('UNPUBLISHED', 'PUBLISHED', 'SUSPENDED', 'CLOSED');
CREATE TYPE match_status              AS ENUM ('DECLINED_BY_CANDIDATE', 'WAITING', 'MATCHED', 'REJECTED_BY_RECRUITER');
CREATE TYPE conversation_status       AS ENUM ('OPEN', 'CLOSED');
CREATE TYPE conversation_close_reason AS ENUM ('CLOSED_BY_PARTY', 'ACCOUNT_BLOCKED_OR_SUSPENDED', 'COMPANY_SUSPENDED');
CREATE TYPE operation_type            AS ENUM ('COMPANY_SUBMITTED', 'COMPANY_UPDATED', 'COMPANY_APPROVED', 'COMPANY_REJECTED',
                                               'COMPANY_SUSPENDED', 'COMPANY_REACTIVATED',
                                               'JOB_CREATED', 'JOB_PUBLISHED', 'JOB_UPDATED', 'JOB_SUSPENDED',
                                               'JOB_REPUBLISHED', 'JOB_CLOSED', 'JOB_DELETED',
                                               'ACCOUNT_BLOCKED', 'ACCOUNT_REACTIVATED', 'ACCOUNT_SUSPENDED',
                                               'SKILL_CREATED', 'SKILL_RENAMED', 'BENEFIT_CREATED', 'BENEFIT_RENAMED');
```

Correspondência com os valores da especificação de requisitos:

| Tipo | Valores na especificação de requisitos |
| --- | --- |
| `account_status` | ativa, bloqueada, suspensa (secção 2.2; o estado «suspensa» corresponde à desativação referida na DA, F001) |
| `work_mode` | presencial, híbrido, remoto (P16) |
| `contract_type` | sem termo, a termo, estágio, prestação de serviços (P09) |
| `availability` | imediata, 15 dias, 1 mês, a combinar (P14) |
| `industry` | tecnologia, saúde, comércio, hotelaria e restauração, construção, indústria, educação, finanças, logística, serviços, outro (P15) |
| `company_status` | pendente, aprovada, recusada, suspensa (secção 2.2) |
| `job_status` | não publicada, publicada, suspensa, encerrada (secção 2.2) |
| `match_status` | `DECLINED_BY_CANDIDATE` = recusa da vaga pelo Candidato; `WAITING` = Candidato em espera; `MATCHED` = match confirmado; `REJECTED_BY_RECRUITER` = recusa do Candidato pelo Recrutador |
| `operation_type` | As operações enumeradas no RF112, exceto o interesse e a recusa de vaga, registados na tabela `match` |

### 5.2. Tabelas

```sql
CREATE TABLE location (
    id         uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    name       varchar(100) NOT NULL CHECK (btrim(name) <> ''),
    district   varchar(50)  NOT NULL CHECK (btrim(district) <> ''),
    latitude   numeric(9,6) NOT NULL CHECK (latitude  BETWEEN -90  AND 90),
    longitude  numeric(9,6) NOT NULL CHECK (longitude BETWEEN -180 AND 180),
    CONSTRAINT uq_location_name_district UNIQUE (name, district)
);

CREATE TABLE app_user (
    id                 uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    email              varchar(255) NOT NULL CHECK (email ~ '^[^@[:space:]]+@[^@[:space:]]+$'),
    password_hash      varchar(255) NOT NULL,
    user_type          user_type NOT NULL,
    status             account_status NOT NULL DEFAULT 'ACTIVE',
    terms_accepted_at  timestamptz,
    created_at         timestamptz NOT NULL DEFAULT now(),
    updated_at         timestamptz NOT NULL DEFAULT now(),
    CONSTRAINT ck_app_user_terms         CHECK (user_type = 'ADMIN' OR terms_accepted_at IS NOT NULL),
    CONSTRAINT ck_app_user_suspended     CHECK (status <> 'SUSPENDED' OR user_type = 'CANDIDATE'),
    CONSTRAINT ck_app_user_admin_active  CHECK (user_type <> 'ADMIN' OR status = 'ACTIVE')
);

CREATE TABLE admin (
    id          uuid PRIMARY KEY REFERENCES app_user (id)
);

CREATE TABLE recruiter (
    id          uuid PRIMARY KEY REFERENCES app_user (id)
);

CREATE TABLE candidate (
    id                        uuid PRIMARY KEY REFERENCES app_user (id),
    full_name                 varchar(150) NOT NULL CHECK (btrim(full_name) <> ''),
    phone_number              varchar(9)   NOT NULL CHECK (phone_number ~ '^[0-9]{9}$'),
    location_id               uuid NOT NULL REFERENCES location (id),
    desired_role              varchar(150),
    experience_summary        varchar(1000),
    availability              availability,
    profile_photo_path        varchar(500),
    cv_path                   varchar(500),
    max_distance_km           integer CHECK (max_distance_km BETWEEN 1 AND 500),
    min_salary_expectation    numeric(10,2) CHECK (min_salary_expectation >= 0),
    preferred_work_modes      work_mode[]     NOT NULL DEFAULT '{}',
    preferred_contract_types  contract_type[] NOT NULL DEFAULT '{}',
    swipe_rights_remaining    smallint NOT NULL DEFAULT 10 CHECK (swipe_rights_remaining BETWEEN 0 AND 10),
    next_swipe_available_at   timestamptz,
    CONSTRAINT ck_candidate_quota_block CHECK (next_swipe_available_at IS NULL OR swipe_rights_remaining = 0)
);

CREATE TABLE candidate_link (
    id            uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    candidate_id  uuid NOT NULL REFERENCES candidate (id) ON DELETE CASCADE,
    position      smallint NOT NULL CHECK (position BETWEEN 1 AND 3),
    url           varchar(500) NOT NULL CHECK (url ~* '^https?://'),
    CONSTRAINT uq_candidate_link_position UNIQUE (candidate_id, position),
    CONSTRAINT uq_candidate_link_url      UNIQUE (candidate_id, url)
);

CREATE TABLE skill (
    id          uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    name        varchar(100) NOT NULL CHECK (btrim(name) <> '')
);

CREATE TABLE benefit (
    id          uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    name        varchar(100) NOT NULL CHECK (btrim(name) <> '')
);

CREATE TABLE candidate_skill (
    candidate_id  uuid NOT NULL REFERENCES candidate (id) ON DELETE CASCADE,
    skill_id      uuid NOT NULL REFERENCES skill (id),
    PRIMARY KEY (candidate_id, skill_id)
);

CREATE TABLE candidate_custom_skill (
    id            uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    candidate_id  uuid NOT NULL REFERENCES candidate (id) ON DELETE CASCADE,
    label         varchar(40) NOT NULL CHECK (char_length(label) BETWEEN 2 AND 40
                                              AND btrim(label) <> ''
                                              AND label ~ '^[[:alnum:] +#.-]+$')
);

CREATE TABLE company (
    id                 uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    recruiter_id       uuid NOT NULL REFERENCES recruiter (id),
    company_name       varchar(200) NOT NULL CHECK (btrim(company_name) <> ''),
    tax_id             varchar(9)   NOT NULL CHECK (tax_id ~ '^[0-9]{9}$'),
    industry           industry NOT NULL,
    address            varchar(255) NOT NULL CHECK (btrim(address) <> ''),
    location_id        uuid NOT NULL REFERENCES location (id),
    contact_email      varchar(255) NOT NULL CHECK (contact_email ~ '^[^@[:space:]]+@[^@[:space:]]+$'),
    contact_phone      varchar(9)   NOT NULL CHECK (contact_phone ~ '^[0-9]{9}$'),
    responsible_name   varchar(150) NOT NULL CHECK (btrim(responsible_name) <> ''),
    website            varchar(500) CHECK (website ~* '^https?://'),
    description        varchar(1000),
    logo_path          varchar(500),
    photo_paths        varchar(500)[] NOT NULL DEFAULT '{}' CHECK (cardinality(photo_paths) <= 6),
    status             company_status NOT NULL DEFAULT 'PENDING',
    status_reason      varchar(500),
    submitted_at       timestamptz NOT NULL DEFAULT now(),
    CONSTRAINT uq_company_recruiter      UNIQUE (recruiter_id),
    CONSTRAINT uq_company_tax_id         UNIQUE (tax_id),
    CONSTRAINT ck_company_reason         CHECK ((status = 'REJECTED') = (status_reason IS NOT NULL)),
    CONSTRAINT ck_company_reason_length  CHECK (status_reason IS NULL OR char_length(btrim(status_reason)) BETWEEN 10 AND 500)
);

CREATE TABLE job (
    id                  uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    company_id          uuid NOT NULL REFERENCES company (id),
    title               varchar(150) NOT NULL CHECK (btrim(title) <> ''),
    description         text NOT NULL CHECK (btrim(description) <> ''),
    min_salary          numeric(10,2) NOT NULL CHECK (min_salary > 0),
    max_salary          numeric(10,2) NOT NULL,
    location_id         uuid NOT NULL REFERENCES location (id),
    contract_type       contract_type NOT NULL,
    work_mode           work_mode NOT NULL,
    is_urgent           boolean NOT NULL DEFAULT false,
    photo_paths         varchar(500)[] NOT NULL DEFAULT '{}' CHECK (cardinality(photo_paths) <= 5),
    expires_at          timestamptz,
    status              job_status NOT NULL DEFAULT 'UNPUBLISHED',
    first_published_at  timestamptz,
    published_at        timestamptz,
    suspended_at        timestamptz,
    deleted_at          timestamptz,
    CONSTRAINT ck_job_salary     CHECK (max_salary >= min_salary),
    CONSTRAINT ck_job_published  CHECK (status NOT IN ('PUBLISHED', 'SUSPENDED') OR first_published_at IS NOT NULL)
);

CREATE TABLE job_skill (
    job_id    uuid NOT NULL REFERENCES job (id) ON DELETE CASCADE,
    skill_id  uuid NOT NULL REFERENCES skill (id),
    PRIMARY KEY (job_id, skill_id)
);

CREATE TABLE job_benefit (
    job_id      uuid NOT NULL REFERENCES job (id) ON DELETE CASCADE,
    benefit_id  uuid NOT NULL REFERENCES benefit (id),
    PRIMARY KEY (job_id, benefit_id)
);

CREATE TABLE match (
    id                         uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    candidate_id               uuid NOT NULL REFERENCES candidate (id),
    job_id                     uuid NOT NULL REFERENCES job (id),
    status                     match_status NOT NULL,
    candidate_action_at        timestamptz NOT NULL DEFAULT now(),
    profile_opened_at          timestamptz,
    recruiter_action_at        timestamptz,
    decided_by                 uuid REFERENCES recruiter (id),
    conversation_status        conversation_status,
    conversation_closed_at     timestamptz,
    conversation_closed_by     uuid REFERENCES app_user (id),
    conversation_close_reason  conversation_close_reason,
    CONSTRAINT uq_match_candidate_job      UNIQUE (candidate_id, job_id),
    CONSTRAINT ck_match_declined           CHECK (status <> 'DECLINED_BY_CANDIDATE'
                                                  OR (profile_opened_at IS NULL AND recruiter_action_at IS NULL)),
    CONSTRAINT ck_match_decision           CHECK ((status IN ('MATCHED', 'REJECTED_BY_RECRUITER')) = (recruiter_action_at IS NOT NULL)
                                                  AND (recruiter_action_at IS NOT NULL) = (decided_by IS NOT NULL)
                                                  AND (recruiter_action_at IS NULL OR profile_opened_at IS NOT NULL)),
    CONSTRAINT ck_match_conversation       CHECK ((conversation_status IS NOT NULL) = (status = 'MATCHED')),
    CONSTRAINT ck_match_conversation_close CHECK (coalesce(conversation_status = 'CLOSED', false) = (conversation_closed_at IS NOT NULL)
                                                  AND coalesce(conversation_status = 'CLOSED', false) = (conversation_close_reason IS NOT NULL)),
    CONSTRAINT ck_match_closed_by          CHECK ((conversation_closed_by IS NOT NULL)
                                                  = coalesce(conversation_close_reason = 'CLOSED_BY_PARTY', false))
);

CREATE TABLE message (
    id         uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    match_id   uuid NOT NULL REFERENCES match (id),
    sender_id  uuid NOT NULL REFERENCES app_user (id),
    content    varchar(1000) NOT NULL CHECK (btrim(content) <> ''),
    sent_at    timestamptz NOT NULL DEFAULT now(),
    read_at    timestamptz,
    CONSTRAINT ck_message_read CHECK (read_at IS NULL OR read_at >= sent_at)
);

CREATE TABLE operation_log (
    id           bigint GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    user_id      uuid REFERENCES app_user (id),
    operation    operation_type NOT NULL,
    entity_id    uuid NOT NULL,
    occurred_at  timestamptz NOT NULL DEFAULT now(),
    CONSTRAINT ck_operation_log_author CHECK (user_id IS NOT NULL OR operation = 'JOB_CLOSED')
);
```

### 5.3. Índices

```sql
CREATE UNIQUE INDEX uq_app_user_email          ON app_user (lower(email));
CREATE UNIQUE INDEX uq_skill_name              ON skill (lower(name));
CREATE UNIQUE INDEX uq_benefit_name            ON benefit (lower(name));
CREATE UNIQUE INDEX uq_candidate_custom_skill  ON candidate_custom_skill (candidate_id, lower(label));

CREATE INDEX ix_candidate_location             ON candidate (location_id);
CREATE INDEX ix_company_status                 ON company (status);
CREATE INDEX ix_job_company_status             ON job (company_id, status) WHERE deleted_at IS NULL;
CREATE INDEX ix_job_location                   ON job (location_id) WHERE deleted_at IS NULL;
CREATE INDEX ix_job_expiry                     ON job (expires_at) WHERE status = 'PUBLISHED';
CREATE INDEX ix_match_job_status               ON match (job_id, status);
CREATE INDEX ix_match_matched                  ON match (status, recruiter_action_at);
CREATE INDEX ix_match_interest_at              ON match (candidate_action_at) WHERE status <> 'DECLINED_BY_CANDIDATE';
CREATE INDEX ix_message_match_sent             ON message (match_id, sent_at);
CREATE INDEX ix_message_unread                 ON message (match_id) WHERE read_at IS NULL;
CREATE INDEX ix_operation_log_entity           ON operation_log (entity_id, occurred_at);
CREATE INDEX ix_operation_log_occurred         ON operation_log (operation, occurred_at);
```

| Índice | Finalidade | Requisitos |
| --- | --- | --- |
| `uq_app_user_email` | Endereço de correio eletrónico único no sistema, sem distinção de maiúsculas. | RF004, RF037 |
| `uq_skill_name`, `uq_benefit_name` | Rejeitar designações já existentes nas listas. | RF099 a RF102 |
| `candidate_skill` (chave primária), `uq_candidate_custom_skill` | A mesma competência não é associada duas vezes ao mesmo Candidato, nem repetida na opção «Outro». | RF008, RF009 |
| `uq_company_tax_id` (restrição) | Número de identificação fiscal único. | RF042 |
| `uq_match_candidate_job` (restrição) | Uma única resposta por par Candidato–vaga; a sua primeira coluna serve também a exclusão das vagas já respondidas. | RF014, RF019 |
| `ix_company_status` | Lista das Empresas pendentes e listas por estado. | RF091, RF097 |
| `ix_job_company_status`, `ix_job_location` | Seleção das vagas publicadas, filtro por distância e número de vagas publicadas por Empresa. | RF014, RF097 |
| `ix_job_expiry` | Encerramento automático na data-limite e exclusão das vagas expiradas das vagas apresentadas ao Candidato. | RF014, RF057 |
| `ix_match_job_status` | Candidatos em espera por vaga e número de interesses por vaga. | RF060, RF098 |
| `ix_match_matched`, `ix_match_interest_at` | Indicadores do período. | RF096 |
| `ix_message_match_sent`, `ix_message_unread` | Histórico, obtenção das mensagens novas, mensagens por ler e última mensagem de cada conversa. | RF027 a RF029, RF071 a RF073 |
| `ix_operation_log_*` | Reconstituir o estado das contas e das Empresas numa data passada. | RF096, RF112 |

---

## 6. Dicionário de dados

Legenda: **Nulo** — «Não» significa obrigatório. **Origem** — requisito, parâmetro ou secção da especificação de requisitos que justifica a coluna ou a regra. Os dados sensíveis estão identificados na secção 8. Os comprimentos máximos que a especificação de requisitos não fixa são limites técnicos (decisão DM-12).

### 6.1. `app_user` — conta de utilizador

| Coluna | Tipo | Nulo | Predefinido | Regras e domínio | Origem |
| --- | --- | --- | --- | --- | --- |
| `id` | `uuid` | Não | `gen_random_uuid()` | Chave primária. | — |
| `email` | `varchar(255)` | Não | — | Formato local@domínio; único no sistema, sem distinção de maiúsculas. Usado no início de sessão. | RF001, RF003, RF004, RF037, P04 |
| `password_hash` | `varchar(255)` | Não | — | Resumo criptográfico irreversível com valor aleatório próprio da conta. A palavra-passe nunca é guardada. A regra P05 é validada antes do cálculo do resumo. | RF002, RF040, RF084, RNF003, P05 |
| `user_type` | `user_type` | Não | — | Candidato, Recrutador ou Administrador. Determina o ponto de acesso e as operações autorizadas. | RF085, RF104, RNF006 |
| `status` | `account_status` | Não | `ACTIVE` | `SUSPENDED` só em contas de Candidato; contas de Administrador sempre `ACTIVE`. | RF005, RF085 a RF090 |
| `terms_accepted_at` | `timestamptz` | Sim | — | Aceitação das condições de utilização; obrigatória em contas de Candidato e de Recrutador. | RF003, RF037; DA, F001 |
| `created_at` | `timestamptz` | Não | `now()` | Data de registo apresentada ao Administrador. | RF088, RF096 |
| `updated_at` | `timestamptz` | Não | `now()` | Atualizada pelo backend em cada alteração. | — |

### 6.2. `admin` — Administrador

| Coluna | Tipo | Nulo | Predefinido | Regras e domínio | Origem |
| --- | --- | --- | --- | --- | --- |
| `id` | `uuid` | Não | — | Chave primária e estrangeira para `app_user`, com `user_type = 'ADMIN'`. As contas de Administrador são criadas pelos dados iniciais; não há registo de Administrador na plataforma. | RF083; DA, F001 |

### 6.3. `recruiter` — Recrutador

| Coluna | Tipo | Nulo | Predefinido | Regras e domínio | Origem |
| --- | --- | --- | --- | --- | --- |
| `id` | `uuid` | Não | — | Chave primária e estrangeira para `app_user`, com `user_type = 'RECRUITER'`. Criada na criação de conta, antes do registo da Empresa. | RF037 |

### 6.4. `candidate` — Candidato e perfil profissional

| Coluna | Tipo | Nulo | Predefinido | Regras e domínio | Origem |
| --- | --- | --- | --- | --- | --- |
| `id` | `uuid` | Não | — | Chave primária e estrangeira para `app_user`, com `user_type = 'CANDIDATE'`. | RF003 |
| `full_name` | `varchar(150)` | Não | — | Nome do Candidato, não vazio. | RF003, RF088 |
| `phone_number` | `varchar(9)` | Não | — | Exatamente 9 algarismos. | RF003, RF004, P04 |
| `location_id` | `uuid` | Não | — | Chave estrangeira para `location`. Localidade do Candidato, usada no cálculo da distância. | RF003, RF006, RF015 |
| `desired_role` | `varchar(150)` | Sim | — | Função pretendida. | RF006 |
| `experience_summary` | `varchar(1000)` | Sim | — | Resumo da experiência, máximo de 1000 caracteres. | RF006, P08 |
| `availability` | `availability` | Sim | — | Imediata, 15 dias, 1 mês ou a combinar. | RF006, P14 |
| `profile_photo_path` | `varchar(500)` | Sim | — | Caminho da fotografia de perfil opcional (JPEG ou PNG, até 5 MB, validados no backend). | RF010, P01, RNF016 |
| `cv_path` | `varchar(500)` | Sim | — | Caminho do curriculum vitae opcional (PDF, até 5 MB). Acesso restrito ao próprio Candidato e ao Recrutador de uma vaga em que o Candidato está em espera. | RF011, RF061, P02, RNF007 |
| `max_distance_km` | `integer` | Sim | — | Distância máxima, 1 a 500 km, número inteiro. `NULL` enquanto o Candidato não a definir; nesse caso o filtro de distância não é aplicado (DM-11). | RF007, RF014, P06 |
| `min_salary_expectation` | `numeric(10,2)` | Sim | — | Pretensão salarial mínima em euros brutos mensais, maior ou igual a 0. `NULL` = sem filtro salarial. | RF007, RF014 |
| `preferred_work_modes` | `work_mode[]` | Não | `'{}'` | Regimes de trabalho pretendidos. Lista vazia = todos os regimes (DM-11). | RF007, RF014, P16 |
| `preferred_contract_types` | `contract_type[]` | Não | `'{}'` | Tipos de contrato pretendidos. Lista vazia = todos os tipos (DM-11). | RF007, RF014, P09 |
| `swipe_rights_remaining` | `smallint` | Não | `10` | Interesses disponíveis na quota, de 0 a 10. | RF020 a RF022, P11 |
| `next_swipe_available_at` | `timestamptz` | Sim | — | Fim do período de bloqueio: 24 horas depois do interesse que esgotou a quota. Só preenchido com a quota a 0. | RF020, RF021, P11 |

### 6.5. `location` — lista pré-definida de localidades

| Coluna | Tipo | Nulo | Predefinido | Regras e domínio | Origem |
| --- | --- | --- | --- | --- | --- |
| `id` | `uuid` | Não | `gen_random_uuid()` | Chave primária. | — |
| `name` | `varchar(100)` | Não | — | Nome da localidade. Único em conjunto com `district`. | RF003, RF041, RF050 |
| `district` | `varchar(50)` | Não | — | Distrito; distingue localidades com o mesmo nome. | — |
| `latitude` | `numeric(9,6)` | Não | — | −90 a 90. | RF015; DA, F002 |
| `longitude` | `numeric(9,6)` | Não | — | −180 a 180. | RF015; DA, F002 |

Carregada pelos dados iniciais e não editável na plataforma (DA, F002). Não tem operações de escrita na interface do servidor.

### 6.6. `candidate_link` — hiperligações profissionais

| Coluna | Tipo | Nulo | Predefinido | Regras e domínio | Origem |
| --- | --- | --- | --- | --- | --- |
| `id` | `uuid` | Não | `gen_random_uuid()` | Chave primária. | — |
| `candidate_id` | `uuid` | Não | — | Chave estrangeira para `candidate`. | RF012 |
| `position` | `smallint` | Não | — | 1 a 3, única por Candidato. Limita o número de hiperligações e fixa a ordem de apresentação. | RF012, P10 |
| `url` | `varchar(500)` | Não | — | Iniciado por `http://` ou `https://`; guardado tal como indicado, sem importar conteúdo da página. | RF012, P10; DA, F004 |

### 6.7. `skill` — lista pré-definida de competências

| Coluna | Tipo | Nulo | Predefinido | Regras e domínio | Origem |
| --- | --- | --- | --- | --- | --- |
| `id` | `uuid` | Não | `gen_random_uuid()` | Chave primária. | — |
| `name` | `varchar(100)` | Não | — | Designação não vazia, única sem distinção de maiúsculas. | RF099, RF100 |

### 6.8. `candidate_skill` e `candidate_custom_skill` — competências do Candidato

`candidate_skill` — competências da lista pré-definida:

| Coluna | Tipo | Nulo | Predefinido | Regras e domínio | Origem |
| --- | --- | --- | --- | --- | --- |
| `candidate_id` | `uuid` | Não | — | Parte da chave primária; chave estrangeira para `candidate`. | RF008 |
| `skill_id` | `uuid` | Não | — | Parte da chave primária; chave estrangeira para `skill`. Competência da lista. | RF008 |

`candidate_custom_skill` — competências da opção «Outro»:

| Coluna | Tipo | Nulo | Predefinido | Regras e domínio | Origem |
| --- | --- | --- | --- | --- | --- |
| `id` | `uuid` | Não | `gen_random_uuid()` | Chave primária (permite várias competências «Outro»). | — |
| `candidate_id` | `uuid` | Não | — | Chave estrangeira para `candidate`. | RF009 |
| `label` | `varchar(40)` | Não | — | Designação: 2 a 40 caracteres; apenas letras, algarismos, espaços e `+ # . -`; não só espaços. Única por Candidato, sem distinção de maiúsculas. | RF009, P07 |

As competências «Outro» não entram no destaque de competências coincidentes, porque as vagas só pretendem competências da lista (RF050, RF062).

### 6.9. `benefit` — lista pré-definida de benefícios

| Coluna | Tipo | Nulo | Predefinido | Regras e domínio | Origem |
| --- | --- | --- | --- | --- | --- |
| `id` | `uuid` | Não | `gen_random_uuid()` | Chave primária. | — |
| `name` | `varchar(100)` | Não | — | Designação não vazia, única sem distinção de maiúsculas. | RF101, RF102 |

### 6.10. `company` — Empresa

| Coluna | Tipo | Nulo | Predefinido | Regras e domínio | Origem |
| --- | --- | --- | --- | --- | --- |
| `id` | `uuid` | Não | `gen_random_uuid()` | Chave primária. | — |
| `recruiter_id` | `uuid` | Não | — | Chave estrangeira para `recruiter`, única. Recrutador que representa a Empresa. | RF041; especificação, secção 2.2 |
| `company_name` | `varchar(200)` | Não | — | Designação social. | RF041, RF092 |
| `tax_id` | `varchar(9)` | Não | — | Número de identificação fiscal: 9 algarismos, único. Não alterável depois da aprovação (backend). | RF041, RF042, RF049, P04 |
| `industry` | `industry` | Não | — | Setor de atividade. | RF041, P15 |
| `address` | `varchar(255)` | Não | — | Morada. | RF041 |
| `location_id` | `uuid` | Não | — | Chave estrangeira para `location`. Proposta por omissão nas vagas. | RF041, RF050 |
| `contact_email` | `varchar(255)` | Não | — | Formato local@domínio. Visível ao Candidato só depois do match. | RF041, RF025, P04 |
| `contact_phone` | `varchar(9)` | Não | — | 9 algarismos. Visível ao Candidato só depois do match. | RF041, RF025, P04 |
| `responsible_name` | `varchar(150)` | Não | — | Nome do responsável da Empresa. | RF041, RF092 |
| `website` | `varchar(500)` | Sim | — | Um único endereço do sítio na Internet, iniciado por `http://` ou `https://`. | RF046, RF017, P10 |
| `description` | `varchar(1000)` | Sim | — | Descrição da atividade, máximo de 1000 caracteres. | RF046, RF017, P08 |
| `logo_path` | `varchar(500)` | Sim | — | Caminho do logótipo (JPEG ou PNG, até 2 MB). | RF047, RF013, P01 |
| `photo_paths` | `varchar(500)[]` | Não | `'{}'` | Galeria: no máximo 6 caminhos; a ordem da lista é a ordem de apresentação. | RF048, RF017, P13 |
| `status` | `company_status` | Não | `PENDING` | Pendente, aprovada, recusada ou suspensa. | RF043, RF044, RF093 a RF095, RF116 |
| `status_reason` | `varchar(500)` | Sim | — | Motivo da recusa do registo: 10 a 500 caracteres; preenchido apenas no estado `REJECTED` e limpo na nova submissão. | RF044, RF094, P12 |
| `submitted_at` | `timestamptz` | Não | `now()` | Data de submissão do pedido; atualizada em cada nova submissão. | RF045, RF091 |

### 6.11. `job` — vaga

| Coluna | Tipo | Nulo | Predefinido | Regras e domínio | Origem |
| --- | --- | --- | --- | --- | --- |
| `id` | `uuid` | Não | `gen_random_uuid()` | Chave primária. | — |
| `company_id` | `uuid` | Não | — | Chave estrangeira para `company`. | RF050, RF059 |
| `title` | `varchar(150)` | Não | — | Função. | RF050, RF013 |
| `description` | `text` | Não | — | Descrição da oportunidade, não vazia. | RF050, RF016 |
| `min_salary` | `numeric(10,2)` | Não | — | Valor mínimo do intervalo salarial em euros, número positivo. | RF050, RF051 |
| `max_salary` | `numeric(10,2)` | Não | — | Valor máximo em euros, maior ou igual a `min_salary`. | RF050, RF051 |
| `location_id` | `uuid` | Não | — | Chave estrangeira para `location`; proposta por omissão igual à da Empresa (backend). | RF050, RF015 |
| `contract_type` | `contract_type` | Não | — | Tipo de contrato. | RF050, P09 |
| `work_mode` | `work_mode` | Não | — | Regime de trabalho. As vagas `REMOTE` ficam fora do filtro de distância. | RF050, RF014, P16 |
| `is_urgent` | `boolean` | Não | `false` | Indicação de urgência. | RF050, RF013 |
| `photo_paths` | `varchar(500)[]` | Não | `'{}'` | No máximo 5 caminhos. | RF052, RF013, P13 |
| `expires_at` | `timestamptz` | Sim | — | Data-limite de publicação (opcional). | RF050, RF057 |
| `status` | `job_status` | Não | `UNPUBLISHED` | Não publicada, publicada, suspensa ou encerrada. | RF050, RF053, RF055 a RF057, RF115 |
| `first_published_at` | `timestamptz` | Sim | — | Primeira publicação; não muda numa nova publicação. | RF096 |
| `published_at` | `timestamptz` | Sim | — | Última publicação (data de publicação apresentada). | RF053, RF098, RF115 |
| `suspended_at` | `timestamptz` | Sim | — | Última suspensão. | RF055 |
| `deleted_at` | `timestamptz` | Sim | — | Eliminação da vaga pelo Recrutador. Uma vaga eliminada deixa de ser apresentada e de poder ser gerida (DM-09). | RF114, RF058 |

### 6.12. `job_skill` e `job_benefit`

| Tabela | Coluna | Tipo | Nulo | Regras e domínio | Origem |
| --- | --- | --- | --- | --- | --- |
| `job_skill` | `job_id` | `uuid` | Não | Parte da chave primária; chave estrangeira para `job`. | RF050 |
| `job_skill` | `skill_id` | `uuid` | Não | Parte da chave primária; chave estrangeira para `skill`. Só competências da lista. | RF050, RF062 |
| `job_benefit` | `job_id` | `uuid` | Não | Parte da chave primária; chave estrangeira para `job`. | RF050 |
| `job_benefit` | `benefit_id` | `uuid` | Não | Parte da chave primária; chave estrangeira para `benefit`. Só benefícios da lista. | RF050, RF016; DA, F005 |

### 6.13. `match` — resposta a vaga, decisão, match e conversa

| Coluna | Tipo | Nulo | Predefinido | Regras e domínio | Origem |
| --- | --- | --- | --- | --- | --- |
| `id` | `uuid` | Não | `gen_random_uuid()` | Chave primária. Identifica também a conversa do match. | RF068 |
| `candidate_id` | `uuid` | Não | — | Chave estrangeira para `candidate`. Único em conjunto com `job_id`. Autor da recusa ou do interesse. | RF018, RF019, RF112 |
| `job_id` | `uuid` | Não | — | Chave estrangeira para `job`. Vaga de origem. | RF019, RF068 |
| `status` | `match_status` | Não | — | Estado da resposta e do match: `DECLINED_BY_CANDIDATE` (recusa da vaga pelo Candidato), `WAITING` (interesse em espera de decisão), `MATCHED` (match confirmado) ou `REJECTED_BY_RECRUITER` (recusa do Candidato pelo Recrutador). Uma decisão do Recrutador não é alterada depois de registada (backend). | RF018, RF019, RF060, RF063, RF064, RF066, RF113, RF117 |
| `candidate_action_at` | `timestamptz` | Não | `now()` | Data e hora do interesse ou da recusa. | RF060, RF096, RF112 |
| `profile_opened_at` | `timestamptz` | Sim | — | Data e hora da primeira abertura do perfil completo para esta vaga. O Recrutador que abre o perfil é o da Empresa da vaga (DM-03). | RF118, RF063, RF064 |
| `recruiter_action_at` | `timestamptz` | Sim | — | Data e hora da decisão; com `status = 'MATCHED'` é também o instante da confirmação do match. | RF065, RF066, RF096 |
| `decided_by` | `uuid` | Sim | — | Chave estrangeira para `recruiter`. Recrutador que decidiu. | RF065 |
| `conversation_status` | `conversation_status` | Sim | — | Preenchido só com `status = 'MATCHED'`: `OPEN` ou `CLOSED` (modo apenas de consulta). | RF068, RF030, RF074, RF075 |
| `conversation_closed_at` | `timestamptz` | Sim | — | Instante em que a conversa passou ao modo apenas de consulta. | RF030, RF074, RF075 |
| `conversation_closed_by` | `uuid` | Sim | — | Chave estrangeira para `app_user`. Parte que encerrou; `NULL` quando o encerramento foi automático. | RF030, RF074 |
| `conversation_close_reason` | `conversation_close_reason` | Sim | — | `CLOSED_BY_PARTY`, `ACCOUNT_BLOCKED_OR_SUSPENDED` ou `COMPANY_SUSPENDED`. Mantém-se depois de qualquer reativação. | RF075 |

Valores do estado, com as transições do diagrama de estados do interesse e do match:

| Situação | `status` | Transição |
| --- | --- | --- |
| Recusa da vaga pelo Candidato | `DECLINED_BY_CANDIDATE` | T1 |
| Candidato em espera | `WAITING` | T2, T3, T4 |
| Match confirmado | `MATCHED` | T5 |
| Recusa do Candidato pelo Recrutador | `REJECTED_BY_RECRUITER` | T6 |

Um interesse em espera só sai desse estado pela decisão do Recrutador (RF113). A transição T7 do diagrama de estados, proposta numa decisão ainda por ratificar, não faz parte das regras em vigor (secção 11).

### 6.14. `message` — mensagem

| Coluna | Tipo | Nulo | Predefinido | Regras e domínio | Origem |
| --- | --- | --- | --- | --- | --- |
| `id` | `uuid` | Não | `gen_random_uuid()` | Chave primária. | — |
| `match_id` | `uuid` | Não | — | Chave estrangeira para `match` (conversa). Só matches confirmados com conversa aberta aceitam mensagens (backend). | RF026, RF069, RF070 |
| `sender_id` | `uuid` | Não | — | Chave estrangeira para `app_user`: o Candidato ou o Recrutador do match (backend). | RF026, RF069 |
| `content` | `varchar(1000)` | Não | — | Texto com 1 a 1000 caracteres, não só espaços. Confidencial. Não é editável nem eliminável. | RF026, RF069, P03, RF103, RNF017; DA, F010 |
| `sent_at` | `timestamptz` | Não | `now()` | Data e hora de envio. | RF028, RF072, RF096 |
| `read_at` | `timestamptz` | Sim | — | Indicação de leitura pelo destinatário. | RF027, RF028, RF108, RF109 |

### 6.15. `operation_log` — registo de autoria das operações relevantes

| Coluna | Tipo | Nulo | Predefinido | Regras e domínio | Origem |
| --- | --- | --- | --- | --- | --- |
| `id` | `bigint` | Não | identidade | Chave primária, sequencial. | — |
| `user_id` | `uuid` | Sim | — | Chave estrangeira para `app_user`. Autor; `NULL` só no encerramento automático de vaga. | RF112, RF057 |
| `operation` | `operation_type` | Não | — | Operação enumerada no RF112. | RF112 |
| `entity_id` | `uuid` | Não | — | Identificador do elemento a que a operação se refere (conta, Empresa, vaga, competência ou benefício, conforme a operação). Sem chave estrangeira, porque o elemento pode estar em tabelas diferentes. | RF112 |
| `occurred_at` | `timestamptz` | Não | `now()` | Data e hora da operação. | RF112, RF096 |

O tipo de elemento resulta da operação: `ACCOUNT_*` refere-se a uma conta, `COMPANY_*` a uma Empresa, `JOB_*` a uma vaga, `SKILL_*` a uma competência e `BENEFIT_*` a um benefício. Só recebe inserções e não é apresentado na plataforma, porque a DA exclui a consulta de um registo detalhado de auditoria (F009). O interesse e a recusa de vaga e a decisão do Recrutador ficam registados com autor e data na tabela `match` (`candidate_id` e `candidate_action_at`; `decided_by` e `recruiter_action_at`).

---

## 7. Integridade

### 7.1. Regras garantidas pela base de dados

| Regra | Mecanismo | Requisitos |
| --- | --- | --- |
| Formatos de correio eletrónico, telefone, NIF e hiperligações | `CHECK` com expressões regulares | P04, P10 |
| Unicidade do correio eletrónico, do NIF e das designações das listas | Índices únicos e restrições `UNIQUE` | RF004, RF037, RF042, RF099 a RF102 |
| Uma Empresa por Recrutador e um Recrutador por Empresa | `company.recruiter_id` `NOT NULL UNIQUE` | Especificação, secção 2.2 |
| Máximo de 3 hiperligações, 6 fotografias da galeria, 5 fotografias da vaga | `position` 1 a 3 único; `cardinality()` | P10, P13 |
| Limites de comprimento | Tamanho das colunas `varchar` e `CHECK` | P03, P07, P08, P12 |
| Quota de interesses entre 0 e 10; bloqueio só com a quota esgotada | `CHECK` | P11 |
| Intervalo salarial positivo e coerente | `CHECK` | RF051 |
| Uma resposta por par Candidato–vaga | `UNIQUE (candidate_id, job_id)` | RF019 |
| Recusa da vaga sem decisão do Recrutador nem abertura do perfil | `ck_match_declined` | RF018, RF060 |
| Decisão do Recrutador só depois da abertura do perfil, com autor e data | `ck_match_decision` | RF063, RF064, RF065 |
| Conversa existe se e só se houver match | `ck_match_conversation` | RF066, RF068, RF070 |
| Encerramento da conversa com data e motivo; autor só quando encerrada por uma das partes | `ck_match_conversation_close`, `ck_match_closed_by` | RF030, RF074, RF075 |
| Motivo obrigatório só na recusa do registo da Empresa | `ck_company_reason`, `ck_company_reason_length` | RF094, P12 |
| Estado `SUSPENDED` só em contas de Candidato; Administrador sempre ativo | `CHECK` | RF086, RF089 |

### 7.2. Regras garantidas pelo backend

Estas regras dependem de outras linhas ou do momento da operação e ficam nos serviços de negócio, dentro de transações (arquitetura, secção 2). A validação automática é sempre aplicada no servidor (RNF008).

| Regra | Requisitos |
| --- | --- |
| A especialização (`candidate`, `recruiter`, `admin`) corresponde ao `user_type` da conta. | RF085, RNF006 |
| As operações reservadas confirmam o estado da conta e, no Recrutador, o estado da Empresa. | RF001, RF038, RF039 |
| Uma vaga só é eliminada sem interesses registados (sem linhas com `status <> 'DECLINED_BY_CANDIDATE'`); a eliminação preenche `deleted_at` e fica no `operation_log`. As vagas com `deleted_at` são excluídas de todas as consultas. | RF058, RF114, RF112 |
| O NIF não é alterado depois da aprovação. | RF049; DA, F003 |
| Uma decisão do Recrutador não é alterada depois de registada. | RF117; DA, F007 |
| Só o Candidato ou o Recrutador do match enviam mensagens, e só com a conversa aberta. | RF026, RF069, RF070 |
| Confirmação do match numa única transação: atualizar `match` (incluindo `conversation_status = 'OPEN'`). | RF066, RF068 |
| Bloqueio ou suspensão de conta, e suspensão de Empresa: passar a `CLOSED` as conversas abertas, com o motivo correspondente. Os interesses em espera mantêm-se (RF113). | RF075, RF086, RF089, RF095, RF113 |
| Tarefas periódicas: encerrar as vagas publicadas com `expires_at` atingida (`status = 'CLOSED'`, registado no `operation_log` como `JOB_CLOSED` sem autor) e repor a quota. | RF057, RF021 |
| Vagas apresentadas ao Candidato: só vagas publicadas, não eliminadas e não expiradas (`expires_at` nula ou posterior ao instante do pedido, mesmo que a tarefa periódica ainda não tenha encerrado a vaga); filtro do RF014 e distância em linha reta pela fórmula de Haversine sobre `location`, arredondada às unidades. | RF014, RF015, RF057 |
| Acesso ao CV apenas pelo próprio Candidato e pelo Recrutador de uma vaga em que o Candidato está em espera. | RNF007; DA, F004 |
| Formato real do ficheiro (JPEG, PNG ou PDF) e dimensão máxima. | P01, P02, RNF016 |
| Inserção no `operation_log` em cada operação do RF112. | RF112 |
| Manifestação de interesse: verificar a quota, inserir a linha de `match` e atualizar `swipe_rights_remaining` e `next_swipe_available_at` na mesma transação, com o controlo de concorrência da secção 7.3. | RF019, RF020, P11 |

### 7.3. Concorrência

As tabelas `match` e `candidate` usam a coluna de sistema `xmin` do PostgreSQL como marca de concorrência otimista, mapeada pelo Npgsql no Entity Framework Core. Não é criada coluna própria de versão. Se a linha tiver sido alterada depois de lida, a gravação falha e a operação é rejeitada sem alterar dados.

| Tabela | Situação protegida | Requisitos |
| --- | --- | --- |
| `match` | Dois pedidos de decisão sobre o mesmo Candidato na mesma vaga (por exemplo, aceitar e recusar em dois separadores): só o primeiro é gravado. | RF063, RF064, RF117 |
| `match` | Encerramento da conversa por uma das partes em simultâneo com o bloqueio ou a suspensão que a passa ao modo apenas de consulta: fica registado um único motivo de encerramento. | RF030, RF074, RF075 |
| `candidate` | Dois interesses simultâneos do mesmo Candidato em vagas diferentes, lidos com o mesmo valor de `swipe_rights_remaining`: o segundo é rejeitado e repetido com o valor atualizado, sem ultrapassar a quota. | RF019, RF020, P11 |

Dois interesses na mesma vaga são impedidos pela restrição `uq_match_candidate_job` (RF019).

---

## 8. Dados sensíveis

| Dado | Coluna | Proteção | Requisitos |
| --- | --- | --- | --- |
| Palavra-passe | `app_user.password_hash` | Só o resumo irreversível com valor aleatório por conta. Nunca nos registos de diagnóstico. | RNF003, RNF017 |
| Credencial de sessão | Não persistida | Emitida no início de sessão e validada em cada pedido; expira ao fim de 8 horas. Nunca nos registos de diagnóstico. Ao terminar a sessão, o cliente descarta a credencial (secção 11). | RNF004, RNF005, RNF017, RF105 a RF107 |
| Contactos pessoais do Candidato | `app_user.email`, `candidate.phone_number` | Visíveis ao Recrutador só depois do match; ao Administrador na lista de contas. | RF067, RF085, RF088 |
| Curriculum vitae e fotografia | `candidate.cv_path`, `candidate.profile_photo_path` e ficheiros | CV só para o próprio Candidato e para o Recrutador de uma vaga em que o Candidato está em espera; pedidos diretos ao ficheiro rejeitados. | RF061, RNF007 |
| Conteúdo das mensagens | `message.content` | Sem acesso do Administrador; os indicadores usam apenas contagens; nunca nos registos de diagnóstico. | RF103, RF096, RNF017 |
| Contactos da Empresa | `company.contact_email`, `company.contact_phone` | Visíveis ao Candidato só depois do match. | RF025 |
| Pretensão salarial e preferências | `candidate.min_salary_expectation` e preferências | Usadas apenas no filtro de vagas; não fazem parte do perfil completo apresentado ao Recrutador. | RF014, RF061 |

Todos os dados são fictícios (P18; RI, secção 14.3). A certificação formal de conformidade legal em matéria de proteção de dados está fora do âmbito (DA, secção 3).

---

## 9. Dados iniciais

Os dados iniciais são criados por scripts do backend (RI, secção 14.3).

| Tabela | Conteúdo | Origem |
| --- | --- | --- |
| `location` | Lista pré-definida de localidades com coordenadas. Obrigatória antes de qualquer Candidato, Empresa ou vaga. | DA, F002 |
| `skill`, `benefit` | Listas iniciais de competências e de benefícios. | DA, F009 |
| `app_user`, `admin` | Pelo menos uma conta de Administrador. | RF083 |
| Restantes | Dados de demonstração: mínimo de 20 Candidatos com conta ativa, 5 Empresas aprovadas, 2 Empresas pendentes e 50 vagas publicadas, todos fictícios. | P18 |

---

## 10. Decisões de modelação

| ID | Decisão | Fundamentação |
| --- | --- | --- |
| DM-01 | Tipos enumerados nativos do PostgreSQL para todos os domínios fechados, com os valores da especificação de requisitos (secção 5.1). | Domínios fixados nos parâmetros P09, P14, P15 e P16 e nos estados da secção 2.2 da especificação. O estado «suspensa» da conta corresponde à «desativação» da DA (F001), como fixa a secção 2.2 da especificação. |
| DM-02 | Uma tabela de contas (`app_user`) e três tabelas de especialização (`candidate`, `recruiter`, `admin`) com a mesma chave primária. | Os três tipos de conta partilham credenciais, estado e unicidade do correio eletrónico (RF037: «único no sistema»), mas têm dados próprios. |
| DM-03 | Relação Recrutador–Empresa 1 : 0..1, com `company.recruiter_id` obrigatório e único. As vagas pertencem à Empresa e não guardam o Recrutador. | Especificação, secção 2.2: cada Empresa tem um único Recrutador e o Recrutador responsável pela vaga é o da Empresa. A conta de Recrutador é criada antes da Empresa (RF037 e RF041 são passos distintos do UC07). |
| DM-04 | A tabela `match` guarda a resposta do Candidato, a abertura do perfil, a decisão do Recrutador e o match, numa única linha por par Candidato–vaga. | RF019 (uma única resposta por vaga), RF063, RF064, RF065, RF118 e o diagrama de estados do interesse e do match. Um único `status` com quatro valores e as restrições da secção 5.2 impedem combinações de estados inválidas (DM-21). |
| DM-05 | A conversa não tem tabela própria: os atributos ficam em `match`, e o modo apenas de consulta é guardado (`conversation_status = 'CLOSED'` com `conversation_close_reason`). | RF068 e DA, F010: uma única conversa por match, criada no instante da confirmação. RF075: a conversa mantém-se só de consulta depois de qualquer reativação, o que obriga a guardar o encerramento em vez de o calcular a partir do estado atual da conta ou da Empresa. |
| DM-06 | Preferências de procura e fotografias como arrays (`work_mode[]`, `contract_type[]`, `varchar(500)[]`) com limite de cardinalidade. | Domínios pequenos e fechados, sem atributos próprios, e limites fixos (P13). A ordem da lista é a ordem de apresentação. |
| DM-08 | Nova tabela `operation_log`, só de inserção e não apresentada na plataforma. | RF112 exige registar o autor e a data e hora de cada operação enumerada, incluindo mudanças sucessivas de estado e eliminações, que o estado atual das tabelas não conserva. É o único registo de quem e quando mudou o estado de uma conta ou de uma Empresa (DM-18). RF096 precisa do estado das contas e das Empresas numa data passada. A DA (F009) exclui apenas a **consulta** de um registo detalhado de auditoria. |
| DM-09 | Eliminação lógica da vaga (`deleted_at`). | RF114 permite eliminar uma vaga sem interesses, mas a vaga pode ter recusas de Candidatos, que o RF112 obriga a conservar. A eliminação lógica tira a vaga de todas as consultas sem perder esses registos. |
| DM-10 | `job.first_published_at` separado de `job.published_at`. | RF096 conta as vagas publicadas pela primeira vez no período; `published_at` muda numa nova publicação (RF115). |
| DM-11 | Preferências de procura opcionais: `max_distance_km` e `min_salary_expectation` sem valor predefinido, e listas vazias de regimes e tipos de contrato = sem restrição. | Nenhum requisito fixa valores iniciais e o RF003 não pede preferências no registo. Sem esta regra, um Candidato acabado de registar não veria nenhuma vaga (RF014). |
| DM-12 | Comprimentos máximos técnicos onde a especificação não fixa limite: nomes até 150 caracteres (Empresa até 200), morada e correio eletrónico até 255, endereços e caminhos até 500, função da vaga até 150, designações das listas até 100. | Limites folgados para os dados de demonstração. Uma alteração a estes limites não altera nenhum requisito. |
| DM-13 | Colunas de ficheiros com o caminho do ficheiro (`*_path`). | Arquitetura, decisão AD-01: os ficheiros ficam no sistema de ficheiros do servidor do backend. |
| DM-14 | Abertura do perfil guardada uma vez por resposta (`profile_opened_at`, primeira abertura). | RF118 regista a abertura do perfil por vaga; é a primeira abertura que habilita a decisão (RF063, RF064). |
| DM-15 | Sem colunas para dados que nenhum requisito usa (por exemplo, tipo de hiperligação, desativação de competências ou benefícios, nome ou cargo do Recrutador, último início de sessão). | DA e especificação de requisitos: não guardar dados de funcionalidades não previstas. RF099 a RF102 só preveem acrescentar e alterar designações. |
| DM-16 | Competências do Candidato em duas tabelas: `candidate_skill` (lista pré-definida, chave primária composta) e `candidate_custom_skill` (opção «Outro»). | Corrige a observação do docente sobre a relação Candidato–competência (`OF-M3-023`) e simplifica o modelo: a relação com a lista passa a N : M pura, sem chave estrangeira anulável nem restrição de exclusividade. A competência «Outro» não pertence à lista e só existe para o Candidato que a indicou (RF008, RF009). |
| DM-17 | As vagas apresentadas ao Candidato excluem as vagas expiradas pela própria data `expires_at`, sem esperar pela tarefa periódica de encerramento. | A tarefa periódica pode ter atraso; sem esta regra o Candidato poderia mostrar interesse numa vaga já expirada (RF014, RF057). |
| DM-18 | A autoria e a data das mudanças de estado das contas e das Empresas ficam só no `operation_log`; `app_user` e `company` não têm colunas `status_changed_*`. | O RF112 obriga a registar cada bloqueio, reativação, suspensão, aprovação e recusa com autor e data. Repetir a informação nas tabelas duplicava-a e obrigava a duas escritas coerentes. O estado numa data passada (RF096) e o autor da última mudança obtêm-se da última operação do registo até essa data. |
| DM-19 | O `operation_log` não tem `entity_type`. | A operação determina o tipo do elemento (`ACCOUNT_*`, `COMPANY_*`, `JOB_*`, `SKILL_*`, `BENEFIT_*`). Uma coluna própria repetia a informação e admitia combinações incoerentes. |
| DM-20 | A tabela `match` não tem `last_message_at`. | A ordenação da lista de conversas e a última mensagem (RF027, RF071) calculam-se com o máximo de `message.sent_at` por conversa, apoiado no índice `ix_message_match_sent`. Evita atualizar a `match` a cada mensagem e a divergência entre as duas tabelas. |
| DM-21 | A `match` tem um único `status` com quatro valores (`DECLINED_BY_CANDIDATE`, `WAITING`, `MATCHED`, `REJECTED_BY_RECRUITER`), em vez de três colunas de estado. | Só existem quatro combinações válidas de estados (diagrama de estados, T1 a T6). Um único valor torna impossíveis as combinações inválidas e dispensa as restrições que as impediam. O interesse registado é qualquer `status` diferente de `DECLINED_BY_CANDIDATE`. |
| DM-22 | Sem `created_at` e `updated_at` nas tabelas `admin`, `recruiter`, `candidate`, `skill`, `benefit`, `company`, `job` e `match`. Mantêm-se em `app_user`. | Nenhum requisito os usa. A data de registo é a de `app_user.created_at` (RF088), e as datas de criação e alteração de Empresas, vagas e listas constam do `operation_log` (RF112). Na `match`, a data de criação é `candidate_action_at`. |
| DM-23 | A `job` não tem `closed_at` nem `close_reason`. | Nenhum requisito nem DTO os apresenta. A data do encerramento consta do `operation_log` (`JOB_CLOSED`), e o encerramento automático distingue-se pela ausência de autor (`user_id` nulo). |
| DM-24 | A `match` não tem `matched_at` nem `profile_opened_by`. | `matched_at` é igual a `recruiter_action_at` quando o estado é `MATCHED`. O Recrutador que abre o perfil é sempre o da Empresa da vaga (DM-03), e o RF118 só exige a data e hora da abertura. |

A decisão DM-07 (tabela `notification`) foi retirada na v02 com a funcionalidade F011. A numeração das restantes decisões mantém-se, para não alterar as referências existentes.

---

## 11. Limitações

| Limitação | Consequência |
| --- | --- |
| Os testes de integração usam SQLite in-memory, que não tem tipos enumerados nativos, arrays, `xmin` nem o operador `~` das expressões regulares. | Estas restrições não são exercitadas pelos testes de integração. São verificadas nos testes de sistema, que correm sobre PostgreSQL, e na revisão das migrations. |
| O ficheiro e o caminho guardado podem ficar inconsistentes (por exemplo, ficheiro apagado no servidor). | O backend grava o ficheiro antes de guardar o caminho e apaga o ficheiro substituído depois de confirmar a transação. |
| O `operation_log` não tem chave estrangeira para o elemento registado. | A coerência entre a operação e o elemento identificado por `entity_id` depende do backend. |
| A eliminação de vagas é lógica. | Todas as consultas têm de excluir as vagas com `deleted_at` preenchido. |
| A transição T7 (encerramento do interesse em espera sem decisão do Recrutador), proposta em `m2-decisao-encerramento-interesses-em-espera-v01.md`, não está ratificada e contraria o RF113 em vigor. | Não faz parte das regras deste modelo. Se a decisão for ratificada e os requisitos forem alterados, entra numa nova versão; o `match_status` não tem valor para esse encerramento e teria de ser acrescentado. |
| A credencial de sessão não é persistida. | Depois de terminar a sessão (RF105 a RF107), uma credencial copiada continua aceite em pedidos diretos até expirar (8 horas, RNF005). Risco aceite no ambiente académico; uma lista de credenciais revogadas exige uma tabela nova numa versão futura. |
| As expressões regulares com `[:alnum:]` dependem da configuração regional da base de dados para aceitar letras acentuadas. | A base de dados é criada em UTF-8 com uma configuração regional portuguesa ou ICU. |

---

## 12. Rastreabilidade

| Requisitos | Tabelas e colunas |
| --- | --- |
| RF001 a RF005, RF037, RF038, RF083 (registo e início de sessão) | `app_user`, `candidate`, `recruiter`, `admin` |
| RF002, RF040, RF084 (palavra-passe) | `app_user.password_hash` |
| RF105 a RF107 (terminar sessão) | Sem persistência: a credencial de sessão não é guardada (RNF005; secção 11). |
| RF006 a RF012 (perfil profissional) | `candidate`, `candidate_link`, `candidate_skill`, `candidate_custom_skill`, `skill` |
| RF013 a RF023 (exploração de vagas e quota) | `job`, `company`, `location`, `job_skill`, `job_benefit`, `match`, `candidate.swipe_rights_remaining`, `candidate.next_swipe_available_at` |
| RF024, RF025, RF067 (matches e contactos) | `match`, `company`, `app_user`, `candidate` |
| RF026 a RF030, RF068 a RF075, RF108, RF109 (conversas) | `match` (colunas `conversation_*`), `message` |
| RF031 a RF036, RF076 a RF082, RF110, RF111 (notificações) | Retirados com a F011 (PS e DA v03). Sem tabela: a informação é consultada nas áreas de cada aplicação. A numeração será revista na especificação de requisitos v02. |
| RF039, RF041 a RF049 (Empresa) | `company`, `recruiter` |
| RF050 a RF059, RF114, RF115 (vagas) | `job`, `job_skill`, `job_benefit` |
| RF060 a RF066, RF113, RF117, RF118 (decisão sobre Candidatos) | `match` |
| RF085 a RF090 (contas) | `app_user`, `operation_log` (autor e data de cada mudança de estado) |
| RF091 a RF095, RF116 (aprovação e supervisão de Empresas) | `company`, `operation_log` (autor e data de cada decisão) |
| RF096 a RF098 (indicadores e listas) | `app_user`, `company`, `job.first_published_at`, `match`, `message`, `operation_log` |
| RF099 a RF102 (listas pré-definidas) | `skill`, `benefit` |
| RF103, RF104 (restrições do Administrador) | Sem coluna própria; autorização no backend com `app_user.user_type`. |
| RF112 (autoria das operações) | `operation_log`, `match` |
| RNF003, RNF004, RNF005, RNF007, RNF013, RNF016, RNF017 | Secções 7.2 e 8; RNF013 assegurado pela persistência transacional do PostgreSQL |
| P01 a P16 | Restrições da secção 5.2 e regras da secção 7 |
| P18 | Secção 9 |
