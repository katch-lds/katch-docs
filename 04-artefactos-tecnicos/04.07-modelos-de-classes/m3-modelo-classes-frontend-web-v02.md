# Modelo de Classes — Frontend Web

**Unidade curricular:** Laboratório de Desenvolvimento de Software — LDS
**Milestone:** `m3`
**Ficheiro:** `m3-modelo-classes-frontend-web-v02.md`
**Pasta de arquivo:** `04-artefactos-tecnicos/04.07-modelos-de-classes`

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
| Módulo | Área de gestão web (`katch-frontend-web`), React 19.3 com TypeScript 7.0 e Vite 8.3 |
| Issue | `I111` — Atualizar à Declaração de Âmbito v04 os modelos de classes dos frontends e os diagramas de comportamento (v02); versão anterior: `I090` — Elaborar o modelo de classes do frontend web |
| Executor / Revisor / Auditor | v01 (`I090`): Roberto Baptista / João Borguem / Miguel Santos; v02 (`I111`): João Borguem / Roberto Baptista / João Coelho |
| Documentos de origem | `m3-proposta-sistema-v04.pdf` e `m3-declaracao-ambito-v04.pdf` (F010, F003, F007, F008 e limites transversais), homologados em 14-10-2026 (`m3-email-homologacao-ps-da-cca-v02.pdf`); `m3-documentacao-arquitetura-v02.md` (secções 2 e 3, decisão AD-02); `m3-modelo-classes-backend-v02.md` (`I103`; secção 6.1 e DC-07); `m3-especificacao-requisitos-v02.md` (67 requisitos funcionais com componente Frontend web, RNF004 a RNF009, RNF012, RNF018; parâmetros P01 a P21, com o P17 da consulta periódica; secção 1.3, requisitos descontinuados); `m3-especificacoes-casos-uso-v02.md` (UC01, UC02, UC07 a UC14 e UC16 a UC20); `m3-prototipo-baixa-fidelidade-v02.pdf` (Recrutador, ecrãs W01 a W23, W25 e W26; Administrador, ecrãs A01 a A11; sem o W24); `m3-diagrama-estados-interesse-match-v02.md`; `m3-diagrama-atividades-aprovacao-empresa-v02.md`; `m3-modelo-de-dados-katch-v02.md` (DM-02, DM-21, `I102`); `m3-documentacao-api-v02.md` (secções 2 a 8, 10 e 11, `I101` e `I104`); `m3-20261016-reuniao-extraordinaria-ata-v01.pdf` (deliberação 1: consulta periódica de 3 em 3 segundos); `m1-regulamento-grupo-v01.pdf` (secções 11.2 a 11.5); `m2-modelo-classes-frontend-web-v01.md` (versão anterior, conservada) |

O documento segue a secção 19 do Regulamento de Funcionamento da Unidade Curricular para `04.07-modelos-de-classes`: um ficheiro por módulo relevante, com classes, responsabilidades, atributos, operações, relações e multiplicidades. Corresponde à linha `OF-M3-025` — «Modelo de classes — frontend web v2» da Checklist de Controlo de Artefactos (v04); a v01 corresponde à linha `OF-M2-008`.

### 1.1. Histórico de versões

| Versão | Data | Descrição das alterações | Issue |
| --- | --- | --- | --- |
| v01 | 2026-10-08 | Criação do documento: camadas, tipos dos DTOs e enumerações, cliente da API e serviços, ficheiros protegidos, ligação persistente, sessão, rotas protegidas por tipo de conta e por estado da Empresa, páginas e componentes do Recrutador e do Administrador, estado dos ecrãs, dependências do backend por resolver, divergências com os protótipos e rastreabilidade. | `I090` |
| v01 | 2026-10-09 | Alinhamento com a documentação da API v01 (`I092`) antes da revisão: enumerações com os valores JSON da API, cabeçalho `X-Client-App`, tratamento dos erros do catálogo da API, carregamentos em `multipart/form-data`, pedido HTTP de cada operação dos serviços, autenticação e eventos dos hubs; dependências G1 a G10 confrontadas com os pontos em aberto PA-01 a PA-06 da API e acrescentadas G11 e G12. | `I090` |
| v01 | 2026-10-09 | Alinhamento com a versão revista da documentação da API v01 (`I092`): lista de matches só com Empresa aprovada (6.7.1), com a rota `/matches`, a moldura do Recrutador e a divergência P2 ajustadas; ERR-35 também na abertura do perfil e na obtenção do CV; dependências G1, G2, G4, G5 e G10 confrontadas com PA-03 e PA-07 a PA-09; cabeçalho `X-Client-App` descrito como exigido no início de sessão. | `I090` |
| v01 | 2026-10-09 | Melhorias M01 a M06 do relatório de revisão (ciclo 1): ligações página → hook, página → componente e hook → serviço com multiplicidades nos diagramas (nova secção 8.4, relações dos componentes partilhados em 8.1 e dos hooks aos serviços na secção 9); `IResourceState` com o prefixo `I`; exceção do `useProtectedFile` na regra da secção 9; ecrãs da `RecruiterShell`; modelo de dados e decisão da consulta periódica nos documentos de origem; destino da notificação `COMPANY_APPROVED` registado (DF-13). | `I090` |
| v02 | 2026-10-29 | Atualização à Declaração de Âmbito v04 (redução de âmbito, `I100`): retirados a área e o contador de notificações (`NotificationsPage`, rota `/notificacoes`, ecrã W24, `useNotifications`, `NotificationService`, `NotificationType`, `NotificationDto`, `NotificationListDto` e a decisão DF-13) e a ligação persistente (camada Tempo real, `IRealtimeClient`, `SignalRRealtimeClient`, `RealtimeProvider`, `RealtimeStatus` e os dois hubs); acrescentada a atualização periódica das conversas do Recrutador por consulta à API de 3 em 3 segundos (camada Consulta periódica, `IPollingScheduler`, `TimeoutPollingScheduler`, `PollingLoop`, `pollingConfig` e `ConversationsProvider`; secção 6, decisões DF-14 e DF-15). Alinhamento com a documentação da API v02: operações e pedidos da secção 5 (dados e página da própria Empresa, parâmetro `after`, obtenção de ficheiros em `/api/files/{kind}/{fileName}`), tipos dos DTOs com os campos acrescentados pela `I104` (`code` e `blockedUntil` em `ApiErrorDto`, `locationId` em `CompanyRegistrationDto`, `desiredRole` e `locationName` em `WaitingCandidateDto`, `profileOpenedAt` em `CandidateFullProfileDto`, `jobStatus`, `locationName` e `logoUrl` em `MatchDto`, `closedBy` em `ConversationSummaryDto`), identificação dos erros pelo campo `code` e dependências G1 a G12 resolvidas ou sem aplicação. Protótipo v02 (sem o W24) e rastreabilidade aos 67 requisitos funcionais da especificação v02 com componente Frontend web. Lista das alterações na secção 16. | `I111` |

Cada alteração posterior acrescenta uma linha. As versões anteriores são conservadas, nos termos da secção 18.2 do Regulamento de Funcionamento da Unidade Curricular.

---

## 2. Âmbito e convenções

### 2.1. Módulo e camadas

O módulo é a área de gestão web, o cliente do Recrutador e do Administrador (arquitetura, secção 2.3). Não acede à base de dados nem ao sistema de ficheiros: tudo o que apresenta vem da API REST. Não há ligação persistente nem notificações (DA v04, F010 e limites transversais): as mensagens novas, o número de mensagens por ler e o estado das conversas do Recrutador chegam pela consulta periódica à API, de 3 em 3 segundos, enquanto a conversa ou a lista de conversas está aberta (arquitetura v02, secção 2.4 e AD-02; backend v02, secção 6.1; API v02, secção 7). O Recrutador conhece as restantes mudanças — decisão sobre a Empresa, novos interessados, novos matches — consultando a área correspondente.

A aplicação móvel do Candidato é um módulo distinto, com modelo de classes próprio (`I091`). Por isso, os controllers `CandidateProfileController` e `JobExplorationController` do backend não têm serviço correspondente neste módulo.

| Camada | Pasta | Responsabilidade | Depende de |
| --- | --- | --- | --- |
| Páginas | `src/pages` | Uma página por ecrã do protótipo, com os estados e diálogos desse ecrã. Compõe componentes, invoca hooks e trata os estados de carregamento, de erro e de lista vazia. Não chama a API diretamente. | Componentes, Hooks, Sessão |
| Componentes | `src/components` | Blocos de interface reutilizáveis, sem conhecimento da API. Recebem dados por propriedades e devolvem eventos. | Tipos, Hooks (só `useProtectedFile`) |
| Rotas | `src/routes` | Declaração das rotas e proteção por sessão, por tipo de conta e pelo estado da Empresa. | Sessão, Páginas |
| Hooks | `src/hooks` | Estado de cada ecrã: invoca os serviços, guarda o resultado e expõe as operações às páginas. | Serviços, Consulta periódica, Tipos |
| Serviços | `src/services` | Um serviço por controller do backend usado neste módulo. Traduz cada operação da interface numa chamada da API. | Cliente da API, Tipos |
| Cliente da API | `src/api` | Pedido HTTP, credencial de sessão no cabeçalho, limite de tempo, tradução dos erros, obtenção de ficheiros protegidos e deteção da falha de ligação. | Sessão, Tipos |
| Consulta periódica | `src/polling` | Agendamento da repetição de um pedido a intervalo fixo (3 segundos), sem sobreposição de pedidos, e paragem quando o ecrã que a pediu é fechado. Não conhece a API: repete a tarefa que o hook lhe entrega. | — |
| Sessão | `src/session` | Credencial de sessão, tipo de conta, expiração e estado da Empresa do Recrutador. | Serviços, Tipos |
| Tipos | `src/types` | Espelho dos DTOs e das enumerações do backend e mapa de designações em português. Sem lógica. | — |

```mermaid
flowchart TB
    subgraph Web["katch-frontend-web"]
        RT["Rotas<br/>sessão · tipo de conta · estado da Empresa"]
        P["Páginas<br/>um ecrã do protótipo por página"]
        CO["Componentes<br/>blocos de interface"]
        HK["Hooks<br/>estado de cada ecrã"]
        SV["Serviços<br/>um por controller"]
        AC["Cliente da API<br/>HTTPS · JSON · JWT"]
        PL["Consulta periódica<br/>3 s · sem sobreposição"]
        SE["Sessão<br/>credencial · tipo de conta"]
        TP["Tipos<br/>DTOs e enumerações"]
    end
    API["API REST<br/>/api"]
    RT --> P
    RT --> SE
    P --> CO
    P --> HK
    HK --> SV
    HK --> PL
    SV --> AC
    AC --> SE
    AC -->|"HTTPS · JSON · JWT<br/>inclui a consulta periódica<br/>das conversas (3 s)"| API
    SV --> TP
    CO --> TP
    classDef camada fill:#eef2ff,stroke:#3730a3,stroke-width:1.5px,color:#111827
    classDef externo fill:#ffffff,stroke:#1f2937,stroke-width:1.5px,color:#111827
    class P,CO,RT,HK,SV,AC,PL,SE,TP camada
    class API externo
    style Web fill:#f9fafb,stroke:#6b7280,stroke-width:1.5px,color:#111827
    linkStyle default stroke:#374151,stroke-width:1.5px
```

### 2.2. Convenções de código e de diagrama

- **Nomes:** Airbnb TypeScript/React Style Guide, verificado por ESLint e Prettier (RI, secções 11.3 e 11.4). Componentes, páginas e tipos em PascalCase; funções, variáveis e propriedades em camelCase; hooks com o prefixo `use`; interfaces com o prefixo `I`.
- **Tipos:** os tipos do cliente espelham os DTOs do backend com os mesmos nomes e os mesmos campos em camelCase (por exemplo, `CompanyStatusDto.CompanyId` ↔ `companyId`), que é a forma por omissão da serialização JSON do ASP.NET Core. Correspondências: `Guid` ↔ `string`; `DateTimeOffset` ↔ `string` em ISO 8601; `DateOnly` ↔ `string` no formato `aaaa-MM-dd`; `decimal` e `int` ↔ `number`; `bool` ↔ `boolean`; `List<T>` ↔ `T[]`; `T?` ↔ `T | null`. As datas e horas chegam em UTC com o sufixo `Z` e são convertidas para a hora local só na apresentação. As enumerações do backend são uniões de literais de texto com os valores JSON da API, em maiúsculas com `_` e iguais aos do PostgreSQL: `ContractType.ServiceProvision` ↔ `"SERVICE_PROVISION"` (documentação da API, secções 2.3 e 2.4, DAPI-01). A enumeração própria do cliente (`LoginArea`) não vai para a API e usa PascalCase.
- **Diagramas Mermaid (`classDiagram`):** `+` público, `-` privado; atributos na forma `tipo nome`, como no modelo de classes do backend; `T?` = valor que pode ser `null`; `~T~` = tipo genérico. Estereótipos: `<<interface>>`, `<<enumeration>>`, `<<type>>` (tipo de dados sem lógica), `<<page>>`, `<<component>>`, `<<hook>>`, `<<context>>` (contexto React) e `<<constant>>` (valor de configuração).
- **Relações:** `--` associação com multiplicidades; `*--` composição (o elemento composto só existe dentro do todo); `..>` dependência (uso por importação ou por propriedade); `..|>` realização de interface; `<|--` especialização.
- **Assíncrono:** as operações dos serviços, do cliente da API e dos hooks que fazem pedidos devolvem `Promise<T>`; nos diagramas indica-se apenas `T`, como no modelo de classes do backend (secção 2.2).
- **Páginas, componentes e hooks:** são funções React. A notação de classe representa as propriedades (atributos), o estado exposto e os comandos de interface (operações), como exige a secção 19 do Regulamento da UC para os modelos de classes.

---

## 3. Tipos do cliente

### 3.1. Enumerações

As enumerações são as do contrato da API (documentação da API v02, secção 2.4) usadas pela área de gestão web, com os valores JSON. Um valor fora da lista é rejeitado pelo servidor com `400`.

```mermaid
classDiagram
    direction LR
    class UserType {
        <<enumeration>>
        CANDIDATE
        RECRUITER
        ADMIN
    }
    class AccountStatus {
        <<enumeration>>
        ACTIVE
        BLOCKED
        SUSPENDED
    }
    class CompanyStatus {
        <<enumeration>>
        PENDING
        APPROVED
        REJECTED
        SUSPENDED
    }
    class JobStatus {
        <<enumeration>>
        UNPUBLISHED
        PUBLISHED
        SUSPENDED
        CLOSED
    }
    class ContractType {
        <<enumeration>>
        PERMANENT
        FIXED_TERM
        INTERNSHIP
        SERVICE_PROVISION
    }
    class WorkMode {
        <<enumeration>>
        ON_SITE
        HYBRID
        REMOTE
    }
    class Availability {
        <<enumeration>>
        IMMEDIATE
        FIFTEEN_DAYS
        ONE_MONTH
        TO_BE_AGREED
    }
    class Industry {
        <<enumeration>>
        TECHNOLOGY
        HEALTHCARE
        RETAIL
        HOSPITALITY
        CONSTRUCTION
        MANUFACTURING
        EDUCATION
        FINANCE
        LOGISTICS
        SERVICES
        OTHER
    }
    class ConversationStatus {
        <<enumeration>>
        OPEN
        CLOSED
    }
    class ConversationCloseReason {
        <<enumeration>>
        CLOSED_BY_PARTY
        ACCOUNT_BLOCKED_OR_SUSPENDED
        COMPANY_SUSPENDED
    }
```

`MatchStatus`, `OperationType` e `ClientApp` são enumerações internas do backend: nenhum DTO as contém e não fazem parte do contrato (documentação da API v02, secção 2.4). O ponto de acesso (`ClientApp`) é indicado no cabeçalho `X-Client-App` (secção 4.1). `FileKind` só aparece na rota de `GET /api/files/{kind}/{fileName}`, dentro dos URL que os DTOs já trazem prontos; o cliente nunca constrói esses URL e, por isso, não declara o tipo (secção 4.3). A enumeração `NotificationType` da v01 foi retirada com a funcionalidade F011.

A língua da interface é apenas o português (arquitetura, secção 2.5). As designações apresentadas estão num único mapa de apresentação (`src/types/labels.ts`):

| Enumeração | Designações em português | Origem |
| --- | --- | --- |
| `UserType` | Candidato; Recrutador; Administrador | RF085 |
| `AccountStatus` | ativa; bloqueada; suspensa | RF085, RF088 |
| `CompanyStatus` | pendente; aprovada; recusada; suspensa | RF044, RF097 |
| `JobStatus` | Não publicada; Publicada; Suspensa; Encerrada | W09, W21 |
| `ContractType` | Sem termo; A termo; Estágio; Prestação de serviços | P09 |
| `WorkMode` | Presencial; Híbrido; Remoto | P16 |
| `Availability` | Imediata; 15 dias; 1 mês; A combinar | P14 |
| `Industry` | Tecnologia; Saúde; Comércio; Hotelaria e restauração; Construção; Indústria; Educação; Finanças; Logística; Serviços; Outro | P15 |
| `ConversationStatus` | Aberta; Encerrada | W22, W23 |
| `ConversationCloseReason` | Encerrada por uma das partes; Conta bloqueada ou suspensa; Empresa suspensa | RF074, RF075 |

No motivo `CLOSED_BY_PARTY`, a página distingue «Encerrada por si» de «Encerrada pelo Candidato» comparando `ConversationSummaryDto.closedBy` com o `userId` da sessão (documentação da API v02, 8.4).

### 3.2. Tipos dos DTOs

Os tipos espelham os DTOs do backend (modelo de classes do backend, secções 7.1 e 7.2), campo a campo. São tipos de dados sem lógica.

```mermaid
classDiagram
    direction LR
    class LoginRequest {
        <<type>>
        +string email
        +string password
    }
    class LoginResponse {
        <<type>>
        +string token
        +string expiresAt
        +UserType userType
        +string userId
    }
    class CreateRecruiterAccountRequest {
        <<type>>
        +string email
        +string password
        +boolean acceptTerms
    }
    class ChangePasswordRequest {
        <<type>>
        +string currentPassword
        +string newPassword
    }
    class CompanyRegistrationRequest {
        <<type>>
        +string companyName
        +string taxId
        +Industry industry
        +string address
        +string locationId
        +string contactEmail
        +string contactPhone
        +string responsibleName
    }
    class CompanyRegistrationDto {
        <<type>>
        +string companyId
        +string companyName
        +string taxId
        +Industry industry
        +string address
        +string locationId
        +string locationName
        +string contactEmail
        +string contactPhone
        +string responsibleName
        +string submittedAt
    }
    class CompanyStatusDto {
        <<type>>
        +string companyId
        +CompanyStatus status
        +string? rejectionReason
        +string submittedAt
    }
    class CompanyPageDto {
        <<type>>
        +string companyId
        +string companyName
        +string? description
        +string? website
        +string? logoUrl
        +string[] photoUrls
    }
    class CompanyPageRequest {
        <<type>>
        +string? description
        +string? website
    }
    class JobRequest {
        <<type>>
        +string title
        +string description
        +number minSalary
        +number maxSalary
        +string locationId
        +ContractType contractType
        +WorkMode workMode
        +string[] skillIds
        +string[] benefitIds
        +boolean isUrgent
        +string? expiresAt
    }
    class JobDto {
        <<type>>
        +string jobId
        +string title
        +string description
        +number minSalary
        +number maxSalary
        +string locationId
        +string locationName
        +ContractType contractType
        +WorkMode workMode
        +ReferenceItemDto[] skills
        +ReferenceItemDto[] benefits
        +boolean isUrgent
        +string? expiresAt
        +JobStatus status
        +string? publishedAt
        +string[] photoUrls
        +number waitingCandidates
    }
    class ReferenceItemDto {
        <<type>>
        +string id
        +string name
    }
    class ReferenceItemRequest {
        <<type>>
        +string name
    }
    JobDto "1" *-- "0..*" ReferenceItemDto : competências e benefícios
```

```mermaid
classDiagram
    direction LR
    class WaitingCandidateDto {
        <<type>>
        +string matchId
        +string candidateId
        +string fullName
        +string? desiredRole
        +string locationName
        +string interestAt
    }
    class CandidateFullProfileDto {
        <<type>>
        +string matchId
        +string fullName
        +string? photoUrl
        +string? desiredRole
        +string locationName
        +Availability? availability
        +string? experienceSummary
        +string[] links
        +SkillTagDto[] skills
        +boolean hasCv
        +string profileOpenedAt
    }
    class SkillTagDto {
        <<type>>
        +string name
        +boolean matchesJob
    }
    class MatchDto {
        <<type>>
        +string matchId
        +string jobTitle
        +JobStatus jobStatus
        +string locationName
        +string companyName
        +string? logoUrl
        +string counterpartName
        +string contactEmail
        +string contactPhone
        +string matchedAt
    }
    class ConversationSummaryDto {
        <<type>>
        +string matchId
        +string counterpartName
        +string jobTitle
        +string companyName
        +ConversationStatus status
        +ConversationCloseReason? closeReason
        +string? closedBy
        +string? lastMessageAt
        +number unreadCount
    }
    class MessageDto {
        <<type>>
        +string messageId
        +string senderId
        +string content
        +string sentAt
        +string? readAt
    }
    class SendMessageRequest {
        <<type>>
        +string content
    }
    CandidateFullProfileDto "1" *-- "0..*" SkillTagDto : competências
```

```mermaid
classDiagram
    direction LR
    class AccountDto {
        <<type>>
        +string userId
        +string email
        +UserType userType
        +AccountStatus status
    }
    class CandidateAdminDto {
        <<type>>
        +string candidateId
        +string fullName
        +string email
        +string locationName
        +string registeredAt
        +AccountStatus status
    }
    class PendingCompanyDto {
        <<type>>
        +string companyId
        +string companyName
        +string taxId
        +string submittedAt
    }
    class CompanyAdminDto {
        <<type>>
        +string companyId
        +string companyName
        +CompanyStatus status
        +number publishedJobs
    }
    class PublishedJobAdminDto {
        <<type>>
        +string jobId
        +string title
        +string companyName
        +string publishedAt
        +number interestCount
    }
    class RejectCompanyRequest {
        <<type>>
        +string reason
    }
    class IndicatorsDto {
        <<type>>
        +number activeCandidates
        +number approvedCompanies
        +number pendingCompanies
        +number jobsFirstPublished
        +number interestsExpressed
        +number matchesConfirmed
        +number conversationsStarted
    }
    class ApiErrorDto {
        <<type>>
        +string code
        +string message
        +FieldErrorDto[] fields
        +string? blockedUntil
    }
    class FieldErrorDto {
        <<type>>
        +string field
        +string reason
    }
    ApiErrorDto "1" *-- "0..*" FieldErrorDto : campos em causa
```

São 29 tipos, com os campos da documentação da API v02 (secção 8). Os campos acrescentados pela `I104` (`ApiErrorDto.code` e `blockedUntil`, `WaitingCandidateDto.desiredRole` e `locationName`, `CandidateFullProfileDto.profileOpenedAt`, `MatchDto.jobStatus`, `locationName` e `logoUrl`, `ConversationSummaryDto.closedBy`) fazem parte do contrato v02, que o cliente segue; o modelo de classes do backend v02 só os acompanha numa versão posterior (documentação da API v02, L-08 e DAPI-09). `blockedUntil` só é preenchido no ERR-31, que não ocorre neste módulo. Os tipos próprios do Candidato (`RegisterCandidateRequest`, `CandidateProfileDto`, `CustomSkillDto`, `UpdateCandidateProfileRequest`, `SearchPreferencesDto`, `AddSkillRequest`, `AddCustomSkillRequest`, `SetLinksRequest`, `JobCardDto`, `JobDetailDto`, `QuotaDto`) pertencem ao modelo de classes da aplicação móvel (`m3-modelo-classes-frontend-mobile-v02.md`).

`photoUrl`, `logoUrl` e `photoUrls` são URL da API, na forma `/api/files/{kind}/{fileName}`, que servem o ficheiro depois de verificar a autorização (documentação da API v02, 2.6 e 6.9.1). O cliente nunca constrói esses URL; usa-os tal como chegam e obtém o conteúdo como descrito na secção 4.3.

---

## 4. Acesso à API

### 4.1. Cliente da API

```mermaid
classDiagram
    direction LR
    class IApiClient {
        <<interface>>
        +get(string path) T
        +post(string path, unknown body) T
        +put(string path, unknown body) T
        +delete(string path) void
        +upload(string method, string path, File file) string?
        +getFile(string path) Blob
    }
    class HttpApiClient {
        -string _baseUrl
        -number _timeoutMs
        -string _clientApp
        -SessionStore _session
        -ConnectionMonitor _monitor
        -request(string method, string path, unknown body) Response
        -buildHeaders() Headers
        -toApiError(Response response) ApiError
        +get(string path) T
        +post(string path, unknown body) T
        +put(string path, unknown body) T
        +delete(string path) void
        +upload(string method, string path, File file) string?
        +getFile(string path) Blob
    }
    class Error
    class ApiError {
        +number httpStatus
        +string? code
        +string message
        +FieldErrorDto[] fields
        +fieldReason(string field) string?
        +isUnauthorized() boolean
        +isForbidden() boolean
        +isConflict() boolean
        +isConnectionFailure() boolean
    }
    class ConnectionMonitor {
        -boolean _online
        +reportSuccess() void
        +reportFailure() void
        +isOnline() boolean
        +subscribe(Listener listener) Unsubscribe
    }
    class SessionStore
    Error <|-- ApiError
    HttpApiClient ..|> IApiClient
    HttpApiClient "1" ..> "1" SessionStore : lê a credencial
    HttpApiClient "1" ..> "1" ConnectionMonitor : reporta o resultado
    HttpApiClient "1" ..> "0..*" ApiError : lança
```

`get`, `post`, `put` e `request` são genéricas no tipo da resposta esperada; por exemplo, `get<CompanyStatusDto>(…)` devolve `Promise<CompanyStatusDto>`. O parâmetro de tipo está omitido no diagrama.

| Elemento | Responsabilidade | Requisitos |
| --- | --- | --- |
| `HttpApiClient.buildHeaders` | Acrescenta a credencial de sessão no cabeçalho `Authorization` (`Bearer`) em todos os pedidos autenticados e o cabeçalho `X-Client-App` com `_clientApp` = `web`, exigido no início de sessão (documentação da API, secção 3.2, DAPI-02). O identificador do utilizador nunca vai na rota nem no corpo do pedido: o servidor obtém-no da credencial (backend, DC-05; documentação da API, secção 2.5). | RF038, RF083, RNF004, RNF006 |
| `HttpApiClient.upload` | Envia o ficheiro em `multipart/form-data`, num único campo `file`, com `PUT` (fotografia única: logótipo) ou `POST` (galeria e fotografias da vaga), e devolve o endereço do cabeçalho `Location` da resposta (`204` ou `201`) (documentação da API, secção 2.6, DAPI-10). | RF047, RF048, RF052 |
| `HttpApiClient.request` | Aborta o pedido ao fim de `_timeoutMs`, que é inferior a 10 segundos, e reporta a falha ao `ConnectionMonitor`; nesse caso lança um `ApiError` com `isConnectionFailure()` verdadeiro. | RNF018, P19 |
| `HttpApiClient.toApiError` | Converte a resposta de erro no `ApiErrorDto`, que é o formato de todas as respostas de erro, incluindo `401`, `403`, `404`, `413`, `415` e `500` (documentação da API, secção 4.1), e expõe o motivo por campo, para a página o apresentar junto de cada campo (W04, A04d, A09b, A10b). O nome do campo vem em camelCase e, nas listas, com o índice (`skillIds[0]`); nos carregamentos é `file`. Preenche `code` com o campo `code` do `ApiErrorDto` (secção 4.2). | RF037, RF040, RF042, RF084, RF094, RF099 a RF102 |
| `HttpApiClient.getFile` | Obtém um ficheiro protegido (fotografia, logótipo ou CV) com a credencial no cabeçalho e devolve-o como `Blob` (secção 4.3). | RNF004, RNF007 |
| `ApiError.isUnauthorized` | Código `401`. Num pedido com sessão é ERR-01 (credencial em falta, inválida ou expirada): o `SessionProvider` termina a sessão e encaminha para o início de sessão. No pedido de início de sessão é ERR-02 (credenciais erradas) e só a `LoginPage` o trata. | RNF004, RNF005, RNF009, P20 |
| `ApiError.isForbidden` | Código `403`: conta bloqueada ou suspensa, ponto de acesso errado, tipo de conta não autorizado, Empresa não aprovada ou elemento de outro titular; o tratamento depende de `code` (secção 4.2). | RF039, RF104, RNF006 |
| `ApiError.isConflict` | Código `409`: a operação não é possível no estado atual do elemento; o tratamento depende de `code` (secção 4.2). | RF058, RF117 |
| `ConnectionMonitor` | Mantém o estado da ligação ao servidor e avisa o `ConnectionBanner`. Os pedidos da consulta periódica também reportam o resultado, pelo que uma falha de ligação com a área de conversas aberta é indicada no limite do RNF018. | RNF018 |

O valor de `_timeoutMs` é fixado na configuração do repositório. Os verbos HTTP, as rotas completas e os códigos de estado de cada operação são os da documentação da API v02 e estão na tabela da secção 5.

### 4.2. Tratamento dos erros

O `ApiErrorDto` traz o identificador do erro no campo `code` (`ERR-01` a `ERR-50`; documentação da API v02, 4.1 e 4.3; PA-05 resolvido). `toApiError` copia-o para `ApiError.code`, e o tratamento de cada erro escolhe-se pelo código HTTP e por `code`, sem comparar o texto das mensagens. Quando a resposta não traz `ApiErrorDto` (por exemplo, falha de um intermediário da rede), `code` fica `null` e a página apresenta a mensagem genérica de erro. A mensagem apresentada ao utilizador é sempre a `message` do servidor.

| Erro da API | Código | Tratamento na área de gestão web | Ecrãs | Requisitos |
| --- | --- | --- | --- | --- |
| ERR-01 | 401 | O `SessionProvider` termina a sessão e encaminha para `/entrar` ou `/admin/entrar`; a consulta periódica para, porque a `ConversationsPage` é desmontada (secção 6). | Todos | RNF004, RNF005 |
| ERR-02 | 401 | Só no início de sessão: a `LoginPage` apresenta a mensagem única, sem distinguir correio inexistente de palavra-passe errada. | W01, A01 | RF038, RF083, RNF009 |
| ERR-03, ERR-04 | 403 | No início de sessão, a `LoginPage` apresenta a mensagem. Durante a sessão (conta bloqueada ou suspensa depois de iniciada), o `SessionProvider` termina a sessão e a `LoginPage` apresenta a mensagem; com a área de conversas aberta, a consulta periódica seguinte recebe o erro (documentação da API v02, 7.2). | W01, A01 | RF086, RNF004 |
| ERR-05 | 403 | Conta de Candidato na área de gestão web: a `LoginPage` apresenta a mensagem (secção 7.2, regra 5). | W01, A01 | RF038, RF083 |
| ERR-06 | 403 | Encaminhamento para `/acesso-negado`. | A11 | RF103, RF104 |
| ERR-07 | 403 | `refreshCompanyStatus`; o `RequireCompanyStatus` encaminha para `/empresa/estado`. Aplica-se também à lista de matches (documentação da API, 6.7.1). | W05, W06, W21, W26 | RF039 |
| ERR-08, ERR-20 | 403, 404 | A página apresenta a mensagem e volta a pedir a lista de origem. `ERR-20` em `getStatus` e em `getRegistration` não é erro: significa Empresa por registar (secção 5, regra 7). Num ficheiro, `ERR-20` mostra o marcador vazio (secção 4.3). | Todos | RF059, RNF006 |
| ERR-10, ERR-11 | 400 | Motivo junto de cada campo de `fields`; ERR-11 junto do campo de ficheiro. Na consulta periódica, um `after` mal formado não ocorre, porque o cliente envia o `sentAt` recebido do servidor. | W04, W06, W08, W10, W11, W22, W25, A04d, A09b, A10b | RF037, RF042, RF051, RF084, RF094, RF099 a RF102 |
| ERR-12, ERR-13 | 413, 415 | Mensagem do servidor. Na prática não ocorrem, porque o formulário verifica o formato e a dimensão antes de enviar (secção 5, regra 8). | W07, W10, W11 | RF047, RF048, RF052 |
| ERR-30 | 409 | Transição de estado já não possível (vaga, Empresa ou conta mudou entretanto): mensagem e nova leitura da lista; no A04e, «Ver lista atualizada». | W12 a W14, A04e, A05b, A07b, A08b | RF053 a RF056, RF093 a RF095, RF115, RF116 |
| ERR-34, ERR-35 | 409 | ERR-34: decisão sem abertura do perfil registada. ERR-35: o interesse já não está em espera, na decisão, na abertura do perfil ou na obtenção do CV (documentação da API, 6.6.2 a 6.6.5; UC12, E1 e E4). Em ambos: mensagem e nova leitura dos candidatos em espera. | W16, W17, W18, W20 | RF061, RF063, RF064, RF117, RF118, RNF007 |
| ERR-36, ERR-37 | 409 | A conversa passa a só de consulta (W23) e a lista de conversas é lida de novo, sem esperar pela consulta periódica seguinte. | W22, W23 | RF069, RF070, RF075 |
| ERR-38 | 409 | Diálogo do W15, com a alternativa de encerrar a vaga. | W15 | RF058, RF114 |
| ERR-39 | 409 | Mensagem do número máximo de fotografias; o contador «n/6» ou «n/5» é atualizado. | W07, W10, W11 | RF048, RF052, P13 |
| ERR-40 | 409 | `refreshCompanyStatus` e encaminhamento para `/empresa/estado`. | W03 | RF041 |
| ERR-41 | 409 | Alteração concorrente: mensagem «Atualize e tente novamente» e nova leitura do elemento. | Todos os formulários | — |
| ERR-50 | 500 | Mensagem do servidor; nada ficou gravado e a ação pode ser repetida. | Todos | RNF013 |
| Sem resposta | — | `isConnectionFailure()`: o `ConnectionBanner` avisa e a página oferece «Tentar de novo». Na consulta periódica, a falha não interrompe o ciclo: a consulta seguinte é feita 3 segundos depois e, se tiver resposta, o aviso desaparece. | A02d e todos | RNF018 |

Os erros ERR-31 a ERR-33 são exclusivos do Candidato e não ocorrem neste módulo.

### 4.3. Ficheiros protegidos

Um elemento `<img>` ou uma ligação `<a href>` do browser não envia o cabeçalho `Authorization`. Como os endereços dos ficheiros só respondem depois de verificar a credencial (backend, secção 7.2; RNF007), o cliente não os usa diretamente: obtém o conteúdo com `IApiClient.getFile` e apresenta-o através de um endereço local do browser (`URL.createObjectURL`), libertado quando o componente desaparece.

Os endereços são servidos por `GET /api/files/{kind}/{fileName}` (documentação da API v02, 6.9.1), autorizado pela credencial de sessão no cabeçalho: logótipos e fotografias da Empresa e das vagas para qualquer conta com sessão; fotografia de perfil do Candidato só para o Recrutador de uma Empresa em cuja vaga o Candidato tem um interesse registado. Um ficheiro inexistente ou sem autorização devolve `404` (ERR-20) e o `ProtectedImage` mostra o marcador vazio. O cliente usa o endereço tal como vem no DTO ou no cabeçalho `Location`, sem o construir. O CV não tem endereço: é obtido em `GET /api/recruiter/interests/{matchId}/cv` (`application/pdf`), só enquanto o interesse está em espera (RNF007).

```mermaid
classDiagram
    direction LR
    class useProtectedFile {
        <<hook>>
        +string? objectUrl
        +boolean loading
        +ApiError? error
    }
    class ProtectedImage {
        <<component>>
        +string? src
        +string alt
    }
    class IApiClient {
        <<interface>>
        +getFile(string path) Blob
    }
    ProtectedImage "1" ..> "1" useProtectedFile : src
    useProtectedFile "0..*" ..> "1" IApiClient : getFile
```

| Elemento | Responsabilidade | Ecrãs | Requisitos |
| --- | --- | --- | --- |
| `useProtectedFile` | Recebe o endereço devolvido pela API, obtém o ficheiro com a credencial (`GET /api/files/{kind}/{fileName}`) e devolve o endereço local; revoga-o ao desmontar. | W07, W10, W11, W17, W19 | RNF004, RNF007 |
| `ProtectedImage` | Apresenta fotografias e logótipos protegidos; sem endereço, ou com `404`, mostra o marcador vazio do protótipo. | W07, W10, W11, W17, W19 | RF047, RF048, RF052, RF061 |
| `CandidateEvaluationService.getCv` | Obtém o CV em PDF e abre-o num novo separador a partir do endereço local. Sem CV, o servidor devolve `404` (ERR-20); com o interesse fora de espera, `409` (ERR-35); com a vaga de outra Empresa, `403` (ERR-08). O botão «Abrir CV (PDF)» só aparece com `hasCv` verdadeiro. | W17 | RF061, RNF007 |

---

## 5. Serviços

Há um serviço por controller do backend usado pela área de gestão web, com as operações do controller em camelCase e os mesmos nomes (backend v02, secção 6). O `FilesController` não tem serviço: os seus URL chegam prontos nos DTOs e são obtidos com `IApiClient.getFile` (secção 4.3). O `NotificationsController` deixou de existir com a funcionalidade F011 e, com ele, o `NotificationService` da v01.

```mermaid
classDiagram
    direction LR
    class IApiClient {
        <<interface>>
    }
    class AuthService {
        -IApiClient _api
        +createRecruiterAccount(CreateRecruiterAccountRequest request) LoginResponse
        +login(LoginRequest request) LoginResponse
        +logout() void
        +changePassword(ChangePasswordRequest request) void
    }
    class CompanyService {
        -IApiClient _api
        +register(CompanyRegistrationRequest request) CompanyStatusDto
        +resubmit(CompanyRegistrationRequest request) CompanyStatusDto
        +getStatus() CompanyStatusDto
        +getRegistration() CompanyRegistrationDto
        +updateRegistration(CompanyRegistrationRequest request) CompanyStatusDto
        +getPage() CompanyPageDto
        +updatePage(CompanyPageRequest request) CompanyPageDto
        +uploadLogo(File file) void
        +addGalleryPhoto(File file) void
    }
    class JobService {
        -IApiClient _api
        +list() JobDto[]
        +create(JobRequest request) JobDto
        +update(string jobId, JobRequest request) JobDto
        +publish(string jobId) JobDto
        +suspend(string jobId) JobDto
        +republish(string jobId) JobDto
        +close(string jobId) JobDto
        +delete(string jobId) void
        +addPhoto(string jobId, File file) void
    }
    class CandidateEvaluationService {
        -IApiClient _api
        +listWaiting(string jobId) WaitingCandidateDto[]
        +openProfile(string matchId) CandidateFullProfileDto
        +getCv(string matchId) Blob
        +accept(string matchId) MatchDto
        +reject(string matchId) void
    }
    class MatchService {
        -IApiClient _api
        +list() MatchDto[]
    }
    class ConversationService {
        -IApiClient _api
        +list() ConversationSummaryDto[]
        +open(string matchId, string? after) MessageDto[]
        +send(string matchId, SendMessageRequest request) MessageDto
        +close(string matchId) void
    }
    AuthService "1" ..> "1" IApiClient
    CompanyService "1" ..> "1" IApiClient
    JobService "1" ..> "1" IApiClient
    CandidateEvaluationService "1" ..> "1" IApiClient
    MatchService "1" ..> "1" IApiClient
    ConversationService "1" ..> "1" IApiClient
```

```mermaid
classDiagram
    direction LR
    class IApiClient {
        <<interface>>
    }
    class AdminAccountService {
        -IApiClient _api
        +listAccounts() AccountDto[]
        +listCandidates() CandidateAdminDto[]
        +block(string userId) void
        +suspendCandidate(string candidateId) void
        +reactivate(string userId) void
    }
    class AdminCompanyService {
        -IApiClient _api
        +listPending() PendingCompanyDto[]
        +getSubmittedData(string companyId) CompanyRegistrationDto
        +approve(string companyId) void
        +reject(string companyId, RejectCompanyRequest request) void
        +suspend(string companyId) void
        +reactivate(string companyId) void
        +listCompanies() CompanyAdminDto[]
        +listPublishedJobs() PublishedJobAdminDto[]
    }
    class IndicatorService {
        -IApiClient _api
        +get(string from, string to) IndicatorsDto
    }
    class ReferenceListService {
        -IApiClient _api
        +listLocations() ReferenceItemDto[]
        +listSkills() ReferenceItemDto[]
        +listBenefits() ReferenceItemDto[]
        +addSkill(ReferenceItemRequest request) ReferenceItemDto
        +renameSkill(string skillId, ReferenceItemRequest request) ReferenceItemDto
        +addBenefit(ReferenceItemRequest request) ReferenceItemDto
        +renameBenefit(string benefitId, ReferenceItemRequest request) ReferenceItemDto
    }
    AdminAccountService "1" ..> "1" IApiClient
    AdminCompanyService "1" ..> "1" IApiClient
    IndicatorService "1" ..> "1" IApiClient
    ReferenceListService "1" ..> "1" IApiClient
```

| Serviço | Controller correspondente | Rota base | Quem usa | Casos de uso |
| --- | --- | --- | --- | --- |
| `AuthService` | `AuthController` | `/api/auth` | Recrutador e Administrador | UC01, UC02, UC07 |
| `CompanyService` | `CompanyController` | `/api/recruiter/company` | Recrutador | UC07, UC08 |
| `JobService` | `JobsController` | `/api/recruiter/jobs` | Recrutador | UC09, UC10 |
| `CandidateEvaluationService` | `CandidateEvaluationController` | `/api/recruiter` | Recrutador | UC11, UC12 |
| `MatchService` | `MatchesController` | `/api/matches` | Recrutador | UC13 |
| `ConversationService` | `ConversationsController` | `/api/conversations` | Recrutador | UC14 |
| `AdminAccountService` | `AdminAccountsController` | `/api/admin/accounts` | Administrador | UC18 |
| `AdminCompanyService` | `AdminCompaniesController` | `/api/admin/companies` | Administrador | UC16, UC17 |
| `IndicatorService` | `AdminIndicatorsController` | `/api/admin/indicators` | Administrador | UC19 |
| `ReferenceListService` | `ReferenceListsController` | `/api/reference-lists` | Recrutador (consulta) e Administrador | UC09, UC10, UC20 |

Pedido HTTP de cada operação, segundo a documentação da API v02 (secções 5 e 6; o número entre parênteses é a secção do endpoint). As rotas com `{…}` recebem o identificador como texto (`Guid`).

| Serviço | Operação | Pedido | Resposta de sucesso |
| --- | --- | --- | --- |
| `AuthService` | `createRecruiterAccount` | `POST /api/auth/recruiters` | `201` · `LoginResponse` |
| | `login` | `POST /api/auth/login` (com `X-Client-App: web`) | `200` · `LoginResponse` |
| | `logout` | `POST /api/auth/logout` | `204` |
| | `changePassword` | `PUT /api/auth/password` | `204` |
| `CompanyService` | `register` | `POST /api/recruiter/company` | `201` · `CompanyStatusDto` |
| | `resubmit` | `POST /api/recruiter/company/resubmission` | `200` · `CompanyStatusDto` |
| | `getStatus` | `GET /api/recruiter/company/status` | `200` · `CompanyStatusDto`; `404` sem Empresa |
| | `getRegistration` | `GET /api/recruiter/company` (6.4.8) | `200` · `CompanyRegistrationDto`; `404` sem Empresa |
| | `updateRegistration` | `PUT /api/recruiter/company` | `200` · `CompanyStatusDto` |
| | `getPage` | `GET /api/recruiter/company/page` (6.4.9) | `200` · `CompanyPageDto` |
| | `updatePage` | `PUT /api/recruiter/company/page` | `200` · `CompanyPageDto` |
| | `uploadLogo` | `PUT /api/recruiter/company/logo` (`multipart/form-data`) | `204` · `Location` |
| | `addGalleryPhoto` | `POST /api/recruiter/company/photos` (`multipart/form-data`) | `201` · `Location` |
| `JobService` | `list` | `GET /api/recruiter/jobs` | `200` · `JobDto[]` |
| | `create` | `POST /api/recruiter/jobs` | `201` · `JobDto` |
| | `update` | `PUT /api/recruiter/jobs/{jobId}` | `200` · `JobDto` |
| | `publish`, `suspend`, `republish`, `close` | `POST /api/recruiter/jobs/{jobId}/publish`, `…/suspend`, `…/republish`, `…/close` | `200` · `JobDto` |
| | `delete` | `DELETE /api/recruiter/jobs/{jobId}` | `204` |
| | `addPhoto` | `POST /api/recruiter/jobs/{jobId}/photos` (`multipart/form-data`) | `201` · `Location` |
| `CandidateEvaluationService` | `listWaiting` | `GET /api/recruiter/jobs/{jobId}/waiting-candidates` | `200` · `WaitingCandidateDto[]` |
| | `openProfile` | `GET /api/recruiter/interests/{matchId}/profile` | `200` · `CandidateFullProfileDto` |
| | `getCv` | `GET /api/recruiter/interests/{matchId}/cv` | `200` · `application/pdf` |
| | `accept` | `POST /api/recruiter/interests/{matchId}/accept` | `200` · `MatchDto` |
| | `reject` | `POST /api/recruiter/interests/{matchId}/reject` | `204` |
| `MatchService` | `list` | `GET /api/matches` | `200` · `MatchDto[]` |
| `ConversationService` | `list` | `GET /api/conversations` | `200` · `ConversationSummaryDto[]` |
| | `open` | `GET /api/conversations/{matchId}/messages` (6.8.2), sem `after` ao abrir a conversa; na consulta periódica, `GET /api/conversations/{matchId}/messages?after=<sentAt da última mensagem recebida>` | `200` · `MessageDto[]` (histórico completo, ou só as mensagens posteriores a `after`; lista vazia se não houver novas) |
| | `send` | `POST /api/conversations/{matchId}/messages` | `201` · `MessageDto` |
| | `close` | `POST /api/conversations/{matchId}/close` | `204` |
| `AdminAccountService` | `listAccounts` | `GET /api/admin/accounts` | `200` · `AccountDto[]` |
| | `listCandidates` | `GET /api/admin/accounts/candidates` | `200` · `CandidateAdminDto[]` |
| | `block` | `POST /api/admin/accounts/{userId}/block` | `204` |
| | `suspendCandidate` | `POST /api/admin/accounts/candidates/{candidateId}/suspend` | `204` |
| | `reactivate` | `POST /api/admin/accounts/{userId}/reactivate` | `204` |
| `AdminCompanyService` | `listPending` | `GET /api/admin/companies/pending` | `200` · `PendingCompanyDto[]` |
| | `getSubmittedData` | `GET /api/admin/companies/{companyId}/registration` | `200` · `CompanyRegistrationDto` |
| | `approve`, `suspend`, `reactivate` | `POST /api/admin/companies/{companyId}/approve`, `…/suspend`, `…/reactivate` | `204` |
| | `reject` | `POST /api/admin/companies/{companyId}/reject` | `204` |
| | `listCompanies` | `GET /api/admin/companies` | `200` · `CompanyAdminDto[]` |
| | `listPublishedJobs` | `GET /api/admin/companies/published-jobs` | `200` · `PublishedJobAdminDto[]` |
| `IndicatorService` | `get` | `GET /api/admin/indicators?from=aaaa-MM-dd&to=aaaa-MM-dd` | `200` · `IndicatorsDto` |
| `ReferenceListService` | `listLocations`, `listSkills`, `listBenefits` | `GET /api/reference-lists/locations`, `…/skills`, `…/benefits` | `200` · `ReferenceItemDto[]` |
| | `addSkill`, `addBenefit` | `POST /api/reference-lists/skills`, `…/benefits` | `201` · `ReferenceItemDto` |
| | `renameSkill`, `renameBenefit` | `PUT /api/reference-lists/skills/{skillId}`, `…/benefits/{benefitId}` | `200` · `ReferenceItemDto` |
| `IApiClient` | `getFile` | `GET /api/files/{kind}/{fileName}` (6.9.1), com o URL tal como vem no DTO ou no `Location` | `200` · `image/jpeg` ou `image/png`; `404` sem ficheiro ou sem autorização |

Regras dos serviços:

1. `AuthService.createRecruiterAccount` devolve a credencial de sessão (`LoginResponse`), como no backend: depois de criar a conta (W02), o Recrutador fica com sessão iniciada e segue para o registo da Empresa (W03).
2. `AuthService.logout` apenas confirma o pedido (`204`); o fim de sessão efetivo é o descarte da credencial no cliente, porque o servidor não guarda estado de sessão (backend, secção 6).
3. `JobService.addPhoto` exige o identificador da vaga. Na criação (W10), as fotografias escolhidas ficam em espera no formulário e só são enviadas depois de `create` devolver a vaga.
4. `CandidateEvaluationService.accept` e `reject` só são chamadas a partir da página do perfil completo, depois de `openProfile`, porque é essa chamada que regista a abertura exigida pelo RF118 (UC12, pré-condição 5). O servidor rejeita a decisão sem esse registo (UC12, E1).
5. `JobService.delete` é pedido sem verificação prévia no cliente: `JobDto.waitingCandidates` só conta os interesses em espera e não todos os interesses registados. Se a vaga tiver interesses, o servidor rejeita a eliminação com `409` (ERR-38) e a página apresenta o diálogo do W15, com a alternativa de encerrar a vaga (RF058, RF114).
6. `IndicatorService.get` envia as datas no formato `aaaa-MM-dd` (`DateOnly` no backend), como parâmetros de consulta; o servidor interpreta-as como dias completos no fuso `Europe/Lisbon` e rejeita, com `400`, a data inicial posterior à final e a data final posterior à atual (documentação da API, 6.12.1, DAPI-07). A página faz as mesmas verificações antes do pedido (A02c).
7. `CompanyService.getStatus` e `getRegistration` devolvem `404` (ERR-20) quando o Recrutador ainda não registou a Empresa (documentação da API v02, 6.4.3 e 6.4.8). O serviço converte essa resposta em `null`, que o `SessionProvider` guarda como «sem Empresa registada» (secção 7.1). `getRegistration` exige só a política `Recruiter`, e serve por isso também os ecrãs de Empresa pendente, recusada ou suspensa (W05, W06, W26); `getPage` exige `ApprovedCompany`.
8. Antes de cada carregamento, o formulário verifica o formato e a dimensão do ficheiro: logótipo JPEG ou PNG até 2 MB; fotografias da galeria e da vaga JPEG ou PNG até 5 MB; no máximo 6 fotografias na galeria e 5 por vaga (P01, P13). O servidor volta a verificar tudo, incluindo o conteúdo do ficheiro (RNF016).
9. `AdminAccountService.reactivate` recebe o identificador da conta. Na `CandidatesPage` é usado o `candidateId`, que é o mesmo identificador, porque a tabela `candidate` partilha a chave primária com `app_user` (modelo de dados v02, DM-02).
10. `ConversationService.open` é a mesma operação na abertura da conversa e na consulta periódica: sem `after`, devolve o histórico completo; com `after`, só as mensagens enviadas depois desse instante. Em ambos os casos o servidor marca como lidas as mensagens do Candidato (RF109); repetir o pedido não muda nada (documentação da API v02, 2.5 e DAPI-05). O valor de `after` é sempre o `sentAt` de uma mensagem recebida do servidor, nunca a hora do browser, para que a diferença entre os relógios não faça perder mensagens.
11. As operações de `ConversationService` não são chamadas pelo Administrador: a área de supervisão não tem conversas e o servidor rejeita o Administrador com `403` (ERR-06) (RF103).

---

## 6. Consulta periódica das conversas

A Declaração de Âmbito v04 substitui a ligação bidirecional persistente pela consulta periódica do cliente ao servidor (F010) e retira as notificações (limites transversais). Enquanto a lista de conversas ou uma conversa está aberta na área de gestão web, o cliente repete, de 3 em 3 segundos, os mesmos pedidos que usa para abrir a área de conversas (documentação da API v02, secção 7; backend v02, secção 6.1 e DC-07; arquitetura v02, AD-02; ata de 2026-10-16, deliberação 1). Não há biblioteca de comunicação em tempo real nem pedidos específicos para a consulta periódica. Só o Recrutador usa esta secção: a área de supervisão não tem conversas (RF103).

Na área de gestão web, a lista de conversas e a conversa escolhida estão no mesmo ecrã (W22, W23). Por isso, com a `ConversationsPage` aberta há sempre uma consulta da lista e, com uma conversa escolhida, também uma consulta das mensagens dessa conversa.

```mermaid
classDiagram
    direction LR
    class IPollingScheduler {
        <<interface>>
        +schedule(number delayMs, Task task) Cancel
    }
    class TimeoutPollingScheduler {
        +schedule(number delayMs, Task task) Cancel
    }
    class PollingLoop {
        -IPollingScheduler _scheduler
        -number _intervalMs
        -Task _task
        -boolean _running
        -Cancel? _pending
        +start() void
        +stop() void
        +isRunning() boolean
        -tick() void
    }
    class pollingConfig {
        <<constant>>
        +number conversationsIntervalMs
    }
    class ConversationsProvider {
        <<context>>
        +ConversationSummaryDto[] conversations
        +number unreadTotal
        +string? openMatchId
        +MessageDto[] messages
        +watchList(boolean active) void
        +select(string? matchId) void
        +send(string content) void
        +close() void
        +reload() void
        -pollList() void
        -pollMessages() void
        -merge(MessageDto[] received) void
    }
    class ConversationService {
        +list() ConversationSummaryDto[]
        +open(string matchId, string? after) MessageDto[]
    }
    TimeoutPollingScheduler ..|> IPollingScheduler
    PollingLoop "1" ..> "1" IPollingScheduler : agenda a consulta seguinte
    PollingLoop ..> pollingConfig : intervalo
    ConversationsProvider "1" *-- "2" PollingLoop : lista e mensagens
    ConversationsProvider "1" ..> "1" ConversationService : list e open
```

Os pedidos da consulta periódica passam pelo `HttpApiClient`, como todos os outros: levam a credencial de sessão no cabeçalho, têm o mesmo tempo limite e reportam o resultado ao `ConnectionMonitor` (secção 4.1).

| Elemento | Responsabilidade | Requisitos |
| --- | --- | --- |
| `IPollingScheduler` e `TimeoutPollingScheduler` | Agendam uma tarefa (`Task`, tipo `() => Promise<void>`) para daqui a `delayMs` milissegundos e devolvem a função que a cancela (`Cancel`, tipo `() => void`). A implementação usa `window.setTimeout`. Nos testes Jest é substituída por temporizadores simulados, para verificar o intervalo sem esperar (RI, secção 11.5). | P17 |
| `PollingLoop` | Repete uma tarefa assíncrona: executa-a, espera pela resposta e só então agenda a execução seguinte para `_intervalMs` depois (regra 2). `stop()` cancela a execução agendada e descarta a resposta de um pedido ainda em curso. | RF073, P17 |
| `pollingConfig.conversationsIntervalMs` | Intervalo da consulta periódica: 3000 ms (P17; deliberação 1 da ata de 2026-10-16). Fixado em `src/polling/config.ts`, num único sítio. | P17 |
| `ConversationsProvider` | Contexto montado na `RecruiterShell` com o estado da área de conversas: lista, total de mensagens por ler (contador da barra lateral), conversa escolhida e histórico. Tem dois `PollingLoop`: o da lista (`pollList` → `list()`), ativo enquanto a `ConversationsPage` está aberta (`watchList(true)`), e o das mensagens (`pollMessages` → `open(matchId, after)`), ativo enquanto há uma conversa escolhida nessa página. O `useConversations` lê este contexto (secção 9). | RF071, RF072, RF073, RF074, RF075, RF109 |

Pedidos repetidos (documentação da API v02, 7.1):

| Situação na área de gestão web | Pedido repetido de 3 em 3 segundos | Efeito no cliente | Requisitos |
| --- | --- | --- | --- |
| `ConversationsPage` aberta (W22, W23) | `ConversationService.list()` → `GET /api/conversations` | Substitui a lista: atualiza `unreadCount` de cada conversa, a ordem por `lastMessageAt`, o estado (`status`, `closeReason`, `closedBy`) e o total `unreadTotal` da barra lateral. Uma conversa que passou a `CLOSED` passa a só de consulta: aparece a faixa «Conversa encerrada. Apenas consulta.» com o motivo, e o campo de escrita, «Enviar» e «Encerrar conversa» ficam desativados (W23). | RF071, RF073, RF074, RF075; P17 |
| Conversa escolhida na `ConversationsPage` | `ConversationService.open(matchId, after)` → `GET /api/conversations/{matchId}/messages?after=<sentAt da última mensagem recebida>` | Acrescenta ao histórico as mensagens novas, sem duplicados (regra 4). O servidor marca-as como lidas (RF109), pelo que o contador dessa conversa fica a zero na consulta seguinte da lista. | RF072, RF073, RF109; P17 |

Regras do cliente:

1. **Quando consulta.** A consulta da lista começa quando a `ConversationsPage` é montada e para quando é desmontada (o Recrutador muda de página, termina a sessão ou a sessão expira). A consulta das mensagens começa quando o Recrutador escolhe uma conversa e para quando escolhe outra, volta à lista ou sai da página. Não há consulta periódica noutras páginas, nem com a sessão terminada (DA v04, F010: «enquanto a conversa ou a lista de conversas estiver aberta»; P17).
2. **Sem sobreposição.** O pedido seguinte só é agendado depois da resposta (ou da falha) do anterior, 3 segundos depois. Uma resposta lenta não acumula pedidos. Uma mensagem enviada logo a seguir a uma consulta surge, no pior caso, na consulta seguinte, 3 segundos mais a duração do pedido depois, dentro dos 5 segundos do P17, com as operações do servidor dentro dos 2 segundos do RNF001 (documentação da API v02, 7.2).
3. **Primeira consulta.** Ao escolher uma conversa, o cliente pede o histórico completo (`open` sem `after`) e guarda o `sentAt` da última mensagem. Daí em diante, cada consulta envia esse valor em `after`; uma resposta vazia não altera o histórico nem o valor de `after`.
4. **Mensagens próprias.** A mensagem enviada pelo Recrutador chega pela resposta de `send` e é acrescentada logo ao histórico; se a consulta seguinte a devolver também, é ignorada, porque o histórico é juntado por `messageId`.
5. **Mudança de estado da conversa.** O encerramento pela outra parte, o bloqueio ou a suspensão de uma conta e a suspensão da Empresa chegam na consulta seguinte da lista, pelo `status`, `closeReason` e `closedBy` (documentação da API v02, 7.2). Um `409` (ERR-36 ou ERR-37) num envio antecipa essa leitura (secção 4.2).
6. **Erros.** Um `401` (ERR-01) ou um `403` de conta bloqueada ou suspensa (ERR-03, ERR-04) termina a sessão (secção 4.2), o que desmonta a página e para a consulta. Um `403` de Empresa não aprovada não ocorre nas conversas, que só exigem `CandidateOrRecruiter`; com a Empresa suspensa, as conversas ficam em modo só de consulta e continuam visíveis (divergência P2). Uma falha de ligação é reportada ao `ConnectionMonitor`, que mostra o `ConnectionBanner`, e o ciclo continua: a consulta seguinte é feita 3 segundos depois.
7. **Contador da barra lateral.** Fora da `ConversationsPage` não há consulta periódica: o `ConversationsProvider` pede a lista uma vez quando a `RecruiterShell` é montada e de cada vez que o Recrutador abre outra página, e o contador mostra o valor dessa leitura (decisão DF-14).
8. **Sem notificações.** O cliente não gera avisos nem sons e não tem área nem contador de notificações. A informação que na v01 chegava por notificação é consultada na página correspondente: o estado do pedido de registo da Empresa em `/empresa/estado` (RF044), os novos interessados em `/candidatos?vaga=:jobId` (RF060) e os novos matches em `/matches` (RF067) (DA v04, limites transversais).

---

## 7. Sessão e autorização

### 7.1. Estado da sessão

```mermaid
classDiagram
    direction LR
    class Session {
        <<type>>
        +string token
        +string userId
        +UserType userType
        +string expiresAt
        +string email
    }
    class SessionStore {
        -Session? _session
        -Set~Listener~ _listeners
        +get() Session?
        +start(LoginResponse response, string email) void
        +clear() void
        +token() string?
        +isExpired(Date now) boolean
        +subscribe(Listener listener) Unsubscribe
    }
    class LoginArea {
        <<enumeration>>
        Recruiter
        Supervision
    }
    class SessionProvider {
        <<context>>
        +Session? session
        +CompanyStatusDto? companyStatus
        +CompanyRegistrationDto? companyRegistration
        +signIn(LoginRequest request, LoginArea area) void
        +signOut() void
        +refreshCompanyStatus() void
    }
    class useSession {
        <<hook>>
        +Session? session
        +boolean isAuthenticated
        +UserType? userType
        +CompanyStatus? companyStatus
        +signIn(LoginRequest request, LoginArea area) void
        +signOut() void
    }
    class AuthService
    class CompanyService
    SessionProvider "1" *-- "1" SessionStore
    SessionProvider "1" ..> "1" AuthService : iniciar e terminar sessão
    SessionProvider "1" ..> "1" CompanyService : estado e dados da Empresa
    useSession "0..*" ..> "1" SessionProvider : lê o contexto
```

| Elemento | Responsabilidade | Requisitos |
| --- | --- | --- |
| `SessionStore.start` | Guarda a credencial, o identificador, o tipo de conta e a data de expiração devolvidos pelo servidor, e o endereço de correio eletrónico indicado no início de sessão (apresentado no W25). | RF038, RF083 |
| `SessionStore.isExpired` | Compara a data de expiração com o instante atual; a credencial expira ao fim de 8 horas. | RNF005, P20 |
| `SessionProvider.signIn` | Inicia sessão e encaminha conforme a área de entrada e o tipo de conta (secção 7.2). Para o Recrutador, obtém o estado e os dados da Empresa. | RF038, RF083, RF104 |
| `SessionProvider.signOut` | Pede `logout`, descarta a credencial, limpa o estado e encaminha para o início de sessão da área respetiva. A consulta periódica para, porque a `RecruiterShell` e a `ConversationsPage` são desmontadas (documentação da API v02, 6.1.4). | RF106, RF107 |
| `SessionProvider.companyStatus` | Estado da Empresa do Recrutador, `null` quando ainda não há Empresa registada (`getStatus` com `404`; secção 5, regra 7). Obtido no início de sessão e atualizado depois de cada submissão, quando o Recrutador abre `/empresa/estado` ou escolhe «Atualizar estado» (`refreshCompanyStatus`), e quando um pedido devolve ERR-07 ou ERR-40. Não há notificação da decisão do Administrador: o Recrutador conhece-a pela consulta do estado do pedido (DA v04, F003; RF044). | RF039, RF043, RF044 |
| `SessionProvider.companyRegistration` | Dados de registo da Empresa (`getRegistration`, documentação da API v02, 6.4.8), usados na barra superior (designação social e nome do responsável), no W05, W06, W08 e W26, e para pré-preencher a localidade dos formulários (W06, W08, W10). | RF044, RF045, RF049, RF050 |

O estado da Empresa guardado no cliente serve apenas para escolher o que mostrar. A restrição é aplicada no servidor em cada operação reservada, pela política `ApprovedCompany` (backend, secção 6; RF039, RNF006): um estado desatualizado no cliente não permite nenhuma operação indevida, porque o pedido é rejeitado e o cliente volta a pedir o estado.

A credencial é guardada em memória no `SessionStore` e espelhada no `sessionStorage` do browser (decisão DF-05).

### 7.2. Rotas protegidas

```mermaid
classDiagram
    direction LR
    class AppRouter {
        <<component>>
        +routes() RouteObject[]
    }
    class PublicOnly {
        <<component>>
        +ReactNode children
    }
    class RequireSession {
        <<component>>
        +ReactNode children
    }
    class RequireUserType {
        <<component>>
        +UserType[] allowed
        +ReactNode children
    }
    class RequireCompanyStatus {
        <<component>>
        +CompanyStatus?[] allowed
        +ReactNode children
    }
    class useSession {
        <<hook>>
    }
    AppRouter "1" *-- "1..*" PublicOnly
    AppRouter "1" *-- "1..*" RequireSession
    RequireSession "1" *-- "0..*" RequireUserType
    RequireUserType "1" *-- "0..*" RequireCompanyStatus
    PublicOnly "1" ..> "1" useSession
    RequireSession "1" ..> "1" useSession
    RequireUserType "1" ..> "1" useSession
    RequireCompanyStatus "1" ..> "1" useSession
```

| Rota | Proteção | Página | Ecrãs do protótipo |
| --- | --- | --- | --- |
| `/entrar` | Pública | `LoginPage` (área do Recrutador) | W01 |
| `/criar-conta` | Pública | `CreateRecruiterAccountPage` | W02 |
| `/admin/entrar` | Pública | `LoginPage` (área de supervisão) | A01, A01b |
| `/empresa/registo` | Recrutador sem Empresa registada | `CompanyRegistrationPage` | W03, W04 |
| `/empresa/estado` | Recrutador com Empresa pendente, recusada ou suspensa | `CompanyRequestStatusPage` | W05, W06, W26 |
| `/empresa` | Recrutador com Empresa aprovada | `CompanyProfilePage` | W07, W08 |
| `/vagas` | Recrutador com Empresa aprovada | `JobListPage` | W09, W12, W13, W14, W15 |
| `/vagas/nova` | Recrutador com Empresa aprovada | `JobFormPage` | W10 |
| `/vagas/:jobId/editar` | Recrutador com Empresa aprovada | `JobFormPage` | W11 |
| `/candidatos?vaga=:jobId` | Recrutador com Empresa aprovada | `WaitingCandidatesPage` | W16 |
| `/candidatos/:matchId` | Recrutador com Empresa aprovada | `CandidateProfilePage` | W17, W18, W20 |
| `/candidatos/:matchId/match` | Recrutador com Empresa aprovada | `MatchConfirmedPage` | W19 |
| `/matches` | Recrutador com Empresa aprovada | `MatchesPage` | W21 |
| `/conversas?match=:matchId` | Recrutador com Empresa aprovada ou suspensa | `ConversationsPage` | W22, W23 |
| `/conta` | Recrutador | `AccountPage` | W25 |
| `/admin/indicadores` | Administrador | `IndicatorsPage` | A02, A02c, A02d |
| `/admin/pendentes` | Administrador | `PendingCompaniesPage` | A03, A03b, A03c, A03d |
| `/admin/pendentes/:companyId` | Administrador | `PendingCompanyDetailPage` | A04, A04b, A04c, A04d, A04e |
| `/admin/empresas` | Administrador | `CompaniesPage` | A05, A05b |
| `/admin/vagas` | Administrador | `PublishedJobsPage` | A06 |
| `/admin/contas` | Administrador | `AccountsPage` | A07, A07b, A07c, A07d |
| `/admin/candidatos` | Administrador | `CandidatesPage` | A08, A08b |
| `/admin/listas` | Administrador | `ReferenceListsPage` | A09, A09b |
| `/admin/conta` | Administrador | `ChangePasswordPage` | A10, A10b |
| `/acesso-negado` | Sessão iniciada | `AccessDeniedPage` | A11 |

O ecrã A02b é o `UserMenu` da barra superior da área de supervisão e está disponível em todas as rotas `/admin/...` com sessão (secção 8.1).

Regras de encaminhamento:

1. `PublicOnly` encaminha quem já tem sessão para a página inicial do seu tipo de conta. `RequireSession` encaminha quem não tem sessão, ou tem a credencial expirada, para `/entrar` ou `/admin/entrar`, conforme a rota pedida (RNF004, RNF005).
2. Depois do início de sessão em `/entrar`: Recrutador → página inicial do Recrutador (regra 4); Administrador → `/admin/indicadores`. Depois do início de sessão em `/admin/entrar`: Administrador → `/admin/indicadores`; Recrutador → `/acesso-negado` (A01, navegação «A11 (conta de Recrutador)»).
3. `RequireUserType`: um Recrutador que abra uma rota `/admin/...` vai para `/acesso-negado` (A11), e não para uma página em branco (RF104); um Administrador que abra uma rota do Recrutador vai para `/admin/indicadores`. O servidor rejeita sempre o pedido não autorizado (RNF006).
4. `RequireCompanyStatus` admite `null` em `allowed`, que representa «sem Empresa registada» (rota `/empresa/registo`). Página inicial do Recrutador: sem Empresa registada → `/empresa/registo`; Empresa pendente, recusada ou suspensa → `/empresa/estado`; Empresa aprovada → `/vagas` (RF039).
5. Uma conta de Candidato não usa a área de gestão web: o cliente envia `X-Client-App: web` e o servidor rejeita a conta de Candidato com `403` (ERR-05), depois de verificar as credenciais e o estado da conta (documentação da API, secções 3.2 e 6.1.3); a página apresenta o motivo devolvido pelo servidor. O texto «Os Candidatos utilizam a aplicação móvel» é uma nota fixa do ecrã A01, e não a mensagem de rejeição.
6. Nenhuma destas proteções é uma medida de segurança: são uma conveniência de navegação. A autorização é verificada no servidor em cada pedido (RNF006).

---

## 8. Páginas e componentes

### 8.1. Estrutura comum

O protótipo tem duas molduras diferentes: a do Recrutador (barra lateral com contadores e barra superior com a designação social e o nome do responsável) e a da área de supervisão (barra lateral por secções, barra superior com o caminho do ecrã e menu do utilizador).

```mermaid
classDiagram
    direction LR
    class RecruiterShell {
        <<component>>
        +ReactNode children
        +boolean locked
    }
    class ConversationsProvider {
        <<context>>
        +number unreadTotal
    }
    class AdminShell {
        <<component>>
        +ReactNode children
        +string[] breadcrumb
    }
    class Sidebar {
        <<component>>
        +NavItem[] items
        +string activeKey
    }
    class NavItem {
        <<type>>
        +string key
        +string label
        +string to
        +string? section
        +number? badge
        +boolean locked
    }
    class RecruiterTopBar {
        <<component>>
        +string companyName
        +string responsibleName
    }
    class AdminTopBar {
        <<component>>
        +string[] breadcrumb
        +string email
    }
    class UserMenu {
        <<component>>
        +string displayName
        +string email
        +string sessionStartedAt
        +onChangePassword() void
        +onSignOut() void
    }
    class ConnectionBanner {
        <<component>>
        +boolean online
        +onRetry() void
    }
    RecruiterShell "1" *-- "1" Sidebar
    RecruiterShell "1" *-- "1" RecruiterTopBar
    RecruiterShell "1" *-- "0..1" ConnectionBanner
    RecruiterShell "1" *-- "1" ConversationsProvider : contador de mensagens por ler
    AdminShell "1" *-- "1" Sidebar
    AdminShell "1" *-- "1" AdminTopBar
    AdminShell "1" *-- "0..1" ConnectionBanner
    AdminTopBar "1" *-- "1" UserMenu
    Sidebar "1" *-- "1..*" NavItem
```

```mermaid
classDiagram
    direction LR
    class ConfirmDialog {
        <<component>>
        +string title
        +string[] consequences
        +string confirmLabel
        +string cancelLabel
        +onConfirm() void
        +onCancel() void
    }
    class ReasonDialog {
        <<component>>
        +string title
        +number minLength
        +number maxLength
        +string? error
        +onConfirm(string reason) void
        +onCancel() void
    }
    class DataTable {
        <<component>>
        +Column[] columns
        +unknown[] rows
        +string emptyMessage
        +Filter[] filters
        +number? pageSize
    }
    class EmptyState {
        <<component>>
        +string message
        +string? actionLabel
        +onAction() void
    }
    class FormField {
        <<component>>
        +string label
        +string? hint
        +string? error
        +number? maxLength
        +ReactNode children
    }
    class FileUpload {
        <<component>>
        +string accept
        +number maxSizeMb
        +number? maxFiles
        +number currentFiles
        +string? error
        +onSelect(File file) void
    }
    class StatusBadge {
        <<component>>
        +BadgeStatus status
    }
    class SkillTag {
        <<component>>
        +string name
        +boolean highlighted
    }
    class ProtectedImage {
        <<component>>
        +string? src
        +string alt
    }
    class BadgeStatus {
        <<type>>
    }
    class useProtectedFile {
        <<hook>>
    }
    StatusBadge "1" ..> "1" BadgeStatus : status
    ProtectedImage "1" ..> "1" useProtectedFile : src
    FormField "1" *-- "0..1" FileUpload : children
```

Os componentes partilhados não dependem uns dos outros, exceto o `FileUpload`, que é apresentado dentro de um `FormField` (rótulo, ajuda e erro do ficheiro). Só o `ProtectedImage` usa um hook (secção 2.1). As páginas que usam cada componente estão na secção 8.4.

| Componente | Responsabilidade | Ecrãs | Requisitos |
| --- | --- | --- | --- |
| `RecruiterShell` | Moldura do Recrutador. Com a Empresa não aprovada (`locked`), mostra Vagas e Candidatos com cadeado e o item «Estado do pedido»; Matches fica com cadeado sempre que a Empresa não está aprovada (documentação da API v02, 6.7.1); Conversas fica com cadeado nos estados pendente e recusada; Conta fica sempre acessível (divergências P1 e P2 da secção 12). Não tem item nem contador de notificações. Contém o `ConversationsProvider` (secção 6). | W05 a W23, W25, W26 | RF039, RF075, RF106 |
| `AdminShell` | Moldura da área de supervisão: Indicadores; Empresas (Pendentes de aprovação, com o número de pendentes; Todas as Empresas; Vagas publicadas); Utilizadores (Contas de utilizador; Candidatos); Configuração (Listas pré-definidas). Não tem conversas nem notificações. | A02 a A10 | RF103, RF104 |
| `Sidebar` | Navegação, com o item ativo em destaque e os contadores: conversas (soma das mensagens por ler, `ConversationsProvider.unreadTotal`), no Recrutador; Empresas pendentes, no Administrador. | W05 a W23, W25, W26, A02 a A10 | RF071, RF091 |
| `RecruiterTopBar` | Designação social e nome do responsável da Empresa (`SessionProvider.companyRegistration`). | W05 a W23, W25, W26 | RF044 |
| `AdminTopBar`, `UserMenu` | Caminho do ecrã e menu do utilizador com «Alterar palavra-passe» e «Terminar sessão»; fecha ao clicar fora. Nenhum DTO tem o nome do Administrador: `displayName` é o texto fixo «Administrador Katch» do A02b; `sessionStartedAt` é calculado como `expiresAt` menos 8 horas (P20). | A02 a A10, A02b | RF084, RF107 |
| `ConnectionBanner` | Aviso «Sem ligação ao servidor», com «Tentar de novo», que repete o último pedido falhado. Na área de conversas, também aparece quando falha uma consulta periódica e desaparece na primeira que obtém resposta. | A02d e qualquer ecrã | RNF018, P19 |
| `ConfirmDialog` | Confirmação antes de uma ação, com a lista dos efeitos; também usado para informar uma rejeição com ação alternativa (W15: «Encerrar vaga» ou «Voltar»). | W12, W13, W14, W15, W18, W20, A04b, A05b, A07b, A07c, A08b | RF053, RF055, RF056, RF058, RF063, RF064, RF093, RF095, RF086, RF087, RF089, RF117 |
| `ReasonDialog` | Pedido do motivo da recusa do registo, com contador de 10 a 500 caracteres e a mensagem de motivo inválido. | A04c, A04d | RF094, P12 |
| `DataTable` | Tabela com cabeçalho, linhas e mensagem de lista vazia. Os filtros, a pesquisa e a paginação, quando existem, são aplicados no cliente sobre a lista completa devolvida pela API: filtro por estado (A05), pesquisa por correio eletrónico e filtros por tipo e estado (A07), paginação de 12 linhas com «Anterior» e «Seguinte» (A06 e A07). A lista de Candidatos (A08) não é paginada e indica o total e o número de ativos. A lista de candidatos em espera não tem filtros, classificação nem ações de decisão (W16). | W09, W16, W21, A03, A05 a A08 | RF053, RF060, RF067, RF085, RF088, RF091, RF097, RF098 |
| `EmptyState` | Lista vazia, com ação opcional (A03d: acesso a todas as Empresas). | A03d e listas vazias | RF091 |
| `FormField`, `FileUpload` | Rótulo, ajuda, contador de caracteres e mensagem de erro por campo; escolha de ficheiro com o formato, a dimensão e o número máximo de ficheiros. | W02 a W04, W06 a W08, W10, W11, W22, W25, A09, A10 | RF037, RF042, RF046 a RF048, RF050 a RF052, RF069; P01, P03, P08, P13 |
| `StatusBadge` | Estado da Empresa, da vaga ou da conta, com a designação do mapa de apresentação. | W05, W06, W08, W09, W21, W26, A04, A05, A07, A08 | RF044, RF053, RF085, RF097 |
| `SkillTag` | Competência em etiqueta, em destaque quando coincide com as pretendidas na vaga. | W10, W11, W17 | RF050, RF062 |
| `ProtectedImage` | Fotografia ou logótipo protegido (secção 4.3). | W07, W10, W11, W17, W19 | RF047, RF048, RF052, RF061 |

`BadgeStatus` é a união de `CompanyStatus`, `JobStatus` e `AccountStatus`. Todos os ecrãs são desenhados sem deslocamento horizontal entre 1280 e 1920 píxeis de largura (RNF012, P21).

### 8.2. Páginas do Recrutador

```mermaid
classDiagram
    direction LR
    class LoginPage {
        <<page>>
        +LoginArea area
        +submit(LoginRequest request) void
        +goToCreateAccount() void
    }
    class CreateRecruiterAccountPage {
        <<page>>
        +submit(CreateRecruiterAccountRequest request) void
        +goToLogin() void
    }
    class CompanyRegistrationPage {
        <<page>>
        +submit(CompanyRegistrationRequest request) void
    }
    class CompanyRequestStatusPage {
        <<page>>
        +resubmit(CompanyRegistrationRequest request) void
        +refresh() void
    }
    class CompanyProfilePage {
        <<page>>
        +selectTab(string tab) void
        +savePage(CompanyPageRequest request) void
        +uploadLogo(File file) void
        +addGalleryPhoto(File file) void
        +saveRegistration(CompanyRegistrationRequest request) void
    }
    class JobListPage {
        <<page>>
        +openCreate() void
        +openEdit(string jobId) void
        +publish(string jobId) void
        +suspend(string jobId) void
        +republish(string jobId) void
        +close(string jobId) void
        +delete(string jobId) void
        +goToWaitingCandidates(string jobId) void
    }
    class JobFormPage {
        <<page>>
        +string? jobId
        +save(JobRequest request) void
        +addPhoto(File file) void
        +cancel() void
    }
    class WaitingCandidatesPage {
        <<page>>
        +selectJob(string jobId) void
        +openProfile(string matchId) void
    }
    class CandidateProfilePage {
        <<page>>
        +string matchId
        +openCv() void
        +accept() void
        +reject() void
        +back() void
    }
    class MatchConfirmedPage {
        <<page>>
        +openConversation() void
        +goToMatches() void
        +backToCandidates() void
    }
    class MatchesPage {
        <<page>>
        +openConversation(string matchId) void
    }
    class ConversationsPage {
        <<page>>
        +selectConversation(string matchId) void
        +send(string content) void
        +closeConversation() void
    }
    class AccountPage {
        <<page>>
        +changePassword(ChangePasswordRequest request) void
        +signOut() void
    }
    class RecruiterShell {
        <<component>>
    }
    RecruiterShell "1" *-- "0..1" CompanyRequestStatusPage
    RecruiterShell "1" *-- "0..1" CompanyProfilePage
    RecruiterShell "1" *-- "0..1" JobListPage
    RecruiterShell "1" *-- "0..1" JobFormPage
    RecruiterShell "1" *-- "0..1" WaitingCandidatesPage
    RecruiterShell "1" *-- "0..1" CandidateProfilePage
    RecruiterShell "1" *-- "0..1" MatchConfirmedPage
    RecruiterShell "1" *-- "0..1" MatchesPage
    RecruiterShell "1" *-- "0..1" ConversationsPage
    RecruiterShell "1" *-- "0..1" AccountPage
    LoginPage "1" ..> "0..1" CreateRecruiterAccountPage : criar conta
    CreateRecruiterAccountPage "1" ..> "0..1" CompanyRegistrationPage : conta criada
    CompanyRegistrationPage "1" ..> "0..1" CompanyRequestStatusPage : registo pendente
    JobListPage "1" ..> "0..1" JobFormPage : criar ou editar
    JobListPage "1" ..> "0..1" WaitingCandidatesPage : ver candidatos
    WaitingCandidatesPage "1" ..> "0..1" CandidateProfilePage : abrir perfil
    CandidateProfilePage "1" ..> "0..1" MatchConfirmedPage : aceitar
    MatchConfirmedPage "1" ..> "0..1" ConversationsPage : abrir conversa
    MatchesPage "1" ..> "0..1" ConversationsPage : abrir conversa
```

A `LoginPage`, a `CreateRecruiterAccountPage` e a `CompanyRegistrationPage` não têm moldura (W01 a W04). As restantes páginas do Recrutador são apresentadas dentro da `RecruiterShell`, uma de cada vez.

| Página | Responsabilidade | Hook | Serviço | Requisitos |
| --- | --- | --- | --- | --- |
| `LoginPage` | Início de sessão na área do Recrutador (W01, com «Criar conta») ou na área de supervisão (A01). Credenciais erradas: mensagem única do servidor (ERR-02), que não distingue endereço inexistente de palavra-passe errada; conta bloqueada ou suspensa: mensagem própria (ERR-03, ERR-04; W01); conta de Candidato: ERR-05 (secção 4.2). | `useSession` | `AuthService` | RF038, RF083, RF086, RNF009 |
| `CreateRecruiterAccountPage` | Criação da conta de Recrutador com correio eletrónico, palavra-passe e aceitação das condições; «Já tenho conta» volta ao W01. | `useSession` | `AuthService` | RF037 |
| `CompanyRegistrationPage` | Primeiro registo da Empresa com os oito dados e o erro por campo (W04). | `useCompany` | `CompanyService`, `ReferenceListService` | RF041, RF042, RF043 |
| `CompanyRequestStatusPage` | Estado do pedido: pendente, com os dados submetidos só de leitura (W05); recusada, com o motivo e o formulário preenchido para corrigir e «Submeter novamente» (W06); suspensa, com os dados só de leitura (W26). Indica sempre que não pode publicar vagas nem aceder a candidatos. É aqui que o Recrutador conhece a decisão do Administrador: ao abrir a página e com «Atualizar estado» (`refresh`), o estado é pedido de novo (`getStatus`); se a Empresa tiver sido aprovada, a proteção da rota encaminha para `/empresa` (secção 7.2, regra 4). Não há notificação nem consulta periódica do estado (DA v04, F003). | `useCompany`, `useSession` | `CompanyService`, `ReferenceListService` | RF039, RF043, RF044, RF045 |
| `CompanyProfilePage` | Separador «Página da Empresa»: descrição (contador até 1000), sítio na Internet, logótipo e galeria com contador «n/6» (W07). Separador «Dados da Empresa»: dados de registo com o NIF bloqueado e o estado «Aprovada» (W08). | `useCompany` | `CompanyService`, `ReferenceListService` | RF046 a RF049 |
| `JobListPage` | Vagas com função, estado, candidatos em espera, data-limite e as ações de cada estado: Publicada (Editar, Suspender, Encerrar, Eliminar); Não publicada (Editar, Publicar, Eliminar); Suspensa (Editar, Voltar a publicar, Encerrar); Encerrada (sem ações). Cada ação passa por `ConfirmDialog` (W12 a W14); a eliminação rejeitada abre o W15. | `useJobs` | `JobService` | RF053 a RF059, RF114, RF115 |
| `JobFormPage` | Criação e alteração da vaga: campos obrigatórios e opcionais, localidade proposta com a da Empresa, competências e benefícios das listas, até 5 fotografias; a vaga é gravada como não publicada. | `useJobs`, `useReferenceLists` | `JobService`, `ReferenceListService` | RF050, RF051, RF052, RF054 |
| `WaitingCandidatesPage` | Vaga escolhida e candidatos em espera, com função pretendida, localidade e data do interesse (`WaitingCandidateDto.desiredRole`, `locationName` e `interestAt`); a única ação é «Abrir perfil». É aqui que o Recrutador conhece os novos interessados de cada vaga, sem aviso (DA v04, F007); a lista de vagas (W09) indica o número de candidatos em espera de cada vaga (`JobDto.waitingCandidates`). | `useWaitingCandidates`, `useJobs` | `CandidateEvaluationService`, `JobService` | RF060 |
| `CandidateProfilePage` | Perfil completo, competências coincidentes em destaque, hiperligações, «Abrir CV (PDF)» e a indicação da abertura registada («Abertura do perfil registada hoje às 10:32», a partir de `CandidateFullProfileDto.profileOpenedAt`); «Aceitar» e «Recusar» com confirmação (W18, W20). | `useCandidateProfile` | `CandidateEvaluationService` | RF061 a RF064, RF117, RF118, RNF007 |
| `MatchConfirmedPage` | Match confirmado com a fotografia, o nome, a vaga e os contactos do Candidato; «Abrir conversa», «Ver matches» e «Voltar aos candidatos». | `useCandidateProfile` | `CandidateEvaluationService` | RF066, RF067 |
| `MatchesPage` | Matches da Empresa com Candidato, vaga, localidade e estado da vaga (`MatchDto.jobStatus`), contactos e «Abrir conversa», incluindo vagas encerradas. É aqui que o Recrutador consulta os matches confirmados, sem aviso (DA v04, F008). | `useMatches` | `MatchService` | RF067 |
| `ConversationsPage` | Lista de conversas com as mensagens por ler e a indicação «Encerrada» (por si, pelo Candidato ou automaticamente, a partir de `closeReason` e `closedBy`); conversa aberta com texto, data e hora e indicação «Lida»; envio com contador até 1000; encerramento; modo só de consulta (W23). Abrir a conversa marca como lidas as mensagens do Candidato. Enquanto está aberta, a lista e a conversa escolhida são atualizadas por consulta periódica de 3 em 3 segundos (`watchList(true)` ao montar e `watchList(false)` ao desmontar; secção 6). | `useConversations` | `ConversationService` | RF068 a RF075, RF109 |
| `AccountPage` | Nome do responsável e correio eletrónico da conta, alteração da palavra-passe e «Terminar sessão». | `useSession` | `AuthService` | RF040, RF106 |

A `MatchConfirmedPage` recebe o `MatchDto` devolvido por `accept`. Como a API não tem leitura de um match isolado, ao recarregar a página o cliente encaminha para `/matches`. A conversa do match já existe quando a página é apresentada (RF068): «Abrir conversa» abre `/conversas?match=:matchId`.

A área de gestão web não tem área de notificações nem ecrã W24 (protótipo v02). A `NotificationsPage`, a rota `/notificacoes`, o `useNotifications` e o `NotificationService` da v01 foram retirados.

### 8.3. Páginas do Administrador

```mermaid
classDiagram
    direction LR
    class IndicatorsPage {
        <<page>>
        +applyPeriod(string from, string to) void
        +retry() void
        +goToPending() void
    }
    class PendingCompaniesPage {
        <<page>>
        +openCompany(string companyId) void
        +goToCompanies() void
    }
    class PendingCompanyDetailPage {
        <<page>>
        +string companyId
        +approve() void
        +reject(RejectCompanyRequest request) void
        +back() void
    }
    class CompaniesPage {
        <<page>>
        +filterByStatus(CompanyStatus? status) void
        +decide(string companyId) void
        +suspend(string companyId) void
        +reactivate(string companyId) void
    }
    class PublishedJobsPage {
        <<page>>
        +goToPage(number page) void
    }
    class AccountsPage {
        <<page>>
        +search(string email) void
        +filter(UserType? type, AccountStatus? status) void
        +goToPage(number page) void
        +block(string userId) void
        +reactivate(string userId) void
        +goToCandidates() void
    }
    class CandidatesPage {
        <<page>>
        +suspend(string candidateId) void
        +reactivate(string candidateId) void
        +goToAccounts() void
    }
    class ReferenceListsPage {
        <<page>>
        +addSkill(ReferenceItemRequest request) void
        +renameSkill(string skillId, ReferenceItemRequest request) void
        +addBenefit(ReferenceItemRequest request) void
        +renameBenefit(string benefitId, ReferenceItemRequest request) void
    }
    class ChangePasswordPage {
        <<page>>
        +submit(ChangePasswordRequest request) void
        +cancel() void
    }
    class AccessDeniedPage {
        <<page>>
        +signOut() void
        +goToOwnArea() void
    }
    class AdminShell {
        <<component>>
    }
    AdminShell "1" *-- "0..1" IndicatorsPage
    AdminShell "1" *-- "0..1" PendingCompaniesPage
    AdminShell "1" *-- "0..1" PendingCompanyDetailPage
    AdminShell "1" *-- "0..1" CompaniesPage
    AdminShell "1" *-- "0..1" PublishedJobsPage
    AdminShell "1" *-- "0..1" AccountsPage
    AdminShell "1" *-- "0..1" CandidatesPage
    AdminShell "1" *-- "0..1" ReferenceListsPage
    AdminShell "1" *-- "0..1" ChangePasswordPage
    PendingCompaniesPage "1" ..> "0..1" PendingCompanyDetailPage : abrir registo
    PendingCompanyDetailPage "1" ..> "1" PendingCompaniesPage : decisão registada
    CompaniesPage "1" ..> "0..1" PendingCompanyDetailPage : decidir
    AccountsPage "1" ..> "0..1" CandidatesPage : contas suspensas
    CandidatesPage "1" ..> "0..1" AccountsPage : contas bloqueadas
```

A `AccessDeniedPage` não tem moldura (A11).

| Página | Responsabilidade | Hook | Serviço | Requisitos |
| --- | --- | --- | --- | --- |
| `IndicatorsPage` | Data inicial e final, «Aplicar período» e número de dias; os três indicadores de estado na data final e os quatro de atividade no período; barras «Do interesse à conversa» calculadas no cliente com os mesmos valores. Faixa «Há n Empresa(s) a aguardar aprovação» com «Ver pendentes» (A03), apresentada quando há Empresas pendentes; o número é o mesmo do contador da barra lateral, obtido pelo `usePendingCompanies`. Rejeita o período com data inicial posterior à final (A02c); falha de ligação com «Tentar de novo» (A02d). | `useIndicators`, `usePendingCompanies` | `IndicatorService`, `AdminCompanyService` | RF091, RF096, RF103, RNF018 |
| `PendingCompaniesPage` | Empresas pendentes com designação social, NIF e data de submissão, das mais antigas para as mais recentes; mensagem depois de uma aprovação (A03b) ou de uma recusa (A03c), sem referência a aviso ao Recrutador (protótipo v02); estado vazio com acesso a todas as Empresas (A03d). | `usePendingCompanies` | `AdminCompanyService` | RF043, RF091 |
| `PendingCompanyDetailPage` | Os oito dados submetidos, data de submissão e estado; «Aprovar registo» com confirmação (A04b), que não anuncia aviso ao Recrutador (o Recrutador consulta a decisão no estado do pedido); «Recusar registo» com motivo de 10 a 500 caracteres (A04c, A04d); decisão rejeitada por a Empresa já ter sido decidida, com «Ver lista atualizada» (A04e). | `usePendingCompanies` | `AdminCompanyService` | RF092, RF093, RF094, RF112 |
| `CompaniesPage` | Todas as Empresas, com filtro por estado, designação social, estado, vagas publicadas e ações: «Decidir» (pendente), «Suspender» com confirmação (aprovada, A05b) e «Reativar» (suspensa). | `useCompaniesAdmin` | `AdminCompanyService` | RF075, RF095, RF097, RF116 |
| `PublishedJobsPage` | Vagas publicadas com função, Empresa, data de publicação e interesses, só de consulta, paginadas no cliente. | `usePublishedJobs` | `AdminCompanyService` | RF098 |
| `AccountsPage` | Contas com correio eletrónico, tipo e estado, com pesquisa, filtros e paginação de 12 linhas; «Bloquear» com confirmação (A07b) e «Reativar» uma conta bloqueada (A07c, A07d). As contas de Administrador não têm ações e as contas suspensas reativam-se em Candidatos. | `useAccounts` | `AdminAccountService` | RF075, RF085, RF086, RF087 |
| `CandidatesPage` | Candidatos com nome, correio eletrónico, localidade, data de registo e estado; «Suspender» com confirmação (A08b) e «Reativar» uma conta suspensa. | `useCandidatesAdmin` | `AdminAccountService` | RF075, RF088, RF089, RF090 |
| `ReferenceListsPage` | Competências e benefícios com «Alterar designação» na própria lista e «Acrescentar»; rejeição das designações vazias ou repetidas com o motivo (A09b). A lista de localidades não é editável e não aparece. | `useReferenceLists` | `ReferenceListService` | RF099 a RF102 |
| `ChangePasswordPage` | Palavra-passe atual e nova, com a mensagem de nova palavra-passe inválida (A10b); «Cancelar» volta ao A02. | `useSession` | `AuthService` | RF084 |
| `AccessDeniedPage` | Apresentada a uma conta que não é de Administrador e tenta abrir a área de supervisão: «Terminar sessão» ou «Ir para a área do Recrutador». | `useSession` | — | RF104, RNF006 |

A área de supervisão não tem nenhum ecrã de conversas nem acesso ao conteúdo das mensagens (RF103).

---

### 8.4. Ligações das páginas aos hooks e aos componentes

Os diagramas seguintes representam as colunas «Hook» das tabelas das secções 8.2 e 8.3 e os ecrãs da tabela de componentes da secção 8.1, com as multiplicidades. Cada página usa uma instância de cada hook; as multiplicidades dos componentes indicam quantas instâncias a página apresenta (`0..1` nos diálogos, que só aparecem quando a ação é pedida). As molduras (`RecruiterShell`, `AdminShell`) e o `ConnectionBanner` estão no primeiro diagrama da secção 8.1. A ligação dos hooks aos serviços está na secção 9.

Páginas do Recrutador e hooks:

```mermaid
classDiagram
    direction LR
    class LoginPage {
        <<page>>
    }
    class CreateRecruiterAccountPage {
        <<page>>
    }
    class CompanyRegistrationPage {
        <<page>>
    }
    class CompanyRequestStatusPage {
        <<page>>
    }
    class CompanyProfilePage {
        <<page>>
    }
    class JobListPage {
        <<page>>
    }
    class JobFormPage {
        <<page>>
    }
    class WaitingCandidatesPage {
        <<page>>
    }
    class CandidateProfilePage {
        <<page>>
    }
    class MatchConfirmedPage {
        <<page>>
    }
    class MatchesPage {
        <<page>>
    }
    class ConversationsPage {
        <<page>>
    }
    class AccountPage {
        <<page>>
    }
    class useSession {
        <<hook>>
    }
    class useCompany {
        <<hook>>
    }
    class useJobs {
        <<hook>>
    }
    class useReferenceLists {
        <<hook>>
    }
    class useWaitingCandidates {
        <<hook>>
    }
    class useCandidateProfile {
        <<hook>>
    }
    class useMatches {
        <<hook>>
    }
    class useConversations {
        <<hook>>
    }
    LoginPage "1" ..> "1" useSession
    CreateRecruiterAccountPage "1" ..> "1" useSession
    CompanyRegistrationPage "1" ..> "1" useCompany
    CompanyRequestStatusPage "1" ..> "1" useCompany
    CompanyRequestStatusPage "1" ..> "1" useSession
    CompanyProfilePage "1" ..> "1" useCompany
    JobListPage "1" ..> "1" useJobs
    JobFormPage "1" ..> "1" useJobs
    JobFormPage "1" ..> "1" useReferenceLists
    WaitingCandidatesPage "1" ..> "1" useWaitingCandidates
    WaitingCandidatesPage "1" ..> "1" useJobs
    CandidateProfilePage "1" ..> "1" useCandidateProfile
    MatchConfirmedPage "1" ..> "1" useCandidateProfile
    MatchesPage "1" ..> "1" useMatches
    ConversationsPage "1" ..> "1" useConversations
    AccountPage "1" ..> "1" useSession
```

Páginas do Recrutador e componentes partilhados (a `LoginPage` não usa nenhum):

```mermaid
classDiagram
    direction LR
    class CreateRecruiterAccountPage {
        <<page>>
    }
    class CompanyRegistrationPage {
        <<page>>
    }
    class CompanyRequestStatusPage {
        <<page>>
    }
    class CompanyProfilePage {
        <<page>>
    }
    class JobListPage {
        <<page>>
    }
    class JobFormPage {
        <<page>>
    }
    class WaitingCandidatesPage {
        <<page>>
    }
    class CandidateProfilePage {
        <<page>>
    }
    class MatchConfirmedPage {
        <<page>>
    }
    class MatchesPage {
        <<page>>
    }
    class ConversationsPage {
        <<page>>
    }
    class AccountPage {
        <<page>>
    }
    class FormField {
        <<component>>
    }
    class StatusBadge {
        <<component>>
    }
    class FileUpload {
        <<component>>
    }
    class ProtectedImage {
        <<component>>
    }
    class DataTable {
        <<component>>
    }
    class ConfirmDialog {
        <<component>>
    }
    class SkillTag {
        <<component>>
    }
    CreateRecruiterAccountPage "1" ..> "1..*" FormField
    CompanyRegistrationPage "1" ..> "1..*" FormField
    CompanyRequestStatusPage "1" ..> "1" StatusBadge
    CompanyRequestStatusPage "1" ..> "0..*" FormField
    CompanyProfilePage "1" ..> "1..*" FormField
    CompanyProfilePage "1" ..> "1..2" FileUpload
    CompanyProfilePage "1" ..> "0..*" ProtectedImage
    CompanyProfilePage "1" ..> "1" StatusBadge
    JobListPage "1" ..> "1" DataTable
    JobListPage "1" ..> "0..*" StatusBadge
    JobListPage "1" ..> "0..1" ConfirmDialog
    JobFormPage "1" ..> "1..*" FormField
    JobFormPage "1" ..> "1" FileUpload
    JobFormPage "1" ..> "0..*" SkillTag
    JobFormPage "1" ..> "0..*" ProtectedImage
    WaitingCandidatesPage "1" ..> "1" DataTable
    CandidateProfilePage "1" ..> "0..*" SkillTag
    CandidateProfilePage "1" ..> "0..1" ProtectedImage
    CandidateProfilePage "1" ..> "0..1" ConfirmDialog
    MatchConfirmedPage "1" ..> "0..1" ProtectedImage
    MatchesPage "1" ..> "1" DataTable
    MatchesPage "1" ..> "0..*" StatusBadge
    ConversationsPage "1" ..> "0..1" FormField
    AccountPage "1" ..> "1..*" FormField
```

Páginas do Administrador e hooks:

```mermaid
classDiagram
    direction LR
    class IndicatorsPage {
        <<page>>
    }
    class PendingCompaniesPage {
        <<page>>
    }
    class PendingCompanyDetailPage {
        <<page>>
    }
    class CompaniesPage {
        <<page>>
    }
    class PublishedJobsPage {
        <<page>>
    }
    class AccountsPage {
        <<page>>
    }
    class CandidatesPage {
        <<page>>
    }
    class ReferenceListsPage {
        <<page>>
    }
    class ChangePasswordPage {
        <<page>>
    }
    class AccessDeniedPage {
        <<page>>
    }
    class useIndicators {
        <<hook>>
    }
    class usePendingCompanies {
        <<hook>>
    }
    class useCompaniesAdmin {
        <<hook>>
    }
    class usePublishedJobs {
        <<hook>>
    }
    class useAccounts {
        <<hook>>
    }
    class useCandidatesAdmin {
        <<hook>>
    }
    class useReferenceLists {
        <<hook>>
    }
    class useSession {
        <<hook>>
    }
    IndicatorsPage "1" ..> "1" useIndicators
    IndicatorsPage "1" ..> "1" usePendingCompanies
    PendingCompaniesPage "1" ..> "1" usePendingCompanies
    PendingCompanyDetailPage "1" ..> "1" usePendingCompanies
    CompaniesPage "1" ..> "1" useCompaniesAdmin
    PublishedJobsPage "1" ..> "1" usePublishedJobs
    AccountsPage "1" ..> "1" useAccounts
    CandidatesPage "1" ..> "1" useCandidatesAdmin
    ReferenceListsPage "1" ..> "1" useReferenceLists
    ChangePasswordPage "1" ..> "1" useSession
    AccessDeniedPage "1" ..> "1" useSession
```

Páginas do Administrador e componentes partilhados (a `IndicatorsPage` e a `AccessDeniedPage` não usam nenhum):

```mermaid
classDiagram
    direction LR
    class PendingCompaniesPage {
        <<page>>
    }
    class PendingCompanyDetailPage {
        <<page>>
    }
    class CompaniesPage {
        <<page>>
    }
    class PublishedJobsPage {
        <<page>>
    }
    class AccountsPage {
        <<page>>
    }
    class CandidatesPage {
        <<page>>
    }
    class ReferenceListsPage {
        <<page>>
    }
    class ChangePasswordPage {
        <<page>>
    }
    class DataTable {
        <<component>>
    }
    class EmptyState {
        <<component>>
    }
    class StatusBadge {
        <<component>>
    }
    class ConfirmDialog {
        <<component>>
    }
    class ReasonDialog {
        <<component>>
    }
    class FormField {
        <<component>>
    }
    PendingCompaniesPage "1" ..> "1" DataTable
    PendingCompaniesPage "1" ..> "0..1" EmptyState
    PendingCompanyDetailPage "1" ..> "1" StatusBadge
    PendingCompanyDetailPage "1" ..> "0..1" ConfirmDialog
    PendingCompanyDetailPage "1" ..> "0..1" ReasonDialog
    CompaniesPage "1" ..> "1" DataTable
    CompaniesPage "1" ..> "0..*" StatusBadge
    CompaniesPage "1" ..> "0..1" ConfirmDialog
    PublishedJobsPage "1" ..> "1" DataTable
    AccountsPage "1" ..> "1" DataTable
    AccountsPage "1" ..> "0..*" StatusBadge
    AccountsPage "1" ..> "0..1" ConfirmDialog
    CandidatesPage "1" ..> "1" DataTable
    CandidatesPage "1" ..> "0..*" StatusBadge
    CandidatesPage "1" ..> "0..1" ConfirmDialog
    ReferenceListsPage "1" ..> "1..*" FormField
    ChangePasswordPage "1" ..> "1..*" FormField
```

O `useSession` não chama serviços: lê o `SessionProvider`, que usa o `AuthService` e o `CompanyService` (secção 7.1). O `useProtectedFile` não é usado pelas páginas, mas pelo componente `ProtectedImage` (secção 8.1).

## 9. Estado dos ecrãs

Os hooks concentram o estado de cada ecrã. Todos os hooks que obtêm dados para um ecrã realizam o contrato `IResourceState<T>`, exceto o `useProtectedFile` (secção 4.3), que serve um componente e devolve apenas o endereço local de um ficheiro, sem recurso nem lista a apresentar; para que as páginas tratem o carregamento, o erro e a lista vazia da mesma forma: `data` é o recurso principal do hook, com o tipo `T` indicado na tabela desta secção, e os atributos acrescentados por cada hook são estado complementar. Os hooks são funções: compõem-se, não se especializam.

```mermaid
classDiagram
    direction LR
    class IResourceState~T~ {
        <<interface>>
        +T? data
        +boolean loading
        +ApiError? error
        +reload() void
    }
    class ReferenceLists {
        <<type>>
        +ReferenceItemDto[] locations
        +ReferenceItemDto[] skills
        +ReferenceItemDto[] benefits
    }
    class useCompany {
        <<hook>>
        +CompanyRegistrationDto? registration
        +CompanyPageDto? page
        +register(CompanyRegistrationRequest request) void
        +resubmit(CompanyRegistrationRequest request) void
        +updateRegistration(CompanyRegistrationRequest request) void
        +updatePage(CompanyPageRequest request) void
        +uploadLogo(File file) void
        +addGalleryPhoto(File file) void
    }
    class useJobs {
        <<hook>>
        +create(JobRequest request, File[] photos) JobDto
        +update(string jobId, JobRequest request) JobDto
        +publish(string jobId) void
        +suspend(string jobId) void
        +republish(string jobId) void
        +close(string jobId) void
        +delete(string jobId) void
    }
    class useReferenceLists {
        <<hook>>
        +addSkill(ReferenceItemRequest request) void
        +renameSkill(string skillId, ReferenceItemRequest request) void
        +addBenefit(ReferenceItemRequest request) void
        +renameBenefit(string benefitId, ReferenceItemRequest request) void
    }
    class useWaitingCandidates {
        <<hook>>
        +JobDto? selectedJob
        +selectJob(string jobId) void
    }
    class useCandidateProfile {
        <<hook>>
        +boolean profileOpened
        +MatchDto? confirmedMatch
        +open(string matchId) void
        +accept() MatchDto
        +reject() void
        +openCv() void
    }
    class useMatches {
        <<hook>>
    }
    class useConversations {
        <<hook>>
        +string? openMatchId
        +MessageDto[] messages
        +number unreadTotal
        +watchList(boolean active) void
        +select(string? matchId) void
        +send(string content) void
        +close() void
    }
    class ConversationsProvider {
        <<context>>
    }
    useReferenceLists ..> ReferenceLists : data
    useCompany ..|> IResourceState
    useJobs ..|> IResourceState
    useReferenceLists ..|> IResourceState
    useWaitingCandidates ..|> IResourceState
    useCandidateProfile ..|> IResourceState
    useMatches ..|> IResourceState
    useConversations ..|> IResourceState
    useConversations "0..*" ..> "1" ConversationsProvider : lê o contexto
    class CompanyService
    class JobService
    class ReferenceListService
    class CandidateEvaluationService
    class MatchService
    class ConversationService
    useCompany "0..*" ..> "1" CompanyService
    useCompany "0..*" ..> "1" ReferenceListService : localidades
    useJobs "0..*" ..> "1" JobService
    useReferenceLists "0..*" ..> "1" ReferenceListService
    useWaitingCandidates "0..*" ..> "1" CandidateEvaluationService
    useCandidateProfile "0..*" ..> "1" CandidateEvaluationService
    useMatches "0..*" ..> "1" MatchService
    ConversationsProvider "1" ..> "1" ConversationService
```

```mermaid
classDiagram
    direction LR
    class IResourceState~T~ {
        <<interface>>
        +T? data
        +boolean loading
        +ApiError? error
        +reload() void
    }
    class useIndicators {
        <<hook>>
        +string from
        +string to
        +number periodDays
        +string? periodError
        +applyPeriod(string from, string to) void
    }
    class usePendingCompanies {
        <<hook>>
        +CompanyRegistrationDto? selected
        +string? lastDecisionMessage
        +open(string companyId) void
        +approve(string companyId) void
        +reject(string companyId, RejectCompanyRequest request) void
    }
    class useCompaniesAdmin {
        <<hook>>
        +CompanyStatus? statusFilter
        +CompanyAdminDto[] visibleRows
        +filterByStatus(CompanyStatus? status) void
        +suspend(string companyId) void
        +reactivate(string companyId) void
    }
    class usePublishedJobs {
        <<hook>>
        +number page
        +number pageCount
        +PublishedJobAdminDto[] visibleRows
        +goToPage(number page) void
    }
    class useAccounts {
        <<hook>>
        +string emailQuery
        +UserType? typeFilter
        +AccountStatus? statusFilter
        +number page
        +number pageCount
        +AccountDto[] visibleRows
        +search(string email) void
        +filter(UserType? type, AccountStatus? status) void
        +goToPage(number page) void
        +block(string userId) void
        +reactivate(string userId) void
    }
    class useCandidatesAdmin {
        <<hook>>
        +number activeCount
        +suspend(string candidateId) void
        +reactivate(string candidateId) void
    }
    useIndicators ..|> IResourceState
    usePendingCompanies ..|> IResourceState
    useCompaniesAdmin ..|> IResourceState
    usePublishedJobs ..|> IResourceState
    useAccounts ..|> IResourceState
    useCandidatesAdmin ..|> IResourceState
    class IndicatorService
    class AdminCompanyService
    class AdminAccountService
    useIndicators "0..*" ..> "1" IndicatorService
    usePendingCompanies "0..*" ..> "1" AdminCompanyService
    useCompaniesAdmin "0..*" ..> "1" AdminCompanyService
    usePublishedJobs "0..*" ..> "1" AdminCompanyService
    useAccounts "0..*" ..> "1" AdminAccountService
    useCandidatesAdmin "0..*" ..> "1" AdminAccountService
```

Cada serviço é uma instância única, partilhada por todos os hooks que o usam (`0..*` → `1`). O `useReferenceLists` do Administrador usa o mesmo `ReferenceListService` do primeiro diagrama, com as operações de acrescento e alteração.

| Hook | `data` (T) | Estado complementar | Origem das atualizações |
| --- | --- | --- | --- |
| `useSession` | — (lê o `SessionProvider`, secção 7.1) | Sessão, estado e dados da Empresa | Início e fim de sessão; `refreshCompanyStatus` |
| `useCompany` | `CompanyStatusDto` | Dados de registo e página da Empresa | Pedido à API; nova leitura depois de submeter, ao abrir a página do estado do pedido e com «Atualizar estado» (consulta da decisão do Administrador, sem notificação) |
| `useJobs` | `JobDto[]` | — | Pedido à API; nova leitura depois de cada ação de estado |
| `useReferenceLists` | `ReferenceLists` | — | Pedido à API; no Administrador, nova leitura depois de cada acrescento ou alteração |
| `useWaitingCandidates` | `WaitingCandidateDto[]` | Vaga escolhida; a sua função dá o título do W17 («nome — vaga função»), porque o `CandidateFullProfileDto` não a contém | Pedido à API ao abrir o ecrã e ao escolher outra vaga; nova leitura depois de uma decisão |
| `useCandidateProfile` | `CandidateFullProfileDto` | Indicação de abertura registada (`profileOpenedAt`) e match confirmado | Pedido à API; `accept` e `reject` só ficam disponíveis com `profileOpened` verdadeiro |
| `useMatches` | `MatchDto[]` | — | Pedido à API ao abrir o ecrã |
| `useConversations` | `ConversationSummaryDto[]` | Conversa aberta, histórico e total de mensagens por ler, lidos do `ConversationsProvider` | Pedido à API ao abrir; consulta periódica de 3 em 3 segundos da lista e da conversa escolhida enquanto a `ConversationsPage` está aberta (secção 6); nova leitura depois de enviar ou encerrar |
| `useProtectedFile` | — (endereço local, secção 4.3) | — | Pedido à API; libertado ao desmontar |
| `useIndicators` | `IndicatorsDto` | Período, número de dias e erro de período | Pedido à API depois de validar o período |
| `usePendingCompanies` | `PendingCompanyDto[]` | Dados submetidos da Empresa aberta e mensagem da última decisão (A03b, A03c) | Pedido à API; nova leitura depois de cada decisão (atualiza também o contador da barra lateral e a faixa do A02) |
| `useCompaniesAdmin` | `CompanyAdminDto[]` | Filtro por estado e linhas visíveis | Pedido à API; nova leitura depois de cada mudança de estado |
| `usePublishedJobs` | `PublishedJobAdminDto[]` | Página atual e linhas visíveis (12 por página) | Pedido à API ao abrir o ecrã |
| `useAccounts` | `AccountDto[]` | Pesquisa, filtros, página atual e linhas visíveis (12 por página) | Pedido à API; nova leitura depois de cada mudança de estado |
| `useCandidatesAdmin` | `CandidateAdminDto[]` | Número de Candidatos ativos («22 Candidatos · 20 ativos») | Pedido à API; nova leitura depois de cada mudança de estado |

O `useConversations` é o único hook que não guarda o estado em si: lê o `ConversationsProvider`, montado na `RecruiterShell`, para que a `ConversationsPage` e o contador da barra lateral partilhem a mesma lista (decisão DF-14). O contador vive por isso na `RecruiterShell`, que se mantém montada enquanto o Recrutador muda de página. Cada ecrã lê o que precisa quando é aberto; só a área de conversas é atualizada periodicamente; o servidor é a única fonte de verdade.

---

## 10. Exemplo de interação entre camadas

Aceitação de um Candidato pelo Recrutador (UC12), com as multiplicidades de cada ligação. O fluxo mostra a regra do RF118: a decisão só é possível depois de a abertura do perfil ter sido registada.

```mermaid
classDiagram
    direction LR
    class WaitingCandidatesPage {
        <<page>>
        +openProfile(string matchId) void
    }
    class CandidateProfilePage {
        <<page>>
        +accept() void
    }
    class useCandidateProfile {
        <<hook>>
        +open(string matchId) void
        +accept() MatchDto
    }
    class CandidateEvaluationService {
        +openProfile(string matchId) CandidateFullProfileDto
        +accept(string matchId) MatchDto
    }
    class HttpApiClient {
        +get(string path) T
        +post(string path, unknown body) T
    }
    class SessionStore {
        +token() string?
    }
    class MatchConfirmedPage {
        <<page>>
    }
    class ConversationsPage {
        <<page>>
    }
    WaitingCandidatesPage "1" ..> "1" CandidateProfilePage : 1 abrir perfil
    CandidateProfilePage "1" ..> "1" useCandidateProfile : 2 e 4 open e accept
    useCandidateProfile "1" ..> "1" CandidateEvaluationService : 2 e 4
    CandidateEvaluationService "1" ..> "1" HttpApiClient : 2 e 4
    HttpApiClient "1" ..> "1" SessionStore : credencial
    CandidateProfilePage "1" ..> "1" MatchConfirmedPage : 6 match confirmado
    MatchConfirmedPage "1" ..> "0..1" ConversationsPage : 7 abrir conversa
```

1. Na `WaitingCandidatesPage` (W16), o Recrutador escolhe «Abrir perfil». A lista não tem ações de decisão (RF060).
2. A `CandidateProfilePage` (W17) chama `useCandidateProfile.open`, que pede o perfil completo (`GET /api/recruiter/interests/{matchId}/profile`). O servidor regista a abertura para a vaga (RF118) e o hook passa `profileOpened` a verdadeiro.
3. Só então «Aceitar» e «Recusar» ficam disponíveis. A confirmação (W18) enumera os efeitos e avisa que a decisão não pode ser alterada (RF117).
4. `CandidateEvaluationService.accept` envia `POST /api/recruiter/interests/{matchId}/accept` com a credencial de sessão no cabeçalho; o identificador do Recrutador vem da credencial e não do corpo (RNF006).
5. O servidor executa a operação atómica (backend, secção 8) e devolve `200` com o `MatchDto`.
6. A página abre a `MatchConfirmedPage` (W19) com o contacto do Candidato e o acesso à conversa (RF066, RF067).
7. «Abrir conversa» abre a `ConversationsPage` com a conversa do match escolhida; a partir daí, a lista e a conversa são atualizadas por consulta periódica (secção 6). Não há notificação ao Recrutador nem ao Candidato: o match passa a constar da lista de matches de cada parte (`/matches` na área de gestão web e a lista de matches da aplicação móvel), que cada uma consulta (DA v04, F008).

Se o servidor rejeitar a decisão com `409` porque o Candidato já foi avaliado noutro pedido (ERR-35; UC12, E4), o `ApiError` é apresentado na página e a lista de candidatos em espera é recarregada; a decisão registada não muda (RF117).

---

## 11. Dependências do backend

Na v01, a verificação cruzada com o modelo de classes do backend (`I036`) e com os protótipos aceites (`I040`, `I041`) encontrou elementos de que a área de gestão web precisava e que o backend ainda não definia (G1 a G12). A documentação da API v02 (`I101`, `I104`) e o modelo de classes do backend v02 (`I103`) resolveram-nas todas, exceto a G6, que deixou de se aplicar com a retirada da ligação persistente. A tabela conserva a descrição da v01 e acrescenta a resolução; o modelo v02 segue as resoluções nas secções indicadas. Não há dependências por resolver nesta versão.

| ID | Dependência (v01) | Onde se manifesta | Proposta (v01) | Resolução na documentação da API v02 e neste modelo |
| --- | --- | --- | --- | --- |
| G1 | O `CompanyController` não tem operação para o Recrutador ler os dados de registo da própria Empresa (a tabela 7.2 do backend associa `CompanyRegistrationDto` ao `CompanyController`, mas nenhuma operação o devolve). | W05, W06, W08, W26, barra superior, W25 (nome do responsável); UC07 A3 («o sistema apresenta os dados submetidos e o motivo da recusa»); RF044, RF045, RF049 | `GetRegistration() ActionResult<CompanyRegistrationDto>` no `CompanyController` e `GetRegistrationAsync` no `ICompanyService`, com a política `Recruiter` e não `ApprovedCompany`, porque os ecrãs W05, W06 e W26 são de Empresa pendente, recusada ou suspensa | Resolvida (PA-03): `GET /api/recruiter/company` (6.4.8), política `Recruiter`, `404` sem Empresa. `CompanyService.getRegistration` (secção 5) |
| G2 | O `CompanyController` não tem operação para o Recrutador ler a página de apresentação da própria Empresa (só o `JobExplorationController`, reservado ao Candidato, a devolve). | W07; RF046 a RF048 | `GetPage() ActionResult<CompanyPageDto>` no `CompanyController` | Resolvida (PA-03): `GET /api/recruiter/company/page` (6.4.9), política `ApprovedCompany`. `CompanyService.getPage` (secção 5) |
| G3 | `CompanyRegistrationDto` só tem `LocationName`; o pedido de alteração exige `LocationId`. | W06 e W08 (seleção da localidade preenchida); W10 (localidade proposta com a da Empresa, RF050) | Acrescentar `Guid LocationId` ao `CompanyRegistrationDto` | Resolvida (PA-03): `CompanyRegistrationDto.locationId` (8.3). Tipo da secção 3.2 |
| G4 | `WaitingCandidateDto` não tem a função pretendida nem a localidade. | W16 (colunas «Função pretendida» e «Localidade») | Acrescentar `string? DesiredRole` e `string LocationName` | Resolvida (PA-07): `WaitingCandidateDto.desiredRole` e `locationName` (8.4). Tipo da secção 3.2; `WaitingCandidatesPage` |
| G5 | `MatchDto` não tem o estado da vaga. | W21 (coluna «Estado da vaga») | Acrescentar `JobStatus JobStatus` | Resolvida (PA-08): `MatchDto.jobStatus`, `locationName` e `logoUrl` (8.4). Tipo da secção 3.2; `MatchesPage` |
| G6 | O acontecimento `MessageReceived` envia só o `MessageDto`, que não identifica a conversa. | RF073 (acrescentar a mensagem à conversa certa) | Enviar o identificador do match no acontecimento; até lá aplica-se a regra 2 da secção 6 | Sem aplicação (PA-02): não há ligação persistente nem acontecimentos; as mensagens novas são pedidas por conversa, identificada pelo `matchId` (6.8.2 com `after`; secção 6 deste modelo) |
| G7 | `IAuthService.LoginAsync` recebe `ClientApp`, mas o contrato não define como o cliente o indica. | Rejeição do início de sessão de um Candidato na área de gestão web (A01) | Fixar na documentação da API | Resolvida: cabeçalho `X-Client-App: web` (secção 3.2, DAPI-02); aplicada na secção 4.1 |
| G8 | Não estava definida a resposta de `GetStatus` quando o Recrutador ainda não registou a Empresa. | Encaminhamento para `/empresa/registo` depois do W02 e no início de sessão | Fixar na documentação da API | Resolvida: `404` com ERR-20 (6.4.3); aplicada na secção 5, regra 7 |
| G9 | Não estava definida a serialização das enumerações em JSON. | Todos os tipos da secção 3 | Serialização das enumerações como texto | Resolvida: valores em maiúsculas com `_` (secção 2.4, DAPI-01); aplicada nas secções 2.2 e 3.1 |
| G10 | `CandidateFullProfileDto` não tem a data e hora da abertura do perfil. | W17 («Abertura do perfil registada hoje às 10:32») | Acrescentar `DateTimeOffset ProfileOpenedAt` | Resolvida (PA-09): `CandidateFullProfileDto.profileOpenedAt` (8.4). Tipo da secção 3.2; `CandidateProfilePage` |
| G11 | Não há operação que sirva os ficheiros referidos por `photoUrl`, `logoUrl` e `photoUrls`. | W07, W10, W11, W17, W19 (logótipo, galeria, fotografias da vaga e do Candidato); RF047, RF048, RF052, RF061 | A proposta da API, com autorização pela credencial de sessão no cabeçalho, para manter a secção 4.3 | Resolvida (PA-01): `GET /api/files/{kind}/{fileName}` (6.9.1), com a credencial no cabeçalho. Secção 4.3 |
| G12 | `ApiErrorDto` não tem código de erro. | Distinção dos erros `403` e `409` (secção 4.2) | Acrescentar `string Code` com os identificadores ERR-xx | Resolvida (PA-05): `ApiErrorDto.code` (4.1, 8.5). Secções 4.1 e 4.2: os erros identificam-se por `code` |

A PA-04 da documentação da API (lista de localidades sem sessão) foi resolvida pela `I101` e não afeta a área de gestão web. A PA-06 (SignalR e ratificação da AD-01) deixou de se aplicar à ligação persistente; a AD-01 foi ratificada na deliberação 3 da ata de 2026-10-16.

Os campos acrescentados pela `I104` constam da documentação da API v02, que é o contrato que o cliente segue; o modelo de classes do backend v02 só os acompanha numa versão posterior (documentação da API v02, L-08). Se a implementação do backend em `m3` divergir do contrato, prevalece a documentação da API (DAPI-11) e a divergência é tratada como defeito do backend.

---

## 12. Divergências com os protótipos

O modelo segue o protótipo v02 (`m3-prototipo-baixa-fidelidade-v02.pdf`), exceto onde contraria os requisitos ou o backend. Nesses casos prevalecem os requisitos, e a divergência fica registada para correção do protótipo numa Issue própria. O protótipo v02 só retirou o ecrã W24, o item e o contador de notificações e as frases dos ecrãs A03b, A03c e A04b que referiam um aviso ao Recrutador; as divergências P1 a P4 mantêm-se, com a P1 ajustada.

| ID | Divergência | Decisão no modelo | Fundamento |
| --- | --- | --- | --- |
| P1 | Na variante bloqueada da barra lateral (W05, W06, W26) não existe o item Conta. | Conta fica sempre acessível ao Recrutador; o item «Estado do pedido» substitui Vagas e Candidatos enquanto a Empresa não está aprovada. | RF106 (terminar sessão) e RF040 (alterar a palavra-passe) com a Empresa em qualquer estado; RF044 (consulta do estado do pedido, que substitui a notificação da decisão). Na v01, a P1 abrangia também Notificações, retiradas com a F011. |
| P2 | No W26 (Empresa suspensa), Conversas está bloqueado. | Com a Empresa suspensa, Conversas fica acessível em modo só de consulta; nos estados pendente e recusada continua bloqueado, porque não pode haver conversas. Matches fica bloqueado, como no W26, porque a lista de matches exige Empresa aprovada. | RF075 (conversas passam a só de consulta, não a inacessíveis); documentação da API, 6.7.1 (`ApprovedCompany` na lista de matches do Recrutador, RF039) e 6.8 (`CandidateOrRecruiter` nas conversas) |
| P3 | O W01 («Backoffice do Recrutador») e o A01 («Recrutadores e Administrador») são dois ecrãs de início de sessão. | Uma `LoginPage` em duas rotas (`/entrar` e `/admin/entrar`), com o encaminhamento da secção 7.2. | Mantém os dois ecrãs aceites e a navegação do A01 para o A11 com conta de Recrutador |
| P4 | Os botões tracejados «Simular: …» (W01, W03, W05, W22, W26) só servem para navegar no protótipo. | Não são modelados. | Não correspondem a nenhuma funcionalidade |

---

## 13. Decisões de desenho

| ID | Decisão | Fundamentação |
| --- | --- | --- |
| DF-01 | Separação em páginas, componentes, rotas, hooks, serviços, cliente da API, consulta periódica, sessão e tipos. | As páginas ficam sem conhecimento do protocolo e os serviços sem conhecimento da interface, o que permite testar hooks e serviços com Jest e o cliente da API simulado (RI, secção 11.5). |
| DF-02 | Um serviço por controller do backend, com os mesmos nomes de operação em camelCase. | Torna imediata a verificação da coerência entre o cliente e a API (backend, secção 6). |
| DF-03 | Os tipos do cliente espelham os DTOs, sem tipos intermédios. | Uma divergência entre o cliente e a API aparece como erro de compilação do TypeScript. |
| DF-04 | Uma página por ecrã do protótipo, com os estados e diálogos do mesmo ecrã na mesma página. | Mantém a rastreabilidade aos ecrãs aceites nas `I040` e `I041`. |
| DF-05 | A credencial é guardada em memória e espelhada no `sessionStorage`, limpo no fim de sessão e quando a credencial expira. | Recarregar a página não obriga a novo início de sessão; o `sessionStorage` é por separador e desaparece ao fechá-lo, ao contrário do `localStorage`. A credencial expira ao fim de 8 horas no servidor (RNF005, P20). O RI não fixa esta escolha. |
| DF-06 | Os ficheiros protegidos são obtidos com a credencial e apresentados por endereço local. | O browser não envia o cabeçalho `Authorization` em `<img>` e `<a href>`; o acesso aos ficheiros é verificado no servidor (RNF007; backend, secção 7.2). |
| DF-07 | As proteções de rota são de navegação, e não de segurança. | A autorização é sempre verificada no servidor (RNF006); o cliente apenas evita mostrar ecrãs inacessíveis e apresenta o ecrã de acesso negado (RF104). |
| DF-08 | As mensagens novas, o número de mensagens por ler e o estado das conversas chegam por consulta periódica à API, de 3 em 3 segundos, só enquanto a lista de conversas ou uma conversa está aberta (revista na v02; a v01 previa a ligação persistente). Não há notificações. | DA v04 (F010 e limites transversais); arquitetura v02, AD-02 (D-08 retirada); backend v02, secção 6.1 e DC-07; documentação da API v02, secção 7; P17; ata de 2026-10-16, deliberação 1. |
| DF-09 | As ações de decisão sobre um Candidato só existem na página do perfil completo e só ficam disponíveis depois de a abertura ser registada. | UC12, pré-condição 5 e E1; RF063, RF064, RF118. A mesma regra é verificada no servidor. |
| DF-10 | Filtros, pesquisa e paginação das listas da área de supervisão são aplicados no cliente. | O backend devolve as listas completas, sem parâmetros de filtragem nem de paginação (backend, secção 6); os volumes de demonstração são pequenos (P18). |
| DF-11 | As ações irreversíveis e as mudanças de estado passam por `ConfirmDialog` com a lista dos efeitos. | Ecrãs W12 a W15, W18, W20, A04b, A05b, A07b, A07c e A08b, aceites nas `I040` e `I041`; RF117. |
| DF-12 | O cliente identifica os erros pelo código HTTP e pelo campo `code` do `ApiErrorDto` (revista na v02; a v01 comparava as mensagens fixas). | A documentação da API v02 acrescentou `code` ao `ApiErrorDto` (PA-05); o texto das mensagens pode mudar sem afetar o cliente. |
| DF-13 | Retirada na v02 (destino da notificação `COMPANY_APPROVED`). O identificador não é reutilizado. | A funcionalidade F011 saiu do âmbito (DA v04); a decisão da Empresa é consultada em `/empresa/estado` (secção 8.2). |
| DF-14 | O estado da área de conversas fica num contexto (`ConversationsProvider`) montado na `RecruiterShell`, lido pelo `useConversations`. Fora da `ConversationsPage`, o contador da barra lateral é atualizado por uma leitura da lista quando a moldura é montada e quando o Recrutador muda de página, e não por consulta periódica. | A lista e o contador têm de ser os mesmos na página e na barra lateral; a DA v04 (F010) e o P17 só preveem a consulta periódica enquanto a conversa ou a lista de conversas está aberta. Uma leitura por mudança de página mantém o contador útil sem pedidos a cada 3 segundos em todas as páginas. |
| DF-15 | A consulta periódica agenda o pedido seguinte só depois da resposta do anterior (`PollingLoop` com `setTimeout`), e não com um intervalo fixo independente das respostas (`setInterval`). O agendamento fica atrás de `IPollingScheduler`. | Evita pedidos sobrepostos quando o servidor demora, cumprindo o atraso máximo do P17 com o RNF001 (2 segundos por operação); a interface permite testar o intervalo com temporizadores simulados do Jest (RI, secção 11.5). |

---

## 14. Limitações

| Limitação | Consequência |
| --- | --- |
| Os campos de DTO acrescentados pela `I104` constam da documentação da API v02, mas ainda não do modelo de classes do backend v02 (API v02, L-08). | Se o backend de `m3` não os implementar, as colunas que deles dependem (W16, W17, W21, W23) ficam sem dados; a divergência é tratada como defeito do backend, porque prevalece a documentação da API (DAPI-11). |
| As divergências P1 e P2 (secção 12) ficam por corrigir no protótipo. | O comportamento implementado segue este modelo, os requisitos e a documentação da API, e não os ecrãs W05, W06 e W26 nesses pontos. |
| A mensagem nova pode demorar até 3 segundos mais a duração do pedido a aparecer, e a mudança de estado de uma conversa só chega na consulta seguinte (documentação da API v02, L-04). | Dentro do P17 (5 segundos). Fora da área de conversas, o contador da barra lateral só é atualizado quando o Recrutador muda de página (DF-14). |
| A consulta das mensagens com `after` só devolve mensagens novas, sem a indicação de leitura das mensagens já recebidas. | A indicação «Lida» das mensagens enviadas pelo Recrutador só se atualiza quando a conversa é aberta de novo, com o histórico completo. O RF072 (histórico com a indicação de leitura) é cumprido na abertura da conversa. |
| A consulta periódica faz um pedido a cada 3 segundos por pedido repetido (lista e conversa), enquanto a `ConversationsPage` está aberta (documentação da API v02, L-07). | Adequado aos dados de demonstração (P18); a arquitetura v02 (secção 3.6, ponto 1) manda verificar o RNF001 com as consultas periódicas no ambiente de demonstração. |
| Os pedidos da secção 5 seguem a documentação da API v02. | Uma nova versão dessa documentação obriga a rever a tabela da secção 5, a secção 4.2 e a secção 6, e a acrescentar uma linha ao histórico de versões. |
| As assinaturas mostram os tipos de retorno sem `Promise`. | Simplificação de leitura; a implementação em `m4` segue a convenção da secção 2.2. |
| O separador do browser em segundo plano mantém a consulta periódica enquanto a `ConversationsPage` estiver aberta. | Os browsers podem espaçar os temporizadores de separadores em segundo plano; ao voltar ao separador, a consulta seguinte repõe o estado. Não afeta os requisitos, que se verificam com a página visível. |
| A decisão DF-05 não está fixada no Regulamento Interno. | Se o grupo decidir outra forma de persistência da credencial, muda apenas o `SessionStore`. |
| O modelo segue o protótipo de baixa fidelidade v02. | Um protótipo de alta fidelidade, se existir, pode alterar componentes de apresentação sem alterar páginas, hooks nem serviços. |

---

## 15. Rastreabilidade

| Casos de uso | Páginas | Hooks | Serviços | Ecrãs do protótipo |
| --- | --- | --- | --- | --- |
| UC01 (início e fim de sessão) | `LoginPage`, `AccountPage`, `AccessDeniedPage` | `useSession` | `AuthService` | W01, W25, A01, A01b, A02b, A11 |
| UC02 (alterar a palavra-passe) | `AccountPage`, `ChangePasswordPage` | `useSession` | `AuthService` | W25, A02b, A10, A10b |
| UC07 (registo da Empresa) | `CreateRecruiterAccountPage`, `CompanyRegistrationPage`, `CompanyRequestStatusPage` | `useSession`, `useCompany`, `useReferenceLists` | `AuthService`, `CompanyService`, `ReferenceListService` | W02 a W06, W26 |
| UC08 (página e dados da Empresa) | `CompanyProfilePage` | `useCompany`, `useProtectedFile` | `CompanyService`, `IApiClient.getFile` | W07, W08 |
| UC09, UC10 (vagas) | `JobListPage`, `JobFormPage` | `useJobs`, `useReferenceLists` | `JobService`, `ReferenceListService` | W09 a W15 |
| UC11 (perfil do Candidato) | `WaitingCandidatesPage`, `CandidateProfilePage` | `useWaitingCandidates`, `useCandidateProfile`, `useProtectedFile` | `CandidateEvaluationService` | W16, W17 |
| UC12 (aceitar ou recusar) | `CandidateProfilePage`, `MatchConfirmedPage` | `useCandidateProfile` | `CandidateEvaluationService` | W17 a W20 |
| UC13 (matches e contactos) | `MatchConfirmedPage`, `MatchesPage` | `useCandidateProfile`, `useMatches`, `useProtectedFile` | `CandidateEvaluationService`, `MatchService` | W19, W21 |
| UC14 (conversas) | `ConversationsPage` | `useConversations` (`ConversationsProvider`, `PollingLoop`) | `ConversationService` | W22, W23 |
| UC16 (aprovar ou recusar o registo de Empresa) | `PendingCompaniesPage`, `PendingCompanyDetailPage`, `AccessDeniedPage` | `usePendingCompanies` | `AdminCompanyService` | A03 a A04e, A11 |
| UC17 (supervisionar Empresas e vagas) | `CompaniesPage`, `PublishedJobsPage` | `useCompaniesAdmin`, `usePublishedJobs` | `AdminCompanyService` | A05, A05b, A06 |
| UC18 (contas de utilizador) | `AccountsPage`, `CandidatesPage` | `useAccounts`, `useCandidatesAdmin` | `AdminAccountService` | A07 a A08b |
| UC19 (painel de indicadores) | `IndicatorsPage` | `useIndicators` | `IndicatorService` | A02, A02c, A02d |
| UC20 (listas pré-definidas) | `ReferenceListsPage` | `useReferenceLists` | `ReferenceListService` | A09, A09b |

O UC15 (consultar notificações) foi retirado da especificação de casos de uso v02 e deixa de ter correspondência.

| Verificação | Resultado |
| --- | --- |
| Ecrãs do Recrutador do protótipo v02 com página, estado ou diálogo correspondente | 25 de 25 (W01 a W23, W25 e W26; o W24 foi retirado) |
| Ecrãs e estados do Administrador do protótipo v02 com página, estado, diálogo ou componente correspondente | 29 de 29 (A01 a A11, com os 18 estados) |
| Controllers do backend usados pela área de gestão web | 11 de 13: 10 com serviço correspondente e o `FilesController` através de `IApiClient.getFile`; `CandidateProfileController` e `JobExplorationController` são exclusivos do Candidato |
| Hubs, acontecimentos e notificações | Nenhum (DA v04); consulta periódica com 2 pedidos repetidos: 6.8.1 e 6.8.2 com `after` (secção 6) |
| DTOs usados pela área de gestão web declarados como tipos | 29, com os campos da documentação da API v02 (secção 8); enumerações do contrato: 10 |
| Operações dos serviços com pedido na documentação da API v02 | 53 de 53 |
| Endpoints da documentação da API v02 usados pela área de gestão web | 54 de 72 (as 53 operações dos serviços e `GET /api/files/{kind}/{fileName}`); os restantes 18 são exclusivos do Candidato (`RegisterCandidate` e os controllers `CandidateProfileController` e `JobExplorationController`) |
| Requisitos funcionais com componente «Frontend web» representados | 67 de 67 (RF037 a RF064, RF066 a RF075, RF083 a RF104, RF106, RF107, RF109, RF114 a RF117); RF065, RF112, RF113 e RF118 são só do backend e são respeitados pelas regras das secções 5 e 10 |
| Requisitos descontinuados ainda referidos como em vigor | Nenhum (RF076 a RF082 e RF111 retirados das tabelas) |
| Rotas declaradas | 25: 3 públicas, 12 do Recrutador, 9 do Administrador e 1 que exige apenas sessão |
| Fonte textual dos diagramas incluída | Sim, em Mermaid, em todas as secções com diagrama |

---

## 16. Alterações face à v01

| # | Secção | Alteração na v02 | Fundamento |
| --- | --- | --- | --- |
| 1 | 1 | Identificação na v02, Issue `I111`, responsáveis da v02, documentos de origem na PS e na DA v04 e nas v02 da arquitetura, do backend, da especificação de requisitos, dos casos de uso, do protótipo, dos diagramas de comportamento, do modelo de dados e da API; linha `OF-M3-025` da CCA. | Redução de âmbito (`I100`); CCA v04 |
| 2 | 2.1, 13 (DF-01) | Camada Tempo real (`src/realtime`) substituída pela camada Consulta periódica (`src/polling`); a Sessão deixa de depender dela; diagrama das camadas sem os hubs. | DA v04, F010; arquitetura v02, AD-02 |
| 3 | 3.1 | Retirada a enumeração `NotificationType` e o mapa das ações das notificações; atualizada a lista das enumerações internas (`MatchStatus`, `OperationType`, `ClientApp`) e o tratamento de `FileKind`; acrescentada a apresentação de `closedBy`. | DA v04; API v02, 2.4 e 8.4 |
| 4 | 3.2 | Retirados `NotificationDto` e `NotificationListDto` (29 tipos); acrescentados `CompanyRegistrationDto.locationId`, `WaitingCandidateDto.desiredRole` e `locationName`, `CandidateFullProfileDto.profileOpenedAt`, `MatchDto.jobStatus`, `locationName` e `logoUrl`, `ConversationSummaryDto.closedBy`, `ApiErrorDto.code` e `blockedUntil`. | API v02, secção 8 e 11.3 (PA-03, PA-05, PA-07 a PA-09, PC-04) |
| 5 | 4.1, 4.2, 13 (DF-12) | `ApiError.errorId` passa a `code`, copiado do `ApiErrorDto`; os erros deixam de ser identificados pela comparação das mensagens; tratamento dos erros na consulta periódica (ERR-01, ERR-03, ERR-04, falha de ligação). | API v02, 4.1 (PA-05) |
| 6 | 4.3, 5 | Ficheiros obtidos em `GET /api/files/{kind}/{fileName}`, com `404` tratado como marcador vazio; acrescentada a linha de `IApiClient.getFile` à tabela dos pedidos. | API v02, 6.9.1 (PA-01) |
| 7 | 5 | Retirado o `NotificationService`; `CompanyService.getRegistration` e `getPage` com os pedidos 6.4.8 e 6.4.9; `ConversationService.open(matchId, after)`; pedidos referidos à API v02 com o número da secção; regras 10 e 11. | API v02, 6.4, 6.8 e 7 |
| 8 | 6 | Secção «Ligação persistente» substituída por «Consulta periódica das conversas»: retirados `IRealtimeClient`, `SignalRRealtimeClient`, `RealtimeStatus`, `RealtimeProvider` e os hubs; acrescentados `IPollingScheduler`, `TimeoutPollingScheduler`, `PollingLoop`, `pollingConfig` e `ConversationsProvider`, com os pedidos repetidos e as regras 1 a 8. | DA v04, F010 e limites transversais; P17; backend v02, 6.1; API v02, 7 |
| 9 | 7.1 | `SessionProvider` sem dependência do cliente de tempo real; `signIn` e `signOut` sem abertura e fecho da ligação; `companyStatus` atualizado pela consulta do estado do pedido, e não pela notificação da decisão. | DA v04, F003; RF044 |
| 10 | 7.2 | Retirada a rota `/notificacoes` (25 rotas). | Protótipo v02 (sem o W24) |
| 11 | 8.1 | `RecruiterShell` sem item nem contador de notificações e com o `ConversationsProvider`; `Sidebar` só com o contador de conversas; ecrã W24 retirado das listas. | Protótipo v02; DF-14 |
| 12 | 8.2, 8.3 | Retirada a `NotificationsPage`; `CompanyRequestStatusPage` passa a ser o ponto de consulta da decisão do Administrador; `WaitingCandidatesPage`, `CandidateProfilePage`, `MatchesPage` e `ConversationsPage` com os campos da API v02 e a consulta periódica; páginas do Administrador sem referência a aviso ao Recrutador. | DA v04, F003, F007, F008, F010; API v02 |
| 13 | 8.4, 9 | Retirado o `useNotifications`; `useConversations` lê o `ConversationsProvider`, com `watchList`; tabela de origem das atualizações revista. | Secção 6; DF-14 |
| 14 | 10 | Passo 7 do exemplo: sem notificação de novo match; abertura da conversa e consulta periódica. | DA v04, F008 |
| 15 | 11 | Dependências G1 a G12 com a resolução da API v02 (G6 sem aplicação); sem dependências por resolver. | API v02, 11.3 |
| 16 | 12 | Divergência P1 ajustada ao protótipo v02 (sem o item Notificações). | Protótipo v02 |
| 17 | 13 | DF-08 e DF-12 revistas; DF-13 retirada; acrescentadas DF-14 (contexto das conversas e contador da barra lateral) e DF-15 (agendamento sem sobreposição). | Secções 4.2 e 6 |
| 18 | 14 | Retiradas as limitações da consulta periódica pendente, do SignalR e das dependências em aberto; acrescentadas as limitações dos campos da `I104` no backend, do atraso da consulta periódica, da indicação de leitura com `after`, da carga e dos separadores em segundo plano. | API v02, L-04, L-07 e L-08 |
| 19 | 15 | Retirado o UC15; verificação com o protótipo v02 (25 ecrãs do Recrutador), 13 controllers da API v02, 29 tipos, 53 operações, 54 de 72 endpoints, 67 requisitos funcionais e 25 rotas. | Especificação de requisitos v02; API v02 |

As restantes classes, operações, relações, regras e decisões da v01 mantêm-se sem alterações.
