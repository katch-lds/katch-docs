# Documentação da API — Katch

**Unidade curricular:** Laboratório de Desenvolvimento de Software — LDS
**Milestone:** `m2`
**Ficheiro:** `m2-documentacao-api-v01.md`
**Pasta de arquivo:** `04-artefactos-tecnicos/04.11-documentacao-de-api`

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
| Módulo | Backend (`katch-backend`), API REST ASP.NET Core 10 em C# 14 e ligação persistente ASP.NET Core SignalR |
| Issue | `I092` — Elaborar a documentação da API |
| Executor / Revisor / Auditor | João Borguem / João Coelho / Miguel Santos |
| Documentos de origem | `m2-modelo-classes-backend-v01.md` (secções 6 e 7, `I036`), `m2-documentacao-arquitetura-v01.md` (secções 2 e 3, `I033` e `I034`), `m2-especificacao-requisitos-v01.md` (RF001 a RF118, RNF001 a RNF018, parâmetros P01 a P21; `I020` a `I023`), `m2-especificacoes-casos-uso-v01.md`, `m2-modelo-de-dados-katch-v01.md` (`I035`), `m2-diagrama-estados-interesse-match-v01.md` (`I086`), `m1-declaracao-ambito-v02.pdf`, `m2-backlog-projeto-v03.xlsx` (Issue `I092`) |

O documento segue a secção 19 do Regulamento de Funcionamento da Unidade Curricular para `04.11-documentacao-de-api` e corresponde à linha `OF-M2-011` da Checklist de Controlo de Artefactos. Fixa o contrato entre o backend e os dois clientes (aplicação móvel e área de gestão web) antes da implementação em `m3`: rotas, autenticação, autorização por tipo de conta, parâmetros, formatos dos DTOs, respostas, códigos de estado, erros e eventos da ligação persistente.

### 1.1. Histórico de versões

| Versão | Data | Descrição das alterações | Issue |
| --- | --- | --- | --- |
| v01 | 2026-10-09 | Criação do documento: convenções, autenticação JWT, modelo de erros, 68 endpoints dos 13 controllers, 2 hubs da ligação persistente, formatos dos DTOs, rastreabilidade com os RF e pontos em aberto. | `I092` |
| v01 | 2026-10-09 | Alinhamento com os artefactos aprovados antes da revisão: evento `MessageReceived` e autorização da lista de localidades iguais ao modelo de classes v01; exemplos sem rotas fora do modelo de classes e sem abreviaturas; pontos em aberto com origem, classificação e necessidade de decisão formal. | `I092` |
| v01 | 2026-10-09 | Acrescento dos pontos em aberto PA-07 a PA-09 (atributos em falta em `WaitingCandidateDto`, `MatchDto` e `CandidateFullProfileDto`), detetados na comparação com o modelo de classes do frontend web (`I090`). | `I092` |
| v01 | 2026-10-09 | Diagrama da secção 7.4 corrigido; ponto em aberto PA-10 (obtenção do CV pelo próprio Candidato); Empresa aprovada exigida na lista de matches do Recrutador; regra do cabeçalho `Location` nas respostas `201`; consultas `GET` com efeito identificadas (secção 2.5, DAPI-05); prevalência sobre o Swagger registada como DAPI-11; cronologia única dos dados fictícios dos exemplos; tempo em falta na mensagem do ERR-31; ERR-35 também na obtenção do CV. | `I092` |
| v01 | 2026-10-09 | Classificação de PA-07 a PA-09 como propostas de melhoria (incoerência com o protótipo da `I040`, ecrãs W16, W21 e W17); políticas das consultas propostas na PA-03 (`Recruiter` para os dados de registo, `ApprovedCompany` para a página). | `I092` |

Cada alteração posterior acrescenta uma linha. As versões anteriores são conservadas, nos termos da secção 18.2 do Regulamento de Funcionamento da Unidade Curricular.

---

## 2. Âmbito e convenções

### 2.1. Objeto e fontes

A API documentada é a interface do servidor (especificação de requisitos, secção 2.2): o conjunto das operações que o backend disponibiliza à aplicação móvel e à área de gestão web. Cada endpoint corresponde a uma operação de um controller do modelo de classes (secção 6) e cada evento a um hub (secção 6.1). Os formatos são os DTOs do modelo de classes (secção 7). Não há endpoints fora da Declaração de Âmbito v02: o sistema não tem integração com serviços externos, nem envio de e-mail, SMS ou notificações nativas (arquitetura, secção 2.5).

O contrato documentado é exatamente o do modelo de classes v01 e da especificação de requisitos v01, nos aspetos que estes já fixam (operações, DTOs, hubs, eventos e autorização). Este documento acrescenta apenas o que o modelo de classes remete para a documentação da API (secção 11 do modelo): rotas, métodos HTTP, códigos de estado, mensagens de erro, cabeçalhos e exemplos.

As lacunas encontradas no modelo de classes e na especificação de requisitos durante a elaboração deste documento não foram resolvidas aqui, para não alterar artefactos aprovados sem decisão formal. Ficam registadas na secção 11, com o impacto e a proposta de correção, para tratamento numa nova versão desses artefactos e, depois, deste documento.

A documentação interativa gerada pelo Swashbuckle (`/swagger`) é derivada do código em `m3` e tem de coincidir com este documento. A arquitetura (secção 3.3) indica o Swagger como referência para explorar e testar os endpoints. Em caso de divergência entre o Swagger e este documento, prevalece este documento até à aprovação de uma nova versão (DAPI-11).

### 2.2. Endereço base

| Ambiente | Endereço base | Notas |
| --- | --- | --- |
| Desenvolvimento | `https://localhost:<porta>` | Porta fixada no `launchSettings.json` do `katch-backend` e indicada no manual de instalação (`04.17`). |
| Ambiente de demonstração | `https://<endereço do servidor do grupo>:<porta>` | Rede local do grupo (especificação de requisitos, secção 2.2). |

Todas as rotas da API REST começam por `/api`; as dos hubs começam por `/hubs`. A documentação interativa está em `/swagger`, disponível apenas em desenvolvimento e no ambiente de demonstração. Só é aceite HTTPS; os pedidos HTTP são redirecionados.

### 2.3. Formato dos dados

| Aspeto | Convenção |
| --- | --- |
| Formato | JSON em UTF-8 (`Content-Type: application/json; charset=utf-8`), exceto os carregamentos de ficheiros (`multipart/form-data`) e a obtenção do CV (`application/pdf`). |
| Nomes das propriedades | camelCase (convenção por omissão do `System.Text.Json` no ASP.NET Core): `FullName` ↔ `fullName`. |
| Identificadores | `Guid` em texto, no formato `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`. |
| Datas e horas | `DateTimeOffset` em ISO 8601, em UTC e com o sufixo `Z`: `2026-10-09T14:30:00Z`. Os clientes convertem para a hora local na apresentação. |
| Datas sem hora | `DateOnly` em `yyyy-MM-dd` (só nos parâmetros dos indicadores). |
| Valores monetários | `decimal` em número JSON com até 2 casas decimais, em euros brutos mensais: `1850.00`. |
| Enumerações | Texto em maiúsculas com `_`, igual aos valores do PostgreSQL (`JsonStringEnumConverter` com `JsonNamingPolicy.SnakeCaseUpper`): `ContractType.ServiceProvision` ↔ `"SERVICE_PROVISION"`. Valores fora da lista são rejeitados com `400`. |
| Valores opcionais | `null` explícito. Nas respostas, as propriedades opcionais aparecem sempre, com `null` quando não há valor. |
| Listas | Arrays JSON; uma lista vazia é `[]`, nunca `null`. |
| Textos | Os espaços no início e no fim são retirados antes da validação; um texto só com espaços conta como vazio. |

### 2.4. Enumerações

| Enumeração | Valores JSON | Significado (especificação de requisitos) |
| --- | --- | --- |
| `UserType` | `CANDIDATE`, `RECRUITER`, `ADMIN` | Tipo de conta |
| `AccountStatus` | `ACTIVE`, `BLOCKED`, `SUSPENDED` | Ativa, bloqueada, suspensa |
| `WorkMode` | `ON_SITE`, `HYBRID`, `REMOTE` | Presencial, híbrido, remoto (P16) |
| `ContractType` | `PERMANENT`, `FIXED_TERM`, `INTERNSHIP`, `SERVICE_PROVISION` | Sem termo, a termo, estágio, prestação de serviços (P09) |
| `Availability` | `IMMEDIATE`, `FIFTEEN_DAYS`, `ONE_MONTH`, `TO_BE_AGREED` | Imediata, 15 dias, 1 mês, a combinar (P14) |
| `Industry` | `TECHNOLOGY`, `HEALTHCARE`, `RETAIL`, `HOSPITALITY`, `CONSTRUCTION`, `MANUFACTURING`, `EDUCATION`, `FINANCE`, `LOGISTICS`, `SERVICES`, `OTHER` | Tecnologia, saúde, comércio, hotelaria e restauração, construção, indústria, educação, finanças, logística, serviços, outro (P15) |
| `CompanyStatus` | `PENDING`, `APPROVED`, `REJECTED`, `SUSPENDED` | Pendente, aprovada, recusada, suspensa |
| `JobStatus` | `UNPUBLISHED`, `PUBLISHED`, `SUSPENDED`, `CLOSED` | Não publicada, publicada, suspensa, encerrada |
| `ConversationStatus` | `OPEN`, `CLOSED` | Aberta, modo apenas de consulta |
| `ConversationCloseReason` | `CLOSED_BY_PARTY`, `ACCOUNT_BLOCKED_OR_SUSPENDED`, `COMPANY_SUSPENDED` | Encerrada por uma das partes; conta bloqueada ou suspensa; Empresa suspensa (RF075) |
| `NotificationType` | `MATCH_CONFIRMED`, `NEW_MESSAGE`, `NEW_INTEREST`, `COMPANY_APPROVED`, `COMPANY_REJECTED`, `INTEREST_QUOTA_RESTORED` | Tipos de notificação (secção 7.3) |

As enumerações internas (`CandidateDecision`, `RecruiterDecision`, `MatchStatus`, `JobCloseReason`, `EntityType`, `OperationType`, `FileKind`, `ClientApp`) não aparecem em nenhum DTO e não fazem parte do contrato.

### 2.5. Rotas e métodos

| Regra | Aplicação |
| --- | --- |
| Rota base | A do controller no modelo de classes (secção 6), sem prefixo de versão. Uma alteração incompatível do contrato exige nova versão deste documento e dos clientes. |
| `GET` | Consulta. Três consultas têm um efeito registado no servidor, todas idempotentes (repetir o pedido não produz efeito novo): 6.3.6 aplica a reposição da quota cujo período de bloqueio já terminou (RF021); 6.6.2 regista a primeira abertura do perfil completo (RF118); 6.8.2 marca como lidas as mensagens da outra parte (RF108, RF109). Nenhum outro `GET` altera dados (DAPI-05). |
| `POST` | Criação de um elemento (`201 Created`) ou ação de mudança de estado sobre um elemento existente, na forma `POST …/{id}/<ação>` (`200 OK` com o elemento atualizado, ou `204 No Content`). |
| `PUT` | Substituição dos dados editáveis de um elemento ou de um ficheiro único (fotografia de perfil, CV, logótipo). |
| `DELETE` | Eliminação (lógica, no caso das vagas). |
| Parâmetros de rota | Identificadores com a restrição `:guid` (`{jobId:guid}`), para que as rotas literais (`/next`, `/quota`, `/pending`) não colidam com os identificadores. Um identificador mal formado devolve `404`. |
| Identificador do utilizador | Nunca vai na rota nem no corpo: é sempre o da credencial de sessão (RNF006; modelo de classes, DC-05). |
| Paginação | Não existe na v01: as listas são devolvidas completas, na ordem indicada em cada endpoint (secção 11, L-02). |

### 2.6. Ficheiros

| Tipo de ficheiro | Formatos | Dimensão máxima | Endpoint | Requisitos |
| --- | --- | --- | --- | --- |
| Fotografia de perfil | JPEG, PNG | 5 MB | `PUT /api/candidate/profile/photo` | RF010, P01 |
| Curriculum vitae | PDF | 5 MB | `PUT /api/candidate/profile/cv` | RF011, P02 |
| Logótipo da Empresa | JPEG, PNG | 2 MB | `PUT /api/recruiter/company/logo` | RF047, P01 |
| Fotografia da galeria | JPEG, PNG | 5 MB; máximo 6 | `POST /api/recruiter/company/photos` | RF048, P01, P13 |
| Fotografia da vaga | JPEG, PNG | 5 MB; máximo 5 por vaga | `POST /api/recruiter/jobs/{jobId}/photos` | RF052, P01, P13 |

- O ficheiro é enviado em `multipart/form-data`, num único campo chamado `file`.
- O `FileValidator` verifica a assinatura do conteúdo (os primeiros bytes), a extensão e a dimensão. Um ficheiro com extensão `.png` e conteúdo de outro formato é rejeitado (RNF016).
- O limite do pedido no Kestrel é de 6 MB nestes endpoints, para que um ficheiro acima do limite do P01 ou do P02 chegue ao validador e receba `400` com o motivo. Acima de 6 MB, o servidor devolve `413` (ERR-12).
- Os carregamentos que substituem um ficheiro único (`PUT`) devolvem `204 No Content`. Os que acrescentam a uma galeria (`POST`) devolvem `201 Created`. Em ambos os casos, exceto no CV, o cabeçalho `Location` traz o URL do ficheiro.
- O caminho no sistema de ficheiros nunca é enviado ao cliente. Os DTOs trazem URLs da API que servem o ficheiro depois de verificar a autorização (modelo de classes, secção 7.2). O modelo de classes v01 não tem a operação que serve esses URLs (secção 11, PA-01), pelo que o formato do URL não é fixado neste documento e os exemplos usam o marcador `<URL do ficheiro>`.
- O CV nunca tem URL: o Candidato vê `hasCv` e o Recrutador obtém-no em `GET /api/recruiter/interests/{matchId}/cv` (RNF007). O RNF007 admite também o próprio Candidato, mas o modelo de classes v01 não tem operação para isso (secção 11, PA-10).

---

## 3. Autenticação e autorização

### 3.1. Credencial de sessão

A autenticação usa JSON Web Token com o esquema JWT Bearer do ASP.NET Core (arquitetura, D-04). A credencial é emitida pelo `JwtTokenService` no início de sessão, no registo do Candidato e na criação de conta de Recrutador, e é apresentada em cada pedido seguinte:

```http
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIzZjJhOWMxZS03YjRkLTRlOGEtOWMyMS01ZDZlN2Y4YTliMDEiLCJyb2xlIjoiQ0FORElEQVRFIn0.<assinatura>
```

| Elemento | Valor |
| --- | --- |
| Algoritmo | HS256, com o segredo em variável de ambiente (desenvolvimento) ou GitHub Secrets (pipeline) (RI, secção 14.2). |
| `sub` | Identificador da conta (`AppUser.Id`). |
| `role` | Tipo de conta: `CANDIDATE`, `RECRUITER` ou `ADMIN`. Usado nos atributos `[Authorize(Roles = …)]`. |
| `iat`, `exp` | Emissão e expiração: 8 horas depois do início de sessão (RNF005, P20). Sem tolerância de relógio (`ClockSkew = 0`). |
| `iss`, `aud` | `katch-backend` e `katch-clients`. |
| Renovação | Não existe. Expirada a credencial, o cliente pede novo início de sessão. |

A credencial não contém o estado da conta nem o da Empresa. Esses estados são confirmados na base de dados em cada pedido (secção 3.4).

### 3.2. Ponto de acesso

O Candidato só inicia sessão na aplicação móvel (RF001) e o Recrutador e o Administrador só na área de gestão web (RF038, RF083). O cliente indica o ponto de acesso no cabeçalho `X-Client-App` do pedido de início de sessão, que o controller converte na enumeração `ClientApp` passada a `IAuthService.LoginAsync` (modelo de classes, secção 5.1).

| Cabeçalho | Valor | Tipos de conta aceites |
| --- | --- | --- |
| `X-Client-App` | `mobile` | Candidato |
| `X-Client-App` | `web` | Recrutador, Administrador |

Um pedido sem o cabeçalho, ou com outro valor, é rejeitado com `400`. Uma conta que tenta entrar pelo ponto de acesso errado recebe `403` (ERR-05), depois de verificadas as credenciais.

### 3.3. Políticas de autorização

| Política | Regra | Onde se aplica |
| --- | --- | --- |
| Anónimo | Sem credencial. | Registo de Candidato, criação de conta de Recrutador, início de sessão (especificação de requisitos, secção 2.2: operações não reservadas). |
| Qualquer conta | Credencial válida, conta ativa. | Fim de sessão, alteração da palavra-passe, consulta das listas pré-definidas (localidades, competências e benefícios). |
| `Candidate` | `role = CANDIDATE`. | `/api/candidate/**` |
| `Recruiter` | `role = RECRUITER`. | `/api/recruiter/company/**` |
| `ApprovedCompany` | `Recruiter` e Empresa do Recrutador no estado `APPROVED`. | `/api/recruiter/jobs/**`, `/api/recruiter/interests/**` (RF039); página, logótipo e galeria da Empresa (modelo de classes, secção 6: «página e logótipo também exigem Empresa aprovada»; a galeria faz parte da página, RF048); alteração dos dados de registo (RF049, só sobre a Empresa aprovada); `GET /api/matches` quando pedido pelo Recrutador (RF039, RF067) |
| `CandidateOrRecruiter` | `role` igual a `CANDIDATE` ou `RECRUITER`; o Administrador é rejeitado. | `/api/matches`, `/api/conversations/**` (RF103), `/api/notifications/**`, `/hubs/**` |
| `Admin` | `role = ADMIN`. | `/api/admin/**`, escrita nas listas pré-definidas (RF104) |

Além da política, os serviços verificam a titularidade do elemento: a vaga é da Empresa do Recrutador (RF059), o match é do Candidato ou de uma vaga da Empresa do Recrutador, a notificação é do utilizador. A falha de titularidade devolve `403` (ERR-08), nos termos do UC12 (exceção E3) e do RNF006.

### 3.4. Verificação do estado em cada pedido

Em cada pedido autenticado, antes da operação, o backend:

1. valida a assinatura e a expiração da credencial; falha → `401` (ERR-01);
2. lê `app_user.status` da conta do `sub`; `BLOCKED` → `403` (ERR-03), `SUSPENDED` → `403` (ERR-04) (RF086, RF089; arquitetura, D-04);
3. aplica a política do endpoint; tipo de conta não autorizado → `403` (ERR-06);
4. na política `ApprovedCompany`, lê `company.status`; Empresa não registada ou noutro estado → `403` (ERR-07), com o estado atual (RF039).

Um bloqueio ou suspensão produz efeito no pedido seguinte, sem esperar pela expiração da credencial.

### 3.5. Fim de sessão

A autenticação não guarda estado no servidor. `POST /api/auth/logout` apenas confirma o pedido; o cliente descarta a credencial e passa a exigir novo início de sessão (RF105 a RF107; modelo de classes, secção 6). A credencial descartada continua tecnicamente válida até expirar (secção 11, L-03).

---

## 4. Respostas de erro

### 4.1. Formato

Todas as respostas de erro, de todos os controllers, usam `ApiErrorDto` (modelo de classes, secção 7.1), incluindo as geradas pelo pipeline do ASP.NET Core (`401` e `403` do JWT Bearer, `404` de rota inexistente, `413`, `415` e `500`), que são reescritas por um filtro de exceções e pelos eventos `OnChallenge` e `OnForbidden`.

| Campo | Tipo | Conteúdo |
| --- | --- | --- |
| `message` | texto | Motivo da rejeição, em português, apresentável ao utilizador. |
| `fields` | `FieldErrorDto[]` | Um elemento por campo em causa (RF004, RF042). Lista vazia quando o erro não é de um campo. |
| `fields[].field` | texto | Nome JSON do campo, em camelCase; nos elementos de listas, com o índice: `urls[1]`, `skillIds[0]`. Nos carregamentos, `file`. |
| `fields[].reason` | texto | Motivo para esse campo. |

```json
{
  "message": "Os dados enviados não cumprem a validação automática.",
  "fields": [
    { "field": "email", "reason": "O endereço de correio eletrónico já está registado." },
    { "field": "phoneNumber", "reason": "O contacto telefónico tem de ter 9 algarismos." }
  ]
}
```

As respostas de erro nunca incluem detalhes internos (pilha de chamadas, SQL, caminhos de ficheiros), palavras-passe, credenciais de sessão nem texto de mensagens (RNF017).

### 4.2. Códigos de estado

| Código | Utilização |
| --- | --- |
| `200 OK` | Consulta, ou ação com o elemento atualizado no corpo. |
| `201 Created` | Elemento criado; o corpo traz o elemento quando o endpoint tem DTO de resposta. O cabeçalho `Location` só é enviado quando existe um URL da API para consultar o elemento criado: rota de consulta em 6.1.1 e 6.4.1; URL do ficheiro em 6.4.7 e 6.5.9. Os restantes `201` (6.1.2, 6.2.4, 6.5.2, 6.8.3, 6.13.4 e 6.13.6) não o trazem, porque o modelo de classes não tem operação de consulta individual desses elementos. |
| `204 No Content` | Ação concluída sem corpo; também `GET /api/candidate/jobs/next` sem vagas para apresentar. |
| `400 Bad Request` | Validação automática: formato, campos obrigatórios, limites P01 a P16, unicidade de dados de formulário (correio eletrónico, NIF, designações das listas). |
| `401 Unauthorized` | Credencial em falta, inválida ou expirada; credenciais erradas no início de sessão. |
| `403 Forbidden` | Conta bloqueada ou suspensa; ponto de acesso errado; tipo de conta não autorizado; Empresa não aprovada; elemento de outro utilizador ou Empresa. |
| `404 Not Found` | Elemento inexistente, eliminado ou não visível para o utilizador. |
| `409 Conflict` | A operação não é possível no estado atual do elemento: transição inválida, quota esgotada, decisão já registada, conversa em modo apenas de consulta, alteração concorrente (`xmin`). |
| `413 Payload Too Large` | Pedido acima de 6 MB nos carregamentos, ou de 64 KB nos restantes. |
| `415 Unsupported Media Type` | `Content-Type` diferente de `application/json` ou `multipart/form-data`, conforme o endpoint. |
| `500 Internal Server Error` | Falha inesperada; a transação é desfeita e nada fica gravado (UC12, E5). |

A unicidade de correio eletrónico, NIF e designações é tratada como `400` com o campo em causa, e não como `409`, porque os requisitos a incluem na validação automática do formulário (RF004, RF037, RF042, RF099 a RF102).

### 4.3. Catálogo de erros

As mensagens são fixas, para que os clientes e a collection Postman de `m3` as possam verificar. Os valores entre chavetas são preenchidos pelo serviço.

| ID | Código | `message` | Situação | Requisitos |
| --- | --- | --- | --- | --- |
| ERR-01 | 401 | A sessão expirou ou não é válida. Inicie sessão novamente. | Credencial em falta, mal formada, com assinatura inválida ou expirada. | RNF004, RNF005 |
| ERR-02 | 401 | O endereço de correio eletrónico ou a palavra-passe estão incorretos. | Início de sessão com credenciais que não coincidem; a mesma mensagem para correio inexistente e palavra-passe errada. | RF001, RF038, RF083, RNF009 |
| ERR-03 | 403 | A conta está bloqueada. | Conta `BLOCKED`, no início de sessão ou em qualquer pedido. | RF001, RF038, RF086 |
| ERR-04 | 403 | A conta está suspensa. | Conta `SUSPENDED`, no início de sessão ou em qualquer pedido. | RF001, RF089 |
| ERR-05 | 403 | Esta conta não tem acesso por este ponto de acesso. | Candidato com `X-Client-App: web`, ou Recrutador ou Administrador com `mobile`. | RF001, RF038, RF083 |
| ERR-06 | 403 | A operação não está disponível para este tipo de conta. | Política do endpoint não cumprida (por exemplo, Administrador em `/api/conversations`). | RF103, RF104, RNF006 |
| ERR-07 | 403 | A operação exige a Empresa aprovada. Estado atual da Empresa: {pendente \| recusada \| suspensa \| não registada}. | Política `ApprovedCompany` não cumprida. | RF039 |
| ERR-08 | 403 | Não tem autorização para aceder a este elemento. | Elemento de outro utilizador ou de outra Empresa. | RF059, RNF006, RNF007 |
| ERR-10 | 400 | Os dados enviados não cumprem a validação automática. | Um ou mais campos não conformes; `fields` identifica cada um. | RF004, RF042, RNF008 |
| ERR-11 | 400 | O ficheiro não é aceite. | Formato, conteúdo ou dimensão fora do P01 ou do P02; `fields[0].field = "file"`. | RF010, RF011, RF047, RF048, RF052, RNF016 |
| ERR-12 | 413 | O pedido excede a dimensão máxima permitida. | Pedido acima do limite do Kestrel. | P01, P02 |
| ERR-13 | 415 | O tipo de conteúdo do pedido não é suportado. | `Content-Type` errado. | — |
| ERR-20 | 404 | O elemento pedido não existe ou não está disponível. | Identificador inexistente, vaga eliminada, vaga ou Empresa não visível ao Candidato. | RF014 |
| ERR-30 | 409 | A operação não é permitida no estado atual: {estado}. | Transição de estado inválida (vaga, Empresa, conta). | RF053 a RF056, RF087, RF090, RF093 a RF095, RF115, RF116 |
| ERR-31 | 409 | Esgotou a quota de interesses. Pode voltar a manifestar interesse dentro de {h} h {min} min. | Interesse no período de bloqueio. A mensagem traz o tempo em falta pedido pelo RF020; o instante exato está em `QuotaDto.blockedUntil` (6.3.6). | RF020, P11 |
| ERR-32 | 409 | Já respondeu a esta vaga. | Segunda resposta do Candidato à mesma vaga. | RF019 |
| ERR-33 | 409 | A vaga já não está disponível. | A vaga deixou de cumprir o RF014 entre a apresentação do cartão e a ação. | RF014; UC05, E3 |
| ERR-34 | 409 | Abra o perfil completo do Candidato antes de registar a decisão. | Aceitação ou recusa sem abertura do perfil registada. | RF063, RF064; UC12, E1 |
| ERR-35 | 409 | O Candidato já foi avaliado nesta vaga. | Interesse que já não está em espera: decisão já registada (incluindo a perda na concorrência), abertura do perfil (6.6.2) ou obtenção do CV (6.6.3). Erro de estado, e não de autorização, porque a vaga é da Empresa do Recrutador. | RF117, RF061, RNF007; UC12, E4 |
| ERR-36 | 409 | A conversa está em modo apenas de consulta. | Envio ou encerramento numa conversa `CLOSED`. | RF026, RF069, RF075 |
| ERR-37 | 409 | Não existe match para esta conversa. | Pedido de conversa sobre um `match` sem `status = MATCHED`. | RF070 |
| ERR-38 | 409 | A vaga tem interesses registados e só pode ser encerrada. | Eliminação de vaga com pelo menos um interesse. | RF058 |
| ERR-39 | 409 | Foi atingido o número máximo de fotografias ({n}). | Sétima fotografia da galeria ou sexta da vaga. | RF048, RF052, P13 |
| ERR-40 | 409 | O Recrutador já registou uma Empresa. | Segundo registo de Empresa pelo mesmo Recrutador. | Especificação, secção 2.2 |
| ERR-41 | 409 | O elemento foi alterado por outro pedido. Atualize e tente novamente. | Falha do controlo de concorrência otimista (`xmin`) fora dos casos com mensagem própria. | Modelo de dados, secção 7.3 |
| ERR-50 | 500 | Ocorreu um erro inesperado. Tente novamente. | Exceção não tratada; nada fica gravado. | RNF013, RNF017 |

Mensagens de campo (`fields[].reason`) mais frequentes:

| Campo | `reason` | Origem |
| --- | --- | --- |
| Qualquer obrigatório | O campo é obrigatório. | RF004, RF042, RF050 |
| `email`, `contactEmail` | O endereço de correio eletrónico tem de ter o formato local@domínio. | P04 |
| `email` | O endereço de correio eletrónico já está registado. | RF004, RF037 |
| `phoneNumber`, `contactPhone` | O contacto telefónico tem de ter 9 algarismos. | P04 |
| `taxId` | O número de identificação fiscal tem de ter 9 algarismos. | P04 |
| `taxId` | O número de identificação fiscal já está registado. | RF042 |
| `taxId` | O número de identificação fiscal não pode ser alterado depois da aprovação. | RF049 |
| `password`, `newPassword` | A palavra-passe tem de ter pelo menos 8 caracteres, uma letra e um algarismo. | P05 |
| `currentPassword` | A palavra-passe atual está errada. | RF002, RF040, RF084 |
| `acceptTerms` | É obrigatório aceitar as condições de utilização. | RF003, RF037 |
| `locationId`, `skillIds[i]`, `benefitIds[i]`, `skillId` | O valor não pertence à lista pré-definida. | RF003, RF008, RF041, RF050 |
| `maxDistanceKm` | A distância máxima tem de ser um número inteiro entre 1 e 500. | P06 |
| `customLabel` | A competência tem de ter entre 2 e 40 caracteres e conter apenas letras, algarismos, espaços e os símbolos + # . - | P07 |
| `experienceSummary`, `description` | O texto não pode exceder 1000 caracteres. | P08 |
| `urls`, `urls[i]`, `website` | O endereço tem de começar por http:// ou https://. / São permitidas no máximo 3 hiperligações. | P10 |
| `minSalary`, `maxSalary` | O valor tem de ser um número positivo em euros. / O valor mínimo não pode ser superior ao valor máximo. | RF051 |
| `content` | A mensagem tem de ter entre 1 e 1000 caracteres. | P03 |
| `reason` | O motivo tem de ter entre 10 e 500 caracteres. | P12 |
| `name` | A designação é obrigatória. / A designação já existe na lista. | RF099 a RF102 |
| `file` | O ficheiro tem de estar no formato {JPEG ou PNG \| PDF}. / O ficheiro excede {2 \| 5} MB. | P01, P02, RNF016 |

---

## 5. Resumo dos endpoints

| N.º | Método | Rota | Operação do controller | Autorização | Requisitos |
| --- | --- | --- | --- | --- | --- |
| 6.1.1 | `POST` | `/api/auth/candidates` | `AuthController.RegisterCandidate` | Anónimo | RF003 a RF005 |
| 6.1.2 | `POST` | `/api/auth/recruiters` | `AuthController.CreateRecruiterAccount` | Anónimo | RF037 |
| 6.1.3 | `POST` | `/api/auth/login` | `AuthController.Login` | Anónimo | RF001, RF038, RF083 |
| 6.1.4 | `POST` | `/api/auth/logout` | `AuthController.Logout` | Qualquer conta | RF105 a RF107 |
| 6.1.5 | `PUT` | `/api/auth/password` | `AuthController.ChangePassword` | Qualquer conta | RF002, RF040, RF084 |
| 6.2.1 | `GET` | `/api/candidate/profile` | `CandidateProfileController.Get` | `Candidate` | RF006 a RF012 |
| 6.2.2 | `PUT` | `/api/candidate/profile` | `CandidateProfileController.Update` | `Candidate` | RF006 |
| 6.2.3 | `PUT` | `/api/candidate/profile/preferences` | `CandidateProfileController.UpdatePreferences` | `Candidate` | RF007, RF023 |
| 6.2.4 | `POST` | `/api/candidate/profile/skills` | `CandidateProfileController.AddSkill` | `Candidate` | RF008, RF009 |
| 6.2.5 | `DELETE` | `/api/candidate/profile/skills/{candidateSkillId}` | `CandidateProfileController.RemoveSkill` | `Candidate` | RF008, RF009 |
| 6.2.6 | `PUT` | `/api/candidate/profile/links` | `CandidateProfileController.SetLinks` | `Candidate` | RF012 |
| 6.2.7 | `PUT` | `/api/candidate/profile/photo` | `CandidateProfileController.UploadPhoto` | `Candidate` | RF010 |
| 6.2.8 | `PUT` | `/api/candidate/profile/cv` | `CandidateProfileController.UploadCv` | `Candidate` | RF011 |
| 6.3.1 | `GET` | `/api/candidate/jobs/next` | `JobExplorationController.GetNextCard` | `Candidate` | RF013 a RF015 |
| 6.3.2 | `GET` | `/api/candidate/jobs/{jobId}` | `JobExplorationController.GetDetail` | `Candidate` | RF016 |
| 6.3.3 | `GET` | `/api/candidate/jobs/companies/{companyId}` | `JobExplorationController.GetCompanyPage` | `Candidate` | RF017 |
| 6.3.4 | `POST` | `/api/candidate/jobs/{jobId}/decline` | `JobExplorationController.Decline` | `Candidate` | RF018, RF112 |
| 6.3.5 | `POST` | `/api/candidate/jobs/{jobId}/interest` | `JobExplorationController.ExpressInterest` | `Candidate` | RF019, RF020, RF076, RF113 |
| 6.3.6 | `GET` | `/api/candidate/jobs/quota` | `JobExplorationController.GetQuota` | `Candidate` | RF020 a RF022 |
| 6.4.1 | `POST` | `/api/recruiter/company` | `CompanyController.Register` | `Recruiter` | RF041 a RF043 |
| 6.4.2 | `POST` | `/api/recruiter/company/resubmission` | `CompanyController.Resubmit` | `Recruiter` | RF045 |
| 6.4.3 | `GET` | `/api/recruiter/company/status` | `CompanyController.GetStatus` | `Recruiter` | RF039, RF044 |
| 6.4.4 | `PUT` | `/api/recruiter/company` | `CompanyController.UpdateRegistration` | `ApprovedCompany` | RF049 |
| 6.4.5 | `PUT` | `/api/recruiter/company/page` | `CompanyController.UpdatePage` | `ApprovedCompany` | RF046 |
| 6.4.6 | `PUT` | `/api/recruiter/company/logo` | `CompanyController.UploadLogo` | `ApprovedCompany` | RF047 |
| 6.4.7 | `POST` | `/api/recruiter/company/photos` | `CompanyController.AddGalleryPhoto` | `ApprovedCompany` | RF048 |
| 6.5.1 | `GET` | `/api/recruiter/jobs` | `JobsController.List` | `ApprovedCompany` | RF059, RF060 |
| 6.5.2 | `POST` | `/api/recruiter/jobs` | `JobsController.Create` | `ApprovedCompany` | RF050, RF051 |
| 6.5.3 | `PUT` | `/api/recruiter/jobs/{jobId}` | `JobsController.Update` | `ApprovedCompany` | RF054, RF051, RF059 |
| 6.5.4 | `POST` | `/api/recruiter/jobs/{jobId}/publish` | `JobsController.Publish` | `ApprovedCompany` | RF053, RF059 |
| 6.5.5 | `POST` | `/api/recruiter/jobs/{jobId}/suspend` | `JobsController.Suspend` | `ApprovedCompany` | RF055, RF059 |
| 6.5.6 | `POST` | `/api/recruiter/jobs/{jobId}/republish` | `JobsController.Republish` | `ApprovedCompany` | RF115, RF059 |
| 6.5.7 | `POST` | `/api/recruiter/jobs/{jobId}/close` | `JobsController.Close` | `ApprovedCompany` | RF056, RF059 |
| 6.5.8 | `DELETE` | `/api/recruiter/jobs/{jobId}` | `JobsController.Delete` | `ApprovedCompany` | RF058, RF114, RF059 |
| 6.5.9 | `POST` | `/api/recruiter/jobs/{jobId}/photos` | `JobsController.AddPhoto` | `ApprovedCompany` | RF052 |
| 6.6.1 | `GET` | `/api/recruiter/jobs/{jobId}/waiting-candidates` | `CandidateEvaluationController.ListWaiting` | `ApprovedCompany` | RF060, RF113 |
| 6.6.2 | `GET` | `/api/recruiter/interests/{matchId}/profile` | `CandidateEvaluationController.OpenProfile` | `ApprovedCompany` | RF061, RF062, RF118 |
| 6.6.3 | `GET` | `/api/recruiter/interests/{matchId}/cv` | `CandidateEvaluationController.GetCv` | `ApprovedCompany` | RF061, RNF007 |
| 6.6.4 | `POST` | `/api/recruiter/interests/{matchId}/accept` | `CandidateEvaluationController.Accept` | `ApprovedCompany` | RF063, RF065, RF066, RF068, RF031, RF077, RF117 |
| 6.6.5 | `POST` | `/api/recruiter/interests/{matchId}/reject` | `CandidateEvaluationController.Reject` | `ApprovedCompany` | RF064, RF065, RF117 |
| 6.7.1 | `GET` | `/api/matches` | `MatchesController.List` | `CandidateOrRecruiter`; Recrutador com `ApprovedCompany` | RF024, RF025, RF067, RF039 |
| 6.8.1 | `GET` | `/api/conversations` | `ConversationsController.List` | `CandidateOrRecruiter` | RF027, RF071 |
| 6.8.2 | `GET` | `/api/conversations/{matchId}/messages` | `ConversationsController.Open` | `CandidateOrRecruiter` | RF028, RF072, RF108, RF109 |
| 6.8.3 | `POST` | `/api/conversations/{matchId}/messages` | `ConversationsController.Send` | `CandidateOrRecruiter` | RF026, RF069, RF070, RF032, RF078 |
| 6.8.4 | `POST` | `/api/conversations/{matchId}/close` | `ConversationsController.Close` | `CandidateOrRecruiter` | RF030, RF074 |
| 6.9.1 | `GET` | `/api/notifications` | `NotificationsController.List` | `CandidateOrRecruiter` | RF034, RF036, RF080, RF082 |
| 6.9.2 | `POST` | `/api/notifications/{notificationId}/read` | `NotificationsController.MarkRead` | `CandidateOrRecruiter` | RF035, RF081 |
| 6.10.1 | `GET` | `/api/admin/accounts` | `AdminAccountsController.ListAccounts` | `Admin` | RF085 |
| 6.10.2 | `GET` | `/api/admin/accounts/candidates` | `AdminAccountsController.ListCandidates` | `Admin` | RF088 |
| 6.10.3 | `POST` | `/api/admin/accounts/{userId}/block` | `AdminAccountsController.Block` | `Admin` | RF086, RF075, RF112 |
| 6.10.4 | `POST` | `/api/admin/accounts/candidates/{candidateId}/suspend` | `AdminAccountsController.SuspendCandidate` | `Admin` | RF089, RF075, RF112 |
| 6.10.5 | `POST` | `/api/admin/accounts/{userId}/reactivate` | `AdminAccountsController.Reactivate` | `Admin` | RF087, RF090, RF112 |
| 6.11.1 | `GET` | `/api/admin/companies/pending` | `AdminCompaniesController.ListPending` | `Admin` | RF091 |
| 6.11.2 | `GET` | `/api/admin/companies/{companyId}/registration` | `AdminCompaniesController.GetSubmittedData` | `Admin` | RF092 |
| 6.11.3 | `POST` | `/api/admin/companies/{companyId}/approve` | `AdminCompaniesController.Approve` | `Admin` | RF093, RF079, RF112 |
| 6.11.4 | `POST` | `/api/admin/companies/{companyId}/reject` | `AdminCompaniesController.Reject` | `Admin` | RF094, RF079, RF112 |
| 6.11.5 | `POST` | `/api/admin/companies/{companyId}/suspend` | `AdminCompaniesController.Suspend` | `Admin` | RF095, RF075, RF112 |
| 6.11.6 | `POST` | `/api/admin/companies/{companyId}/reactivate` | `AdminCompaniesController.Reactivate` | `Admin` | RF116, RF112 |
| 6.11.7 | `GET` | `/api/admin/companies` | `AdminCompaniesController.ListCompanies` | `Admin` | RF097 |
| 6.11.8 | `GET` | `/api/admin/companies/published-jobs` | `AdminCompaniesController.ListPublishedJobs` | `Admin` | RF098 |
| 6.12.1 | `GET` | `/api/admin/indicators` | `AdminIndicatorsController.Get` | `Admin` | RF096, RF103 |
| 6.13.1 | `GET` | `/api/reference-lists/locations` | `ReferenceListsController.ListLocations` | Qualquer conta | RF003, RF006, RF041, RF050 |
| 6.13.2 | `GET` | `/api/reference-lists/skills` | `ReferenceListsController.ListSkills` | Qualquer conta | RF008, RF050 |
| 6.13.3 | `GET` | `/api/reference-lists/benefits` | `ReferenceListsController.ListBenefits` | Qualquer conta | RF050 |
| 6.13.4 | `POST` | `/api/reference-lists/skills` | `ReferenceListsController.AddSkill` | `Admin` | RF099, RF112 |
| 6.13.5 | `PUT` | `/api/reference-lists/skills/{skillId}` | `ReferenceListsController.RenameSkill` | `Admin` | RF100, RF112 |
| 6.13.6 | `POST` | `/api/reference-lists/benefits` | `ReferenceListsController.AddBenefit` | `Admin` | RF101, RF112 |
| 6.13.7 | `PUT` | `/api/reference-lists/benefits/{benefitId}` | `ReferenceListsController.RenameBenefit` | `Admin` | RF102, RF112 |

Total: 68 endpoints em 13 controllers. A ligação persistente (secção 7) acrescenta 2 hubs e 3 eventos.

---

## 6. Endpoints por controller

Cada endpoint tem uma tabela com a operação do controller e do serviço que a executa, a autorização, os parâmetros, o corpo do pedido, a resposta de sucesso, os erros específicos e os requisitos, seguida de um exemplo com dados fictícios. Os erros comuns a todos os endpoints autenticados — `401` (ERR-01), `403` por conta bloqueada ou suspensa (ERR-03, ERR-04) e por tipo de conta (ERR-06), `500` (ERR-50) — não são repetidos nas tabelas. Os formatos completos dos DTOs estão na secção 8.

Identificadores fictícios usados nos exemplos, com uma cronologia única, para poderem ser reutilizados na collection Postman de `m3`:

| Elemento | Identificador | Dados fictícios |
| --- | --- | --- |
| Candidato | `3f2a9c1e-7b4d-4e8a-9c21-5d6e7f8a9b01` | Ana Ferreira, `ana.ferreira@exemplo.pt`, 912345678, Guimarães; registada em 2026-10-09 |
| Recrutador | `8b1d2e3f-4a5b-4c6d-8e7f-9a0b1c2d3e02` | `recrutamento@lumen-exemplo.pt`; conta criada em 2026-10-05 |
| Empresa | `c4e5f6a7-b8c9-4d0e-a1f2-b3c4d5e6f703` | Lumen Software, Lda., NIF 509123456, Braga; submetida em 2026-10-05, recusada, submetida de novo e aprovada em 2026-10-06 |
| Outra Empresa | `e1f2a3b4-c5d6-4e7f-8a9b-0c1d2e3f4a14` | Atlântico Logística, S.A. (suspensão e reativação em 6.11.5 e 6.11.6) |
| Administrador | `a0b1c2d3-e4f5-4a6b-8c7d-9e0f1a2b3c04` | `admin@katch-exemplo.pt` |
| Vaga | `5e6f7a8b-9c0d-4e1f-a2b3-c4d5e6f7a805` | Programador Backend .NET, publicada em 2026-10-09 |
| Segunda vaga | `5e6f7a8b-9c0d-4e1f-a2b3-c4d5e6f7a806` | Analista de Dados (ciclo de vida em 6.5.3 a 6.5.7) |
| Match (interesse) | `9d8c7b6a-5f4e-4d3c-b2a1-0f9e8d7c6b06` | Ana Ferreira ↔ Programador Backend .NET: interesse 15:12, aceite 16:02 |
| Outro interesse | `9d8c7b6a-5f4e-4d3c-b2a1-0f9e8d7c6b07` | Candidato em espera sem perfil aberto (6.6.5) |
| Outras contas | `d7e8f9a0-1b2c-4d3e-8f4a-5b6c7d8e9f20` / `f0e1d2c3-b4a5-4968-8776-a5b4c3d2e115` | Recrutador e Candidato (Bruno Sousa) usados nos exemplos de 6.10 |
| Localidades | `1a2b3c4d-5e6f-4a7b-8c9d-0e1f2a3b4c07` / `…4c08` | Braga / Guimarães |
| Competências | `7c6b5a49-3827-4615-a0b9-c8d7e6f5a409` / `…a410` / `…a411` | C# / SQL / Docker |
| Benefício | `6b5a4938-2716-4504-9f8e-d7c6b5a4f312` | Seguro de saúde |

### 6.1. `AuthController` — `/api/auth`

Casos de uso UC01, UC02, UC03 e UC07 (criação de conta). Serviço: `IAuthService`.

#### 6.1.1. `POST /api/auth/candidates` — Registar-se como Candidato

| Campo | Conteúdo |
| --- | --- |
| Operação | `AuthController.RegisterCandidate` → `IAuthService.RegisterCandidateAsync` |
| Autorização | Anónimo |
| Corpo do pedido | `RegisterCandidateRequest`: `fullName`, `email`, `phoneNumber`, `locationId`, `password`, `acceptTerms` |
| Resposta de sucesso | `201 Created` — `LoginResponse`; a conta fica `ACTIVE` e a sessão iniciada (RF005). `Location: /api/candidate/profile`. |
| Erros | `400` ERR-10: campo vazio, correio fora do formato ou já registado, telefone sem 9 algarismos, palavra-passe fora do P05, localidade fora da lista, `acceptTerms` diferente de `true` (RF004). Todos os campos em falta são indicados no mesmo pedido. |
| Regras | A palavra-passe é guardada só como resumo (RNF003). A quota começa em 10 interesses. O correio é comparado sem distinção de maiúsculas. |
| Requisitos | RF003, RF004, RF005; RNF003, RNF008; P04, P05 · UC03 |
| Nota | A lista de localidades exige sessão iniciada (6.13.1), pelo que, com o modelo de classes v01, a aplicação móvel não a consegue obter antes do registo. Ponto em aberto PA-04. |

```http
POST /api/auth/candidates HTTP/1.1
Content-Type: application/json

{
  "fullName": "Ana Ferreira",
  "email": "ana.ferreira@exemplo.pt",
  "phoneNumber": "912345678",
  "locationId": "1a2b3c4d-5e6f-4a7b-8c9d-0e1f2a3b4c08",
  "password": "katch2026",
  "acceptTerms": true
}

HTTP/1.1 201 Created
Location: /api/candidate/profile
Content-Type: application/json

{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.<carga>.<assinatura>",
  "expiresAt": "2026-10-09T22:30:00Z",
  "userType": "CANDIDATE",
  "userId": "3f2a9c1e-7b4d-4e8a-9c21-5d6e7f8a9b01"
}
```

#### 6.1.2. `POST /api/auth/recruiters` — Criar conta de Recrutador

| Campo | Conteúdo |
| --- | --- |
| Operação | `AuthController.CreateRecruiterAccount` → `IAuthService.CreateRecruiterAccountAsync` |
| Autorização | Anónimo |
| Corpo do pedido | `CreateRecruiterAccountRequest`: `email`, `password`, `acceptTerms` |
| Resposta de sucesso | `201 Created` — `LoginResponse`. A conta fica ativa sem Empresa; as operações `ApprovedCompany` devolvem ERR-07 até à aprovação. Sem `Location`: o estado da Empresa (6.4.3) só existe depois do registo da Empresa. |
| Erros | `400` ERR-10: correio fora do formato ou já registado, palavra-passe fora do P05, condições não aceites. |
| Requisitos | RF037; RNF003; P04, P05 · UC07 |

```http
POST /api/auth/recruiters HTTP/1.1
Content-Type: application/json

{
  "email": "recrutamento@lumen-exemplo.pt",
  "password": "Lumen2026rh",
  "acceptTerms": true
}

HTTP/1.1 201 Created
Content-Type: application/json

{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.<carga>.<assinatura>",
  "expiresAt": "2026-10-05T17:55:00Z",
  "userType": "RECRUITER",
  "userId": "8b1d2e3f-4a5b-4c6d-8e7f-9a0b1c2d3e02"
}
```

#### 6.1.3. `POST /api/auth/login` — Iniciar sessão

| Campo | Conteúdo |
| --- | --- |
| Operação | `AuthController.Login` → `IAuthService.LoginAsync(request, ClientApp)` |
| Autorização | Anónimo |
| Cabeçalhos | `X-Client-App`: `mobile` ou `web` (obrigatório, secção 3.2) |
| Corpo do pedido | `LoginRequest`: `email`, `password` |
| Resposta de sucesso | `200 OK` — `LoginResponse` |
| Erros | `400` ERR-10 (campos vazios ou `X-Client-App` em falta); `401` ERR-02 (credenciais erradas, sempre a mesma mensagem); `403` ERR-03, ERR-04 (conta bloqueada ou suspensa); `403` ERR-05 (ponto de acesso errado). |
| Regras | Ordem das verificações: credenciais, estado da conta, ponto de acesso. O estado só é revelado a quem apresenta a palavra-passe correta. |
| Requisitos | RF001, RF038, RF083; RNF005, RNF009 · UC01 |

```http
POST /api/auth/login HTTP/1.1
X-Client-App: mobile
Content-Type: application/json

{ "email": "ana.ferreira@exemplo.pt", "password": "katch2026" }

HTTP/1.1 200 OK
Content-Type: application/json

{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.<carga>.<assinatura>",
  "expiresAt": "2026-10-09T22:45:00Z",
  "userType": "CANDIDATE",
  "userId": "3f2a9c1e-7b4d-4e8a-9c21-5d6e7f8a9b01"
}
```

Exemplo de rejeição por credenciais erradas:

```http
HTTP/1.1 401 Unauthorized
Content-Type: application/json

{ "message": "O endereço de correio eletrónico ou a palavra-passe estão incorretos.", "fields": [] }
```

#### 6.1.4. `POST /api/auth/logout` — Terminar sessão

| Campo | Conteúdo |
| --- | --- |
| Operação | `AuthController.Logout` (sem serviço) |
| Autorização | Qualquer conta |
| Resposta de sucesso | `204 No Content`. O cliente descarta a credencial e fecha as ligações aos hubs. |
| Requisitos | RF105, RF106, RF107 · UC01 |

```http
POST /api/auth/logout HTTP/1.1
Authorization: Bearer <credencial>

HTTP/1.1 204 No Content
```

#### 6.1.5. `PUT /api/auth/password` — Alterar a própria palavra-passe

| Campo | Conteúdo |
| --- | --- |
| Operação | `AuthController.ChangePassword` → `IAuthService.ChangePasswordAsync` |
| Autorização | Qualquer conta |
| Corpo do pedido | `ChangePasswordRequest`: `currentPassword`, `newPassword` |
| Resposta de sucesso | `204 No Content`. O início de sessão seguinte só aceita a nova palavra-passe; a credencial atual mantém-se válida até expirar. |
| Erros | `400` ERR-10: `currentPassword` errada (com `400`, e não `401`, para o cliente não interpretar o erro como sessão expirada); `newPassword` fora do P05. |
| Requisitos | RF002, RF040, RF084; RNF003; P05 · UC02 |

```http
PUT /api/auth/password HTTP/1.1
Authorization: Bearer <credencial>
Content-Type: application/json

{ "currentPassword": "katch2026", "newPassword": "katch" }

HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "message": "Os dados enviados não cumprem a validação automática.",
  "fields": [
    { "field": "newPassword", "reason": "A palavra-passe tem de ter pelo menos 8 caracteres, uma letra e um algarismo." }
  ]
}
```

### 6.2. `CandidateProfileController` — `/api/candidate/profile`

Caso de uso UC04 (e UC05, A3, nas preferências). Serviço: `ICandidateProfileService`. Autorização: `Candidate` em todos os endpoints.

#### 6.2.1. `GET /api/candidate/profile` — Consultar o perfil profissional

| Campo | Conteúdo |
| --- | --- |
| Operação | `CandidateProfileController.Get` → `ICandidateProfileService.GetAsync` |
| Resposta de sucesso | `200 OK` — `CandidateProfileDto`, com competências, hiperligações e preferências. |
| Requisitos | RF006 a RF012 · UC04 |

```http
GET /api/candidate/profile HTTP/1.1
Authorization: Bearer <credencial>

HTTP/1.1 200 OK
Content-Type: application/json

{
  "candidateId": "3f2a9c1e-7b4d-4e8a-9c21-5d6e7f8a9b01",
  "fullName": "Ana Ferreira",
  "email": "ana.ferreira@exemplo.pt",
  "phoneNumber": "912345678",
  "locationId": "1a2b3c4d-5e6f-4a7b-8c9d-0e1f2a3b4c08",
  "locationName": "Guimarães (Braga)",
  "desiredRole": "Programadora backend",
  "availability": "ONE_MONTH",
  "experienceSummary": "Dois anos de desenvolvimento de APIs REST em C# e PostgreSQL.",
  "photoUrl": "<URL do ficheiro>",
  "hasCv": true,
  "links": ["https://github.com/ana-ferreira-exemplo"],
  "skills": [
    { "candidateSkillId": "b1c2d3e4-f5a6-4b7c-8d9e-0f1a2b3c4d14", "name": "C#", "isCustom": false },
    { "candidateSkillId": "b1c2d3e4-f5a6-4b7c-8d9e-0f1a2b3c4d15", "name": "Entity Framework", "isCustom": true }
  ],
  "preferences": {
    "maxDistanceKm": 50,
    "minSalaryExpectation": 1400.00,
    "workModes": ["HYBRID", "REMOTE"],
    "contractTypes": ["PERMANENT"]
  }
}
```

#### 6.2.2. `PUT /api/candidate/profile` — Gravar os dados do perfil profissional

| Campo | Conteúdo |
| --- | --- |
| Operação | `CandidateProfileController.Update` → `ICandidateProfileService.UpdateAsync` |
| Corpo do pedido | `UpdateCandidateProfileRequest`: `desiredRole`, `locationId`, `availability`, `experienceSummary`. Substitui os quatro campos; `null` limpa os opcionais. |
| Resposta de sucesso | `200 OK` — `CandidateProfileDto` atualizado. A nova localidade passa a ser usada no cálculo da distância do cartão seguinte (RF015). |
| Erros | `400` ERR-10: localidade fora da lista, `experienceSummary` acima de 1000 caracteres, `desiredRole` acima de 150, `availability` fora da enumeração. |
| Requisitos | RF006; P08, P14 · UC04 |

```http
PUT /api/candidate/profile HTTP/1.1
Authorization: Bearer <credencial>
Content-Type: application/json

{
  "desiredRole": "Programadora backend",
  "locationId": "1a2b3c4d-5e6f-4a7b-8c9d-0e1f2a3b4c08",
  "availability": "ONE_MONTH",
  "experienceSummary": "Dois anos de desenvolvimento de APIs REST em C# e PostgreSQL."
}

HTTP/1.1 200 OK
Content-Type: application/json

{
  "candidateId": "3f2a9c1e-7b4d-4e8a-9c21-5d6e7f8a9b01",
  "fullName": "Ana Ferreira",
  "email": "ana.ferreira@exemplo.pt",
  "phoneNumber": "912345678",
  "locationId": "1a2b3c4d-5e6f-4a7b-8c9d-0e1f2a3b4c08",
  "locationName": "Guimarães (Braga)",
  "desiredRole": "Programadora backend",
  "availability": "ONE_MONTH",
  "experienceSummary": "Dois anos de desenvolvimento de APIs REST em C# e PostgreSQL.",
  "photoUrl": "<URL do ficheiro>",
  "hasCv": true,
  "links": [
    "https://github.com/ana-ferreira-exemplo"
  ],
  "skills": [
    {
      "candidateSkillId": "b1c2d3e4-f5a6-4b7c-8d9e-0f1a2b3c4d14",
      "name": "C#",
      "isCustom": false
    },
    {
      "candidateSkillId": "b1c2d3e4-f5a6-4b7c-8d9e-0f1a2b3c4d15",
      "name": "Entity Framework",
      "isCustom": true
    }
  ],
  "preferences": {
    "maxDistanceKm": 50,
    "minSalaryExpectation": 1400.00,
    "workModes": [
      "HYBRID",
      "REMOTE"
    ],
    "contractTypes": [
      "PERMANENT"
    ]
  }
}
```

#### 6.2.3. `PUT /api/candidate/profile/preferences` — Definir ou ajustar as preferências de procura

| Campo | Conteúdo |
| --- | --- |
| Operação | `CandidateProfileController.UpdatePreferences` → `ICandidateProfileService.UpdatePreferencesAsync` |
| Corpo do pedido | `SearchPreferencesDto`: `maxDistanceKm`, `minSalaryExpectation`, `workModes`, `contractTypes`. `null` ou lista vazia = sem filtro nesse critério (modelo de dados, DM-11). |
| Resposta de sucesso | `200 OK` — `SearchPreferencesDto` gravado. Aplica-se ao cartão seguinte pedido em 6.3.1 (RF023). |
| Erros | `400` ERR-10: `maxDistanceKm` fora de 1 a 500 ou não inteiro (UC05, E4); `minSalaryExpectation` negativo; valores fora das enumerações. As preferências anteriores mantêm-se. |
| Requisitos | RF007, RF023; P06, P09, P16 · UC04, UC05 (A3) |

```http
PUT /api/candidate/profile/preferences HTTP/1.1
Authorization: Bearer <credencial>
Content-Type: application/json

{ "maxDistanceKm": 30, "minSalaryExpectation": 1500.00, "workModes": ["ON_SITE", "HYBRID"], "contractTypes": ["PERMANENT", "FIXED_TERM"] }

HTTP/1.1 200 OK
Content-Type: application/json

{ "maxDistanceKm": 30, "minSalaryExpectation": 1500.00, "workModes": ["ON_SITE", "HYBRID"], "contractTypes": ["PERMANENT", "FIXED_TERM"] }
```

#### 6.2.4. `POST /api/candidate/profile/skills` — Acrescentar uma competência

| Campo | Conteúdo |
| --- | --- |
| Operação | `CandidateProfileController.AddSkill` → `ICandidateProfileService.AddSkillAsync` |
| Corpo do pedido | `AddSkillRequest`: exatamente um de `skillId` (competência da lista, RF008) e `customLabel` (opção «Outro», RF009). |
| Resposta de sucesso | `201 Created` — `CandidateProfileDto` atualizado. |
| Erros | `400` ERR-10: os dois campos preenchidos ou nenhum; `skillId` fora da lista; `customLabel` fora do P07; competência da lista já associada ao perfil. |
| Requisitos | RF008, RF009; P07 · UC04 |

```http
POST /api/candidate/profile/skills HTTP/1.1
Authorization: Bearer <credencial>
Content-Type: application/json

{ "skillId": null, "customLabel": "Blazor" }

HTTP/1.1 201 Created
Content-Type: application/json

{
  "candidateId": "3f2a9c1e-7b4d-4e8a-9c21-5d6e7f8a9b01",
  "fullName": "Ana Ferreira",
  "email": "ana.ferreira@exemplo.pt",
  "phoneNumber": "912345678",
  "locationId": "1a2b3c4d-5e6f-4a7b-8c9d-0e1f2a3b4c08",
  "locationName": "Guimarães (Braga)",
  "desiredRole": "Programadora backend",
  "availability": "ONE_MONTH",
  "experienceSummary": "Dois anos de desenvolvimento de APIs REST em C# e PostgreSQL.",
  "photoUrl": "<URL do ficheiro>",
  "hasCv": true,
  "links": [
    "https://github.com/ana-ferreira-exemplo"
  ],
  "skills": [
    {
      "candidateSkillId": "b1c2d3e4-f5a6-4b7c-8d9e-0f1a2b3c4d14",
      "name": "C#",
      "isCustom": false
    },
    {
      "candidateSkillId": "b1c2d3e4-f5a6-4b7c-8d9e-0f1a2b3c4d15",
      "name": "Entity Framework",
      "isCustom": true
    },
    {
      "candidateSkillId": "b1c2d3e4-f5a6-4b7c-8d9e-0f1a2b3c4d16",
      "name": "Blazor",
      "isCustom": true
    }
  ],
  "preferences": {
    "maxDistanceKm": 50,
    "minSalaryExpectation": 1400.00,
    "workModes": [
      "HYBRID",
      "REMOTE"
    ],
    "contractTypes": [
      "PERMANENT"
    ]
  }
}
```

#### 6.2.5. `DELETE /api/candidate/profile/skills/{candidateSkillId}` — Retirar uma competência

| Campo | Conteúdo |
| --- | --- |
| Operação | `CandidateProfileController.RemoveSkill` → `ICandidateProfileService.RemoveSkillAsync` |
| Parâmetros | `candidateSkillId` (rota, `Guid`): `ProfileSkillDto.candidateSkillId` |
| Resposta de sucesso | `204 No Content` |
| Erros | `404` ERR-20: a competência não existe no perfil do Candidato. |
| Requisitos | RF008, RF009 · UC04 |

```http
DELETE /api/candidate/profile/skills/b1c2d3e4-f5a6-4b7c-8d9e-0f1a2b3c4d15 HTTP/1.1
Authorization: Bearer <credencial>

HTTP/1.1 204 No Content
```

#### 6.2.6. `PUT /api/candidate/profile/links` — Indicar as hiperligações profissionais

| Campo | Conteúdo |
| --- | --- |
| Operação | `CandidateProfileController.SetLinks` → `ICandidateProfileService.SetLinksAsync` |
| Corpo do pedido | `SetLinksRequest`: `urls`, com 0 a 3 endereços, pela ordem de apresentação. Substitui a lista anterior; `[]` apaga todas. |
| Resposta de sucesso | `200 OK` — `CandidateProfileDto` atualizado. Os endereços são guardados tal como indicados, sem importar conteúdo. |
| Erros | `400` ERR-10: mais de 3 endereços; endereço sem `http://` ou `https://`; endereço acima de 500 caracteres. |
| Requisitos | RF012; P10 · UC04 |

```http
PUT /api/candidate/profile/links HTTP/1.1
Authorization: Bearer <credencial>
Content-Type: application/json

{ "urls": ["https://github.com/ana-ferreira-exemplo", "www.portfolio-exemplo.pt"] }

HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "message": "Os dados enviados não cumprem a validação automática.",
  "fields": [ { "field": "urls[1]", "reason": "O endereço tem de começar por http:// ou https://." } ]
}
```

#### 6.2.7. `PUT /api/candidate/profile/photo` — Carregar a fotografia de perfil

| Campo | Conteúdo |
| --- | --- |
| Operação | `CandidateProfileController.UploadPhoto` → `ICandidateProfileService.UploadPhotoAsync` |
| Corpo do pedido | `multipart/form-data`, campo `file`: JPEG ou PNG até 5 MB. |
| Resposta de sucesso | `204 No Content`, com `Location` = URL da nova fotografia. A fotografia anterior é eliminada do armazenamento. |
| Erros | `400` ERR-11 (formato, conteúdo ou dimensão); `413` ERR-12; `415` ERR-13. |
| Requisitos | RF010; RNF015, RNF016; P01 · UC04 |

```http
PUT /api/candidate/profile/photo HTTP/1.1
Authorization: Bearer <credencial>
Content-Type: multipart/form-data; boundary=----katch

------katch
Content-Disposition: form-data; name="file"; filename="ana.jpg"
Content-Type: image/jpeg

<conteúdo binário>
------katch--

HTTP/1.1 204 No Content
Location: <URL do ficheiro>
```

#### 6.2.8. `PUT /api/candidate/profile/cv` — Enviar o curriculum vitae

| Campo | Conteúdo |
| --- | --- |
| Operação | `CandidateProfileController.UploadCv` → `ICandidateProfileService.UploadCvAsync` |
| Corpo do pedido | `multipart/form-data`, campo `file`: PDF até 5 MB. |
| Resposta de sucesso | `204 No Content`, sem `Location` (o CV não tem URL; `hasCv` passa a `true`). Substitui o CV anterior. O Candidato não consegue voltar a obter o CV enviado (PA-10). |
| Erros | `400` ERR-11; `413` ERR-12; `415` ERR-13. |
| Requisitos | RF011; RNF007, RNF015, RNF016; P02 · UC04 |

```http
PUT /api/candidate/profile/cv HTTP/1.1
Authorization: Bearer <credencial>
Content-Type: multipart/form-data; boundary=----katch

------katch
Content-Disposition: form-data; name="file"; filename="cv-ana.pdf"
Content-Type: application/pdf

<conteúdo binário>
------katch--

HTTP/1.1 204 No Content
```

### 6.3. `JobExplorationController` — `/api/candidate/jobs`

Casos de uso UC05 e UC06. Serviço: `IJobExplorationService`. Autorização: `Candidate` em todos os endpoints.

#### 6.3.1. `GET /api/candidate/jobs/next` — Obter o cartão de vaga seguinte

| Campo | Conteúdo |
| --- | --- |
| Operação | `JobExplorationController.GetNextCard` → `IJobExplorationService.GetNextCardAsync` |
| Resposta de sucesso | `200 OK` — `JobCardDto`; ou `204 No Content` quando nenhuma vaga cumpre o filtro (UC05, A6). |
| Regras | Filtro do RF014: vaga `PUBLISHED` e não eliminada; Empresa `APPROVED`; sem resposta anterior do Candidato; distância ≤ `maxDistanceKm`, exceto vagas `REMOTE`; `maxSalary` ≥ `minSalaryExpectation`; regime e contrato nas preferências. `distanceKm` é a distância em linha reta (Haversine) entre as localidades do Candidato e da vaga, arredondada às unidades (RF015). Com a quota esgotada, os cartões continuam a ser apresentados (UC05, A4); o estado da quota obtém-se em 6.3.6. A ordem dos cartões não é fixada pelos requisitos; fica definida na implementação. |
| Desempenho | Resposta em 2 segundos, com os dados de demonstração (RNF002, P19). |
| Requisitos | RF013, RF014, RF015; RNF002 · UC05 |

```http
GET /api/candidate/jobs/next HTTP/1.1
Authorization: Bearer <credencial>

HTTP/1.1 200 OK
Content-Type: application/json

{
  "jobId": "5e6f7a8b-9c0d-4e1f-a2b3-c4d5e6f7a805",
  "title": "Programador Backend .NET",
  "companyName": "Lumen Software, Lda.",
  "logoUrl": "<URL do ficheiro>",
  "minSalary": 1600.00,
  "maxSalary": 2100.00,
  "locationName": "Braga (Braga)",
  "distanceKm": 22,
  "contractType": "PERMANENT",
  "workMode": "HYBRID",
  "isUrgent": true,
  "photoUrls": ["<URL do ficheiro>"]
}
```

#### 6.3.2. `GET /api/candidate/jobs/{jobId}` — Consultar o detalhe de uma vaga

| Campo | Conteúdo |
| --- | --- |
| Operação | `JobExplorationController.GetDetail` → `IJobExplorationService.GetDetailAsync` |
| Parâmetros | `jobId` (rota, `Guid`) |
| Resposta de sucesso | `200 OK` — `JobDetailDto`: o cartão, a descrição completa, as competências, os benefícios e o `companyId` para a página da Empresa. |
| Erros | `404` ERR-20: vaga inexistente, eliminada, não publicada ou de Empresa não aprovada. |
| Requisitos | RF016 · UC05 (A2) |

```http
GET /api/candidate/jobs/5e6f7a8b-9c0d-4e1f-a2b3-c4d5e6f7a805 HTTP/1.1
Authorization: Bearer <credencial>

HTTP/1.1 200 OK
Content-Type: application/json

{
  "card": {
    "jobId": "5e6f7a8b-9c0d-4e1f-a2b3-c4d5e6f7a805",
    "title": "Programador Backend .NET",
    "companyName": "Lumen Software, Lda.",
    "logoUrl": "<URL do ficheiro>",
    "minSalary": 1600.00,
    "maxSalary": 2100.00,
    "locationName": "Braga (Braga)",
    "distanceKm": 22,
    "contractType": "PERMANENT",
    "workMode": "HYBRID",
    "isUrgent": true,
    "photoUrls": ["<URL do ficheiro>"]
  },
  "description": "Desenvolvimento e manutenção de APIs REST em ASP.NET Core para clientes do setor da logística.",
  "skills": ["C#", "SQL", "Docker"],
  "benefits": ["Seguro de saúde"],
  "companyId": "c4e5f6a7-b8c9-4d0e-a1f2-b3c4d5e6f703"
}
```

#### 6.3.3. `GET /api/candidate/jobs/companies/{companyId}` — Consultar a página de apresentação da Empresa

| Campo | Conteúdo |
| --- | --- |
| Operação | `JobExplorationController.GetCompanyPage` → `IJobExplorationService.GetCompanyPageAsync` |
| Parâmetros | `companyId` (rota, `Guid`) |
| Resposta de sucesso | `200 OK` — `CompanyPageDto`: designação, descrição, sítio na Internet, logótipo e galeria. Os contactos da Empresa não são incluídos (só depois do match, RF025). |
| Erros | `404` ERR-20: Empresa inexistente ou não aprovada. |
| Requisitos | RF017 · UC06 |

```http
GET /api/candidate/jobs/companies/c4e5f6a7-b8c9-4d0e-a1f2-b3c4d5e6f703 HTTP/1.1
Authorization: Bearer <credencial>

HTTP/1.1 200 OK
Content-Type: application/json

{
  "companyId": "c4e5f6a7-b8c9-4d0e-a1f2-b3c4d5e6f703",
  "companyName": "Lumen Software, Lda.",
  "description": "Software de gestão de frotas e armazéns, com equipas em Braga e no Porto.",
  "website": "https://www.lumen-exemplo.pt",
  "logoUrl": "<URL do ficheiro>",
  "photoUrls": ["<URL do ficheiro>", "<URL do ficheiro>"]
}
```

#### 6.3.4. `POST /api/candidate/jobs/{jobId}/decline` — Recusar uma vaga

| Campo | Conteúdo |
| --- | --- |
| Operação | `JobExplorationController.Decline` → `IJobExplorationService.DeclineAsync` (`Match.Decline`) |
| Parâmetros | `jobId` (rota, `Guid`) |
| Resposta de sucesso | `204 No Content`. A recusa fica registada na linha de `match` com o Candidato e a data e hora (RF112), sem consumo da quota e sem limite de recusas. |
| Erros | `404` ERR-20 (vaga inexistente); `409` ERR-32 (já respondeu a esta vaga); `409` ERR-33 (a vaga deixou de cumprir o RF014). |
| Requisitos | RF018, RF112 · UC05 (A1) |

```http
POST /api/candidate/jobs/5e6f7a8b-9c0d-4e1f-a2b3-c4d5e6f7a805/decline HTTP/1.1
Authorization: Bearer <credencial>

HTTP/1.1 204 No Content
```

#### 6.3.5. `POST /api/candidate/jobs/{jobId}/interest` — Manifestar interesse numa vaga

| Campo | Conteúdo |
| --- | --- |
| Operação | `JobExplorationController.ExpressInterest` → `IJobExplorationService.ExpressInterestAsync` (`Match.ExpressInterest`, `Candidate.ConsumeInterest`) |
| Parâmetros | `jobId` (rota, `Guid`) |
| Resposta de sucesso | `200 OK` — `QuotaDto` atualizado. Se este interesse esgotou a quota, `blockedUntil` traz o fim do período de bloqueio (24 horas depois). |
| Efeitos | Numa transação: linha de `match` em espera (`WAITING`), sem prazo de expiração (RF113); quota reduzida em 1; notificação `NEW_INTEREST` ao Recrutador, entregue pela ligação persistente depois de confirmada a transação (RF076). Dois interesses simultâneos do mesmo Candidato são serializados pelo `xmin` de `candidate`: o serviço repete o segundo com o valor atualizado, e o cliente nunca recebe ERR-41 neste endpoint. |
| Erros | `404` ERR-20; `409` ERR-31 (quota esgotada, com a data de reposição); `409` ERR-32 (segunda resposta à vaga); `409` ERR-33 (vaga já não disponível). Em qualquer erro, nada é registado e a quota não muda. |
| Requisitos | RF019, RF020, RF076, RF112, RF113; P11 · UC05 |

```http
POST /api/candidate/jobs/5e6f7a8b-9c0d-4e1f-a2b3-c4d5e6f7a805/interest HTTP/1.1
Authorization: Bearer <credencial>

HTTP/1.1 200 OK
Content-Type: application/json

{ "available": 9, "blockedUntil": null }
```

Rejeição de um interesse no período de bloqueio (noutro momento, com a quota esgotada):

```http
HTTP/1.1 409 Conflict
Content-Type: application/json

{ "message": "Esgotou a quota de interesses. Pode voltar a manifestar interesse dentro de 23 h 47 min.", "fields": [] }
```

#### 6.3.6. `GET /api/candidate/jobs/quota` — Consultar os interesses disponíveis

| Campo | Conteúdo |
| --- | --- |
| Operação | `JobExplorationController.GetQuota` → `IJobExplorationService.GetQuotaAsync` |
| Resposta de sucesso | `200 OK` — `QuotaDto`: `available` (0 a 10) e `blockedUntil` (`null` fora do período de bloqueio). O cliente calcula o tempo em falta a partir de `blockedUntil` (RF020). |
| Regras | Se o período de bloqueio já terminou e a tarefa periódica ainda não correu, o serviço aplica `Candidate.RestoreQuotaIfDue` antes de responder, com a notificação `INTEREST_QUOTA_RESTORED`, para que o valor devolvido seja sempre o correto (RF021, RF033). A mesma verificação é feita em 6.3.5 antes de avaliar a quota. |
| Requisitos | RF020, RF021, RF022; P11 · UC05 |

```http
GET /api/candidate/jobs/quota HTTP/1.1
Authorization: Bearer <credencial>

HTTP/1.1 200 OK
Content-Type: application/json

{ "available": 9, "blockedUntil": null }
```

### 6.4. `CompanyController` — `/api/recruiter/company`

Casos de uso UC07 e UC08. Serviço: `ICompanyService`. A Empresa é sempre a do Recrutador da credencial (`company.recruiter_id`); não há identificador da Empresa na rota.

#### 6.4.1. `POST /api/recruiter/company` — Registar a Empresa

| Campo | Conteúdo |
| --- | --- |
| Operação | `CompanyController.Register` → `ICompanyService.RegisterAsync` |
| Autorização | `Recruiter` |
| Corpo do pedido | `CompanyRegistrationRequest`: `companyName`, `taxId`, `industry`, `address`, `locationId`, `contactEmail`, `contactPhone`, `responsibleName` |
| Resposta de sucesso | `201 Created` — `CompanyStatusDto` com `status = PENDING` (RF043). `Location: /api/recruiter/company/status`. Regista `COMPANY_SUBMITTED` no `operation_log` (RF112). |
| Erros | `400` ERR-10: campo vazio, correio fora do formato, telefone ou NIF sem 9 algarismos, NIF já registado, setor ou localidade fora das listas (RF042); `409` ERR-40: o Recrutador já tem Empresa. |
| Requisitos | RF041, RF042, RF043, RF112; P04, P15 · UC07 |

```http
POST /api/recruiter/company HTTP/1.1
Authorization: Bearer <credencial>
Content-Type: application/json

{
  "companyName": "Lumen Software, Lda.",
  "taxId": "509123456",
  "industry": "TECHNOLOGY",
  "address": "Avenida da Liberdade, 100, 4710-000 Braga",
  "locationId": "1a2b3c4d-5e6f-4a7b-8c9d-0e1f2a3b4c07",
  "contactEmail": "geral@lumen-exemplo.pt",
  "contactPhone": "253000111",
  "responsibleName": "Rui Matos"
}

HTTP/1.1 201 Created
Location: /api/recruiter/company/status
Content-Type: application/json

{
  "companyId": "c4e5f6a7-b8c9-4d0e-a1f2-b3c4d5e6f703",
  "status": "PENDING",
  "rejectionReason": null,
  "submittedAt": "2026-10-05T10:00:00Z"
}
```

#### 6.4.2. `POST /api/recruiter/company/resubmission` — Corrigir e submeter novamente o pedido

| Campo | Conteúdo |
| --- | --- |
| Operação | `CompanyController.Resubmit` → `ICompanyService.ResubmitAsync` (`Company.Resubmit`) |
| Autorização | `Recruiter` |
| Corpo do pedido | `CompanyRegistrationRequest` completo, com os dados corrigidos. |
| Resposta de sucesso | `200 OK` — `CompanyStatusDto` com `status = PENDING`, `rejectionReason = null` e `submittedAt` atualizado. Regista `COMPANY_SUBMITTED` (RF112). |
| Erros | `400` ERR-10 (validação automática, como em 6.4.1; o NIF pode ser corrigido, porque a Empresa nunca foi aprovada); `404` ERR-20 (sem Empresa registada); `409` ERR-30 (Empresa fora do estado `REJECTED`). |
| Requisitos | RF045, RF042, RF112 · UC07 |

```http
POST /api/recruiter/company/resubmission HTTP/1.1
Authorization: Bearer <credencial>
Content-Type: application/json

{
  "companyName": "Lumen Software, Lda.",
  "taxId": "509123456",
  "industry": "TECHNOLOGY",
  "address": "Avenida da Liberdade, 102, 4710-000 Braga",
  "locationId": "1a2b3c4d-5e6f-4a7b-8c9d-0e1f2a3b4c07",
  "contactEmail": "geral@lumen-exemplo.pt",
  "contactPhone": "253000111",
  "responsibleName": "Rui Matos"
}

HTTP/1.1 200 OK
Content-Type: application/json

{ "companyId": "c4e5f6a7-b8c9-4d0e-a1f2-b3c4d5e6f703", "status": "PENDING", "rejectionReason": null, "submittedAt": "2026-10-06T09:05:00Z" }
```

#### 6.4.3. `GET /api/recruiter/company/status` — Consultar o estado do pedido de registo

| Campo | Conteúdo |
| --- | --- |
| Operação | `CompanyController.GetStatus` → `ICompanyService.GetStatusAsync` |
| Autorização | `Recruiter` |
| Resposta de sucesso | `200 OK` — `CompanyStatusDto`. `rejectionReason` só no estado `REJECTED`. É o endpoint que a área de gestão web consulta para indicar o estado da Empresa quando as operações reservadas estão vedadas (RF039). |
| Erros | `404` ERR-20: o Recrutador ainda não registou a Empresa (a área de gestão web apresenta o formulário de 6.4.1). |
| Requisitos | RF044, RF039 · UC07 |

```http
GET /api/recruiter/company/status HTTP/1.1
Authorization: Bearer <credencial>

HTTP/1.1 200 OK
Content-Type: application/json

{
  "companyId": "c4e5f6a7-b8c9-4d0e-a1f2-b3c4d5e6f703",
  "status": "REJECTED",
  "rejectionReason": "A morada indicada não corresponde à certidão permanente da Empresa.",
  "submittedAt": "2026-10-05T10:00:00Z"
}
```

#### 6.4.4. `PUT /api/recruiter/company` — Alterar os dados de registo da Empresa aprovada

| Campo | Conteúdo |
| --- | --- |
| Operação | `CompanyController.UpdateRegistration` → `ICompanyService.UpdateRegistrationAsync` (`Company.UpdateRegistration`) |
| Autorização | `ApprovedCompany` |
| Corpo do pedido | `CompanyRegistrationRequest` completo; `taxId` tem de ser igual ao registado. |
| Resposta de sucesso | `200 OK` — `CompanyStatusDto`; a Empresa mantém-se `APPROVED`. Regista `COMPANY_UPDATED` (RF112). |
| Erros | `400` ERR-10: validação automática; `taxId` diferente do registado («O número de identificação fiscal não pode ser alterado depois da aprovação.»); `403` ERR-07. |
| Requisitos | RF049, RF112 · UC08 |

```http
PUT /api/recruiter/company HTTP/1.1
Authorization: Bearer <credencial>
Content-Type: application/json

{
  "companyName": "Lumen Software, Lda.",
  "taxId": "509123456",
  "industry": "TECHNOLOGY",
  "address": "Rua Nova de Santa Cruz, 15, 4710-409 Braga",
  "locationId": "1a2b3c4d-5e6f-4a7b-8c9d-0e1f2a3b4c07",
  "contactEmail": "recrutamento@lumen-exemplo.pt",
  "contactPhone": "253000222",
  "responsibleName": "Rui Matos"
}

HTTP/1.1 200 OK
Content-Type: application/json

{ "companyId": "c4e5f6a7-b8c9-4d0e-a1f2-b3c4d5e6f703", "status": "APPROVED", "rejectionReason": null, "submittedAt": "2026-10-06T09:05:00Z" }
```

#### 6.4.5. `PUT /api/recruiter/company/page` — Gravar a página de apresentação

| Campo | Conteúdo |
| --- | --- |
| Operação | `CompanyController.UpdatePage` → `ICompanyService.UpdatePageAsync` |
| Autorização | `ApprovedCompany` |
| Corpo do pedido | `CompanyPageRequest`: `description` (até 1000 caracteres), `website` (`http://` ou `https://`). `null` limpa o campo. |
| Resposta de sucesso | `200 OK` — `CompanyPageDto` |
| Erros | `400` ERR-10; `403` ERR-07. |
| Requisitos | RF046; P08, P10 · UC08 |

```http
PUT /api/recruiter/company/page HTTP/1.1
Authorization: Bearer <credencial>
Content-Type: application/json

{ "description": "Software de gestão de frotas e armazéns, com equipas em Braga e no Porto.", "website": "https://www.lumen-exemplo.pt" }

HTTP/1.1 200 OK
Content-Type: application/json

{
  "companyId": "c4e5f6a7-b8c9-4d0e-a1f2-b3c4d5e6f703",
  "companyName": "Lumen Software, Lda.",
  "description": "Software de gestão de frotas e armazéns, com equipas em Braga e no Porto.",
  "website": "https://www.lumen-exemplo.pt",
  "logoUrl": null,
  "photoUrls": []
}
```

#### 6.4.6. `PUT /api/recruiter/company/logo` — Carregar o logótipo

| Campo | Conteúdo |
| --- | --- |
| Operação | `CompanyController.UploadLogo` → `ICompanyService.UploadLogoAsync` |
| Autorização | `ApprovedCompany` |
| Corpo do pedido | `multipart/form-data`, campo `file`: JPEG ou PNG até 2 MB. |
| Resposta de sucesso | `204 No Content`, com `Location` = URL do logótipo. Substitui o anterior. |
| Erros | `400` ERR-11 (incluindo ficheiro acima de 2 MB); `403` ERR-07; `413` ERR-12; `415` ERR-13. |
| Requisitos | RF047; RNF016; P01 · UC08 |

```http
PUT /api/recruiter/company/logo HTTP/1.1
Authorization: Bearer <credencial>
Content-Type: multipart/form-data; boundary=----katch

------katch
Content-Disposition: form-data; name="file"; filename="logo.png"
Content-Type: image/png

<conteúdo binário>
------katch--

HTTP/1.1 204 No Content
Location: <URL do ficheiro>
```

#### 6.4.7. `POST /api/recruiter/company/photos` — Acrescentar uma fotografia à galeria

| Campo | Conteúdo |
| --- | --- |
| Operação | `CompanyController.AddGalleryPhoto` → `ICompanyService.AddGalleryPhotoAsync` (`Company.AddGalleryPhoto`) |
| Autorização | `ApprovedCompany` (a galeria faz parte da página de apresentação) |
| Corpo do pedido | `multipart/form-data`, campo `file`: JPEG ou PNG até 5 MB. |
| Resposta de sucesso | `201 Created`, com `Location` = URL da fotografia, acrescentada no fim da galeria. |
| Erros | `400` ERR-11; `403` ERR-07; `409` ERR-39 (a galeria já tem 6 fotografias); `413` ERR-12. |
| Requisitos | RF048; RNF016; P01, P13 · UC08 |

```http
POST /api/recruiter/company/photos HTTP/1.1
Authorization: Bearer <credencial>
Content-Type: multipart/form-data; boundary=----katch

------katch
Content-Disposition: form-data; name="file"; filename="escritorio.jpg"
Content-Type: image/jpeg

<conteúdo binário>
------katch--

HTTP/1.1 201 Created
Location: <URL do ficheiro>
```

### 6.5. `JobsController` — `/api/recruiter/jobs`

Casos de uso UC09 e UC10. Serviço: `IJobService`. Autorização: `ApprovedCompany` em todos os endpoints. Uma vaga de outra Empresa devolve `403` (ERR-08, RF059); uma vaga eliminada devolve `404` (ERR-20).

Transições de estado admitidas (as restantes devolvem `409` ERR-30, com o estado atual):

| Endpoint | Estado de origem | Estado de destino | Operação registada (RF112) |
| --- | --- | --- | --- |
| 6.5.2 Criar | — | `UNPUBLISHED` | `JOB_CREATED` |
| 6.5.3 Alterar | `UNPUBLISHED`, `PUBLISHED`, `SUSPENDED` | sem mudança | `JOB_UPDATED` |
| 6.5.4 Publicar | `UNPUBLISHED` | `PUBLISHED` | `JOB_PUBLISHED` |
| 6.5.5 Suspender | `PUBLISHED` | `SUSPENDED` | `JOB_SUSPENDED` |
| 6.5.6 Voltar a publicar | `SUSPENDED` | `PUBLISHED` | `JOB_REPUBLISHED` |
| 6.5.7 Encerrar | `UNPUBLISHED`, `PUBLISHED`, `SUSPENDED` | `CLOSED` | `JOB_CLOSED` |
| 6.5.8 Eliminar | qualquer, sem interesses | eliminada (`deleted_at`) | `JOB_DELETED` |
| Tarefa periódica | `PUBLISHED` com `expiresAt` atingida | `CLOSED` (`EXPIRED`) | `JOB_CLOSED`, sem autor (RF057) |

Suspender, encerrar ou encerrar automaticamente uma vaga não altera os interesses em espera nem os matches (RF055, RF056, RF113).

#### 6.5.1. `GET /api/recruiter/jobs` — Listar as vagas da Empresa

| Campo | Conteúdo |
| --- | --- |
| Operação | `JobsController.List` → `IJobService.ListOwnAsync` |
| Resposta de sucesso | `200 OK` — `JobDto[]`, só da Empresa do Recrutador, sem as eliminadas, da mais recente para a mais antiga. `waitingCandidates` conta os interesses em espera de cada vaga (RF060). |
| Requisitos | RF059, RF060 · UC09, UC10 |

```http
GET /api/recruiter/jobs HTTP/1.1
Authorization: Bearer <credencial>

HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "jobId": "5e6f7a8b-9c0d-4e1f-a2b3-c4d5e6f7a805",
    "title": "Programador Backend .NET",
    "description": "Desenvolvimento e manutenção de APIs REST em ASP.NET Core para clientes do setor da logística.",
    "minSalary": 1600.00,
    "maxSalary": 2100.00,
    "locationId": "1a2b3c4d-5e6f-4a7b-8c9d-0e1f2a3b4c07",
    "locationName": "Braga (Braga)",
    "contractType": "PERMANENT",
    "workMode": "HYBRID",
    "skills": [
      { "id": "7c6b5a49-3827-4615-a0b9-c8d7e6f5a409", "name": "C#" },
      { "id": "7c6b5a49-3827-4615-a0b9-c8d7e6f5a410", "name": "SQL" }
    ],
    "benefits": [ { "id": "6b5a4938-2716-4504-9f8e-d7c6b5a4f312", "name": "Seguro de saúde" } ],
    "isUrgent": true,
    "expiresAt": "2026-11-30T23:59:00Z",
    "status": "PUBLISHED",
    "publishedAt": "2026-10-09T15:00:00Z",
    "photoUrls": ["<URL do ficheiro>"],
    "waitingCandidates": 3
  }
]
```

#### 6.5.2. `POST /api/recruiter/jobs` — Criar uma vaga

| Campo | Conteúdo |
| --- | --- |
| Operação | `JobsController.Create` → `IJobService.CreateAsync` |
| Corpo do pedido | `JobRequest`: obrigatórios `title`, `description`, `minSalary`, `maxSalary`, `locationId`, `contractType`, `workMode`, `skillIds` (pelo menos 1); opcionais `benefitIds`, `isUrgent` (por omissão `false`), `expiresAt`. A área de gestão web propõe por omissão a localidade da Empresa (RF050); o pedido traz sempre `locationId`. |
| Resposta de sucesso | `201 Created` — `JobDto` com `status = UNPUBLISHED`, sem `Location` (não há consulta individual de vaga para o Recrutador; a vaga aparece em 6.5.1). |
| Erros | `400` ERR-10: campo obrigatório vazio; `minSalary` ou `maxSalary` não positivos ou `minSalary > maxSalary` (RF051); identificadores fora das listas; `expiresAt` no passado; `title` acima de 150 caracteres. |
| Requisitos | RF050, RF051, RF112 · UC09 |

```http
POST /api/recruiter/jobs HTTP/1.1
Authorization: Bearer <credencial>
Content-Type: application/json

{
  "title": "Programador Backend .NET",
  "description": "Desenvolvimento e manutenção de APIs REST em ASP.NET Core para clientes do setor da logística.",
  "minSalary": 2100.00,
  "maxSalary": 1600.00,
  "locationId": "1a2b3c4d-5e6f-4a7b-8c9d-0e1f2a3b4c07",
  "contractType": "PERMANENT",
  "workMode": "HYBRID",
  "skillIds": ["7c6b5a49-3827-4615-a0b9-c8d7e6f5a409", "7c6b5a49-3827-4615-a0b9-c8d7e6f5a410"],
  "benefitIds": ["6b5a4938-2716-4504-9f8e-d7c6b5a4f312"],
  "isUrgent": true,
  "expiresAt": "2026-11-30T23:59:00Z"
}

HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "message": "Os dados enviados não cumprem a validação automática.",
  "fields": [ { "field": "minSalary", "reason": "O valor mínimo não pode ser superior ao valor máximo." } ]
}
```

#### 6.5.3. `PUT /api/recruiter/jobs/{jobId}` — Alterar uma vaga

| Campo | Conteúdo |
| --- | --- |
| Operação | `JobsController.Update` → `IJobService.UpdateAsync` (`Job.Update`) |
| Parâmetros | `jobId` (rota, `Guid`) |
| Corpo do pedido | `JobRequest` completo, com as validações da criação (RF054). |
| Resposta de sucesso | `200 OK` — `JobDto` atualizado; o estado não muda. |
| Erros | `400` ERR-10; `403` ERR-08; `404` ERR-20; `409` ERR-30 (vaga `CLOSED`). |
| Requisitos | RF054, RF051, RF059, RF112 · UC10 |

```http
PUT /api/recruiter/jobs/5e6f7a8b-9c0d-4e1f-a2b3-c4d5e6f7a806 HTTP/1.1
Authorization: Bearer <credencial>
Content-Type: application/json

{
  "title": "Analista de Dados",
  "description": "Análise de dados operacionais e construção de relatórios em SQL.",
  "minSalary": 1300.00,
  "maxSalary": 1800.00,
  "locationId": "1a2b3c4d-5e6f-4a7b-8c9d-0e1f2a3b4c07",
  "contractType": "FIXED_TERM",
  "workMode": "ON_SITE",
  "skillIds": ["7c6b5a49-3827-4615-a0b9-c8d7e6f5a410"],
  "benefitIds": [],
  "isUrgent": false,
  "expiresAt": null
}

HTTP/1.1 200 OK
Content-Type: application/json

{
  "jobId": "5e6f7a8b-9c0d-4e1f-a2b3-c4d5e6f7a806",
  "title": "Analista de Dados",
  "description": "Análise de dados operacionais e construção de relatórios em SQL.",
  "minSalary": 1300.00,
  "maxSalary": 1800.00,
  "locationId": "1a2b3c4d-5e6f-4a7b-8c9d-0e1f2a3b4c07",
  "locationName": "Braga (Braga)",
  "contractType": "FIXED_TERM",
  "workMode": "ON_SITE",
  "skills": [
    {
      "id": "7c6b5a49-3827-4615-a0b9-c8d7e6f5a410",
      "name": "SQL"
    }
  ],
  "benefits": [],
  "isUrgent": false,
  "expiresAt": null,
  "status": "UNPUBLISHED",
  "publishedAt": null,
  "photoUrls": [],
  "waitingCandidates": 0
}
```

#### 6.5.4. `POST /api/recruiter/jobs/{jobId}/publish` — Publicar uma vaga

| Campo | Conteúdo |
| --- | --- |
| Operação | `JobsController.Publish` → `IJobService.PublishAsync` (`Job.Publish`) |
| Parâmetros | `jobId` (rota, `Guid`) |
| Resposta de sucesso | `200 OK` — `JobDto` com `status = PUBLISHED` e `publishedAt`; na primeira publicação, o backend preenche também `first_published_at` (RF096). |
| Erros | `403` ERR-08; `404` ERR-20; `409` ERR-30 (vaga fora de `UNPUBLISHED`; com `expiresAt` já atingida, a publicação também é rejeitada). |
| Requisitos | RF053, RF059, RF112 · UC09 |

```http
POST /api/recruiter/jobs/5e6f7a8b-9c0d-4e1f-a2b3-c4d5e6f7a806/publish HTTP/1.1
Authorization: Bearer <credencial>

HTTP/1.1 200 OK
Content-Type: application/json

{
  "jobId": "5e6f7a8b-9c0d-4e1f-a2b3-c4d5e6f7a806",
  "title": "Analista de Dados",
  "description": "Análise de dados operacionais e construção de relatórios em SQL.",
  "minSalary": 1300.00,
  "maxSalary": 1800.00,
  "locationId": "1a2b3c4d-5e6f-4a7b-8c9d-0e1f2a3b4c07",
  "locationName": "Braga (Braga)",
  "contractType": "FIXED_TERM",
  "workMode": "ON_SITE",
  "skills": [
    {
      "id": "7c6b5a49-3827-4615-a0b9-c8d7e6f5a410",
      "name": "SQL"
    }
  ],
  "benefits": [],
  "isUrgent": false,
  "expiresAt": null,
  "status": "PUBLISHED",
  "publishedAt": "2026-10-09T16:30:00Z",
  "photoUrls": [],
  "waitingCandidates": 0
}
```

#### 6.5.5. `POST /api/recruiter/jobs/{jobId}/suspend` — Suspender uma vaga

| Campo | Conteúdo |
| --- | --- |
| Operação | `JobsController.Suspend` → `IJobService.SuspendAsync` (`Job.Suspend`) |
| Parâmetros | `jobId` (rota, `Guid`) |
| Resposta de sucesso | `200 OK` — `JobDto` com `status = SUSPENDED`. A vaga deixa de ser apresentada aos Candidatos; os interesses mantêm-se. |
| Erros | `403` ERR-08; `404` ERR-20; `409` ERR-30 (vaga fora de `PUBLISHED`). |
| Requisitos | RF055, RF059, RF112, RF113 · UC10 |

```http
POST /api/recruiter/jobs/5e6f7a8b-9c0d-4e1f-a2b3-c4d5e6f7a806/suspend HTTP/1.1
Authorization: Bearer <credencial>

HTTP/1.1 200 OK
Content-Type: application/json

{
  "jobId": "5e6f7a8b-9c0d-4e1f-a2b3-c4d5e6f7a806",
  "title": "Analista de Dados",
  "description": "Análise de dados operacionais e construção de relatórios em SQL.",
  "minSalary": 1300.00,
  "maxSalary": 1800.00,
  "locationId": "1a2b3c4d-5e6f-4a7b-8c9d-0e1f2a3b4c07",
  "locationName": "Braga (Braga)",
  "contractType": "FIXED_TERM",
  "workMode": "ON_SITE",
  "skills": [
    {
      "id": "7c6b5a49-3827-4615-a0b9-c8d7e6f5a410",
      "name": "SQL"
    }
  ],
  "benefits": [],
  "isUrgent": false,
  "expiresAt": null,
  "status": "SUSPENDED",
  "publishedAt": "2026-10-09T16:30:00Z",
  "photoUrls": [],
  "waitingCandidates": 0
}
```

#### 6.5.6. `POST /api/recruiter/jobs/{jobId}/republish` — Voltar a publicar uma vaga suspensa

| Campo | Conteúdo |
| --- | --- |
| Operação | `JobsController.Republish` → `IJobService.RepublishAsync` (`Job.Republish`) |
| Parâmetros | `jobId` (rota, `Guid`) |
| Resposta de sucesso | `200 OK` — `JobDto` com `status = PUBLISHED` e `publishedAt` atualizado; `first_published_at` não muda. |
| Erros | `403` ERR-08; `404` ERR-20; `409` ERR-30 (vaga fora de `SUSPENDED`). |
| Requisitos | RF115, RF059, RF112 · UC10 |

```http
POST /api/recruiter/jobs/5e6f7a8b-9c0d-4e1f-a2b3-c4d5e6f7a806/republish HTTP/1.1
Authorization: Bearer <credencial>

HTTP/1.1 200 OK
Content-Type: application/json

{
  "jobId": "5e6f7a8b-9c0d-4e1f-a2b3-c4d5e6f7a806",
  "title": "Analista de Dados",
  "description": "Análise de dados operacionais e construção de relatórios em SQL.",
  "minSalary": 1300.00,
  "maxSalary": 1800.00,
  "locationId": "1a2b3c4d-5e6f-4a7b-8c9d-0e1f2a3b4c07",
  "locationName": "Braga (Braga)",
  "contractType": "FIXED_TERM",
  "workMode": "ON_SITE",
  "skills": [
    {
      "id": "7c6b5a49-3827-4615-a0b9-c8d7e6f5a410",
      "name": "SQL"
    }
  ],
  "benefits": [],
  "isUrgent": false,
  "expiresAt": null,
  "status": "PUBLISHED",
  "publishedAt": "2026-10-09T17:10:00Z",
  "photoUrls": [],
  "waitingCandidates": 0
}
```

#### 6.5.7. `POST /api/recruiter/jobs/{jobId}/close` — Encerrar uma vaga

| Campo | Conteúdo |
| --- | --- |
| Operação | `JobsController.Close` → `IJobService.CloseAsync` (`Job.Close`) |
| Parâmetros | `jobId` (rota, `Guid`) |
| Resposta de sucesso | `200 OK` — `JobDto` com `status = CLOSED` (`close_reason = MANUAL`). O encerramento é definitivo; interesses e matches mantêm-se. |
| Erros | `403` ERR-08; `404` ERR-20; `409` ERR-30 (vaga já `CLOSED`). |
| Requisitos | RF056, RF059, RF112, RF113 · UC10 |

```http
POST /api/recruiter/jobs/5e6f7a8b-9c0d-4e1f-a2b3-c4d5e6f7a806/close HTTP/1.1
Authorization: Bearer <credencial>

HTTP/1.1 200 OK
Content-Type: application/json

{
  "jobId": "5e6f7a8b-9c0d-4e1f-a2b3-c4d5e6f7a806",
  "title": "Analista de Dados",
  "description": "Análise de dados operacionais e construção de relatórios em SQL.",
  "minSalary": 1300.00,
  "maxSalary": 1800.00,
  "locationId": "1a2b3c4d-5e6f-4a7b-8c9d-0e1f2a3b4c07",
  "locationName": "Braga (Braga)",
  "contractType": "FIXED_TERM",
  "workMode": "ON_SITE",
  "skills": [
    {
      "id": "7c6b5a49-3827-4615-a0b9-c8d7e6f5a410",
      "name": "SQL"
    }
  ],
  "benefits": [],
  "isUrgent": false,
  "expiresAt": null,
  "status": "CLOSED",
  "publishedAt": "2026-10-09T17:10:00Z",
  "photoUrls": [],
  "waitingCandidates": 0
}
```

#### 6.5.8. `DELETE /api/recruiter/jobs/{jobId}` — Eliminar uma vaga sem interesses

| Campo | Conteúdo |
| --- | --- |
| Operação | `JobsController.Delete` → `IJobService.DeleteAsync` (`Job.MarkDeleted`) |
| Parâmetros | `jobId` (rota, `Guid`) |
| Resposta de sucesso | `204 No Content`. Eliminação lógica (`deleted_at`); a vaga deixa de aparecer em todas as consultas. As recusas de Candidatos (sem interesse) não impedem a eliminação. |
| Erros | `403` ERR-08; `404` ERR-20; `409` ERR-38 (pelo menos um interesse registado, em qualquer estado da decisão). |
| Requisitos | RF058, RF114, RF059, RF112 · UC10 |

```http
DELETE /api/recruiter/jobs/5e6f7a8b-9c0d-4e1f-a2b3-c4d5e6f7a805 HTTP/1.1
Authorization: Bearer <credencial>

HTTP/1.1 409 Conflict
Content-Type: application/json

{ "message": "A vaga tem interesses registados e só pode ser encerrada.", "fields": [] }
```

#### 6.5.9. `POST /api/recruiter/jobs/{jobId}/photos` — Acrescentar uma fotografia à vaga

| Campo | Conteúdo |
| --- | --- |
| Operação | `JobsController.AddPhoto` → `IJobService.AddPhotoAsync` (`Job.AddPhoto`) |
| Parâmetros | `jobId` (rota, `Guid`) |
| Corpo do pedido | `multipart/form-data`, campo `file`: JPEG ou PNG até 5 MB. |
| Resposta de sucesso | `201 Created`, com `Location` = URL da fotografia. |
| Erros | `400` ERR-11; `403` ERR-08; `404` ERR-20; `409` ERR-39 (a vaga já tem 5 fotografias); `409` ERR-30 (vaga `CLOSED`); `413` ERR-12. |
| Requisitos | RF052, RF059; RNF016; P01, P13 · UC09 |

```http
POST /api/recruiter/jobs/5e6f7a8b-9c0d-4e1f-a2b3-c4d5e6f7a805/photos HTTP/1.1
Authorization: Bearer <credencial>
Content-Type: multipart/form-data; boundary=----katch

------katch
Content-Disposition: form-data; name="file"; filename="equipa.jpg"
Content-Type: image/jpeg

<conteúdo binário>
------katch--

HTTP/1.1 201 Created
Location: <URL do ficheiro>
```

### 6.6. `CandidateEvaluationController` — `/api/recruiter`

Casos de uso UC11 e UC12. Serviço: `ICandidateEvaluationService`. Autorização: `ApprovedCompany` em todos os endpoints. O `matchId` identifica a resposta do Candidato à vaga (linha de `match`); nesta secção, designa um interesse. O serviço confirma sempre que a vaga do interesse pertence à Empresa do Recrutador (ERR-08 em caso contrário).

#### 6.6.1. `GET /api/recruiter/jobs/{jobId}/waiting-candidates` — Consultar os candidatos em espera

| Campo | Conteúdo |
| --- | --- |
| Operação | `CandidateEvaluationController.ListWaiting` → `ICandidateEvaluationService.ListWaitingAsync` |
| Parâmetros | `jobId` (rota, `Guid`): vaga da Empresa, em qualquer estado não eliminado (os interesses de vagas suspensas ou encerradas continuam em espera, RF113). |
| Resposta de sucesso | `200 OK` — `WaitingCandidateDto[]`, só com interesses `WAITING`, do mais antigo para o mais recente. A lista não tem ações de decisão (RF060). |
| Erros | `403` ERR-08; `404` ERR-20. |
| Requisitos | RF060, RF113 · UC11, UC12 |

```http
GET /api/recruiter/jobs/5e6f7a8b-9c0d-4e1f-a2b3-c4d5e6f7a805/waiting-candidates HTTP/1.1
Authorization: Bearer <credencial>

HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "matchId": "9d8c7b6a-5f4e-4d3c-b2a1-0f9e8d7c6b06",
    "candidateId": "3f2a9c1e-7b4d-4e8a-9c21-5d6e7f8a9b01",
    "fullName": "Ana Ferreira",
    "interestAt": "2026-10-09T15:12:40Z"
  }
]
```

#### 6.6.2. `GET /api/recruiter/interests/{matchId}/profile` — Abrir o perfil completo do Candidato

| Campo | Conteúdo |
| --- | --- |
| Operação | `CandidateEvaluationController.OpenProfile` → `ICandidateEvaluationService.OpenProfileAsync` (`Match.OpenProfile`) |
| Parâmetros | `matchId` (rota, `Guid`) |
| Resposta de sucesso | `200 OK` — `CandidateFullProfileDto`: fotografia, dados profissionais, hiperligações, competências com `matchesJob` (RF062) e `hasCv`. Não inclui contactos, pretensão salarial nem preferências (modelo de dados, secção 8). |
| Efeito | Na primeira chamada para este interesse, regista `profile_opened_at` e `profile_opened_by` (RF118), condição da decisão (RF063, RF064). As chamadas seguintes não alteram o registo: o pedido é idempotente, embora seja um `GET` com efeito (DAPI-05). |
| Erros | `403` ERR-08; `404` ERR-20; `409` ERR-35 (o interesse já não está em espera). |
| Requisitos | RF061, RF062, RF118 · UC11 |

```http
GET /api/recruiter/interests/9d8c7b6a-5f4e-4d3c-b2a1-0f9e8d7c6b06/profile HTTP/1.1
Authorization: Bearer <credencial>

HTTP/1.1 200 OK
Content-Type: application/json

{
  "matchId": "9d8c7b6a-5f4e-4d3c-b2a1-0f9e8d7c6b06",
  "fullName": "Ana Ferreira",
  "photoUrl": "<URL do ficheiro>",
  "desiredRole": "Programadora backend",
  "locationName": "Guimarães (Braga)",
  "availability": "ONE_MONTH",
  "experienceSummary": "Dois anos de desenvolvimento de APIs REST em C# e PostgreSQL.",
  "links": ["https://github.com/ana-ferreira-exemplo"],
  "skills": [
    { "name": "C#", "matchesJob": true },
    { "name": "Entity Framework", "matchesJob": false }
  ],
  "hasCv": true
}
```

#### 6.6.3. `GET /api/recruiter/interests/{matchId}/cv` — Obter o curriculum vitae

| Campo | Conteúdo |
| --- | --- |
| Operação | `CandidateEvaluationController.GetCv` → `ICandidateEvaluationService.GetCvAsync` |
| Parâmetros | `matchId` (rota, `Guid`) |
| Resposta de sucesso | `200 OK`, `Content-Type: application/pdf`, `Content-Disposition: attachment; filename="cv.pdf"`, `Cache-Control: no-store`. O nome do ficheiro não revela o caminho no servidor. |
| Erros | `403` ERR-08: vaga de outra Empresa; `404` ERR-20: interesse inexistente ou Candidato sem CV; `409` ERR-35: interesse fora do estado `WAITING`, com o mesmo código da abertura do perfil (6.6.2). Em ambos os casos o pedido é rejeitado, como exige o RNF007, que só admite o Recrutador de uma vaga em que o Candidato está em espera. |
| Requisitos | RF061; RNF007 · UC11 |

```http
GET /api/recruiter/interests/9d8c7b6a-5f4e-4d3c-b2a1-0f9e8d7c6b06/cv HTTP/1.1
Authorization: Bearer <credencial>

HTTP/1.1 200 OK
Content-Type: application/pdf
Content-Disposition: attachment; filename="cv.pdf"
Cache-Control: no-store

<conteúdo binário do PDF>
```

#### 6.6.4. `POST /api/recruiter/interests/{matchId}/accept` — Aceitar o Candidato

| Campo | Conteúdo |
| --- | --- |
| Operação | `CandidateEvaluationController.Accept` → `ICandidateEvaluationService.AcceptAsync` (`Match.Accept`) |
| Parâmetros | `matchId` (rota, `Guid`) |
| Resposta de sucesso | `200 OK` — `MatchDto`, com o contacto do Candidato (`contactEmail`, `contactPhone`) e `matchedAt`. O `matchId` passa a identificar também a conversa (`/api/conversations/{matchId}`). |
| Efeitos | Numa única transação (UC12, passo 6): decisão `ACCEPTED` com Recrutador e data (RF065); `MATCHED` com `matchedAt` (RF066); conversa `OPEN` (RF068); duas notificações `MATCH_CONFIRMED`, uma para cada parte (RF031, RF077), entregues pela ligação persistente depois de confirmada a transação. |
| Erros | `403` ERR-08; `404` ERR-20; `409` ERR-34 (perfil não aberto); `409` ERR-35 (decisão já registada, incluindo a perda na concorrência `xmin` com outro pedido de decisão). Em qualquer erro, nada é alterado (UC12, E5). |
| Requisitos | RF063, RF065, RF066, RF068, RF031, RF077, RF117 · UC12 |

```http
POST /api/recruiter/interests/9d8c7b6a-5f4e-4d3c-b2a1-0f9e8d7c6b06/accept HTTP/1.1
Authorization: Bearer <credencial>

HTTP/1.1 200 OK
Content-Type: application/json

{
  "matchId": "9d8c7b6a-5f4e-4d3c-b2a1-0f9e8d7c6b06",
  "jobTitle": "Programador Backend .NET",
  "companyName": "Lumen Software, Lda.",
  "counterpartName": "Ana Ferreira",
  "contactEmail": "ana.ferreira@exemplo.pt",
  "contactPhone": "912345678",
  "matchedAt": "2026-10-09T16:02:11Z"
}
```

#### 6.6.5. `POST /api/recruiter/interests/{matchId}/reject` — Recusar o Candidato

| Campo | Conteúdo |
| --- | --- |
| Operação | `CandidateEvaluationController.Reject` → `ICandidateEvaluationService.RejectAsync` (`Match.Reject`) |
| Parâmetros | `matchId` (rota, `Guid`) |
| Resposta de sucesso | `204 No Content`. Decisão `DECLINED` com Recrutador e data (RF065); o Candidato sai da lista de espera dessa vaga, sem efeito nas outras vagas; não há conversa, contacto nem notificação ao Candidato (UC12, A1). |
| Erros | `403` ERR-08; `404` ERR-20; `409` ERR-34; `409` ERR-35. |
| Requisitos | RF064, RF065, RF117 · UC12 |

```http
POST /api/recruiter/interests/9d8c7b6a-5f4e-4d3c-b2a1-0f9e8d7c6b07/reject HTTP/1.1
Authorization: Bearer <credencial>

HTTP/1.1 409 Conflict
Content-Type: application/json

{ "message": "Abra o perfil completo do Candidato antes de registar a decisão.", "fields": [] }
```

### 6.7. `MatchesController` — `/api/matches`

Caso de uso UC13. Serviço: `IMatchService`.

#### 6.7.1. `GET /api/matches` — Consultar os matches

| Campo | Conteúdo |
| --- | --- |
| Operação | `MatchesController.List` → `IMatchService.ListForCandidateAsync` (Candidato) ou `ListForRecruiterAsync` (Recrutador), conforme o `role` da credencial |
| Autorização | `CandidateOrRecruiter`. O Recrutador tem também de cumprir a política `ApprovedCompany`: os matches dão acesso aos contactos dos Candidatos, que o RF039 veda sem Empresa aprovada, e o RF067 refere os matches da Empresa (modelo de classes, secção 6: «a política `ApprovedCompany` confirma também o estado da Empresa em cada operação reservada»). O Candidato continua a ver os matches de Empresas suspensas (RF024). |
| Resposta de sucesso | `200 OK` — `MatchDto[]`, do mais recente para o mais antigo, incluindo matches de vagas encerradas e de Empresas suspensas (RF024, RF067). |
| Conteúdo por tipo de conta | Candidato: `counterpartName` = nome do responsável da Empresa; `contactEmail` e `contactPhone` = contactos de registo da Empresa (RF025). Recrutador: matches das vagas da sua Empresa; `counterpartName` = nome do Candidato; contactos do Candidato (RF067). |
| Erros | `403` ERR-07: Recrutador cuja Empresa não está aprovada. |
| Requisitos | RF024, RF025, RF067, RF039 · UC13 |

```http
GET /api/matches HTTP/1.1
Authorization: Bearer <credencial do Candidato>

HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "matchId": "9d8c7b6a-5f4e-4d3c-b2a1-0f9e8d7c6b06",
    "jobTitle": "Programador Backend .NET",
    "companyName": "Lumen Software, Lda.",
    "counterpartName": "Rui Matos",
    "contactEmail": "recrutamento@lumen-exemplo.pt",
    "contactPhone": "253000222",
    "matchedAt": "2026-10-09T16:02:11Z"
  }
]
```

### 6.8. `ConversationsController` — `/api/conversations`

Caso de uso UC14. Serviço: `IConversationService`. Autorização: `CandidateOrRecruiter` em todos os endpoints; o Administrador recebe `403` (ERR-06), sem acesso ao conteúdo das conversas (RF103). A conversa é identificada pelo `matchId`. Só são partes da conversa o Candidato do match e o Recrutador da Empresa da vaga; qualquer outro utilizador recebe `403` (ERR-08). As mensagens novas, os encerramentos e as notificações chegam pela ligação persistente (secção 7); estes endpoints servem o histórico, o envio e a recuperação depois de uma reconexão.

#### 6.8.1. `GET /api/conversations` — Consultar a lista de conversas

| Campo | Conteúdo |
| --- | --- |
| Operação | `ConversationsController.List` → `IConversationService.ListAsync` |
| Resposta de sucesso | `200 OK` — `ConversationSummaryDto[]`: todas as conversas do utilizador (abertas e em modo apenas de consulta), com `unreadCount` (mensagens da outra parte por ler, RF027, RF071), ordenadas por `lastMessageAt` decrescente; as conversas sem mensagens aparecem primeiro, pela data do match. |
| Requisitos | RF027, RF071 · UC14 |

```http
GET /api/conversations HTTP/1.1
Authorization: Bearer <credencial do Recrutador>

HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "matchId": "9d8c7b6a-5f4e-4d3c-b2a1-0f9e8d7c6b06",
    "counterpartName": "Ana Ferreira",
    "jobTitle": "Programador Backend .NET",
    "companyName": "Lumen Software, Lda.",
    "status": "OPEN",
    "closeReason": null,
    "lastMessageAt": "2026-10-09T16:20:05Z",
    "unreadCount": 1
  }
]
```

#### 6.8.2. `GET /api/conversations/{matchId}/messages` — Abrir uma conversa

| Campo | Conteúdo |
| --- | --- |
| Operação | `ConversationsController.Open` → `IConversationService.OpenAsync` |
| Parâmetros | `matchId` (rota, `Guid`) |
| Resposta de sucesso | `200 OK` — `MessageDto[]`, da mais antiga para a mais recente, com texto, data de envio e `readAt` (indicação de leitura pelo destinatário, RF028, RF072). |
| Efeito | Marca como lidas todas as mensagens da outra parte ainda por ler (RF108, RF109); repetir o pedido não muda nada. Funciona também com a conversa em modo apenas de consulta. |
| Erros | `403` ERR-08; `404` ERR-20; `409` ERR-37 (o `matchId` não corresponde a um match confirmado). |
| Requisitos | RF028, RF072, RF108, RF109 · UC14 |

```http
GET /api/conversations/9d8c7b6a-5f4e-4d3c-b2a1-0f9e8d7c6b06/messages HTTP/1.1
Authorization: Bearer <credencial do Candidato>

HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "messageId": "2e3f4a5b-6c7d-4e8f-9a0b-1c2d3e4f5a12",
    "senderId": "8b1d2e3f-4a5b-4c6d-8e7f-9a0b1c2d3e02",
    "content": "Olá, Ana. Tem disponibilidade para uma entrevista na próxima terça-feira?",
    "sentAt": "2026-10-09T16:10:00Z",
    "readAt": "2026-10-09T16:18:30Z"
  },
  {
    "messageId": "2e3f4a5b-6c7d-4e8f-9a0b-1c2d3e4f5a13",
    "senderId": "3f2a9c1e-7b4d-4e8a-9c21-5d6e7f8a9b01",
    "content": "Bom dia. Sim, de manhã é ideal para mim.",
    "sentAt": "2026-10-09T16:20:05Z",
    "readAt": null
  }
]
```

#### 6.8.3. `POST /api/conversations/{matchId}/messages` — Enviar uma mensagem

| Campo | Conteúdo |
| --- | --- |
| Operação | `ConversationsController.Send` → `IConversationService.SendAsync` (`Match.CanSendMessages`, `Match.RegisterMessage`) |
| Parâmetros | `matchId` (rota, `Guid`) |
| Corpo do pedido | `SendMessageRequest`: `content`, com 1 a 1000 caracteres, não só espaços. |
| Resposta de sucesso | `201 Created` — `MessageDto` gravado (`readAt = null`). |
| Efeitos | Atualiza `last_message_at`; cria a notificação `NEW_MESSAGE` para o destinatário (RF032, RF078); depois de confirmada a transação, entrega `MessageReceived` às duas partes e `NotificationReceived` ao destinatário (secção 7), em 5 segundos (P17). |
| Erros | `400` ERR-10 (`content` vazio ou acima de 1000 caracteres); `403` ERR-08; `404` ERR-20; `409` ERR-36 (conversa em modo apenas de consulta); `409` ERR-37 (sem match, RF070). |
| Requisitos | RF026, RF069, RF070, RF032, RF078, RF029, RF073; P03, P17 · UC14 |

```http
POST /api/conversations/9d8c7b6a-5f4e-4d3c-b2a1-0f9e8d7c6b06/messages HTTP/1.1
Authorization: Bearer <credencial do Candidato>
Content-Type: application/json

{ "content": "Bom dia. Sim, de manhã é ideal para mim." }

HTTP/1.1 201 Created
Content-Type: application/json

{
  "messageId": "2e3f4a5b-6c7d-4e8f-9a0b-1c2d3e4f5a13",
  "senderId": "3f2a9c1e-7b4d-4e8a-9c21-5d6e7f8a9b01",
  "content": "Bom dia. Sim, de manhã é ideal para mim.",
  "sentAt": "2026-10-09T16:20:05Z",
  "readAt": null
}
```

#### 6.8.4. `POST /api/conversations/{matchId}/close` — Encerrar uma conversa

| Campo | Conteúdo |
| --- | --- |
| Operação | `ConversationsController.Close` → `IConversationService.CloseAsync` (`Match.CloseConversation`) |
| Parâmetros | `matchId` (rota, `Guid`) |
| Resposta de sucesso | `204 No Content`. A conversa passa a `CLOSED` com `CLOSED_BY_PARTY`, data e autor, para as duas partes; o histórico continua visível. `ConversationClosed` é entregue às duas partes (secção 7.2). |
| Erros | `403` ERR-08; `404` ERR-20; `409` ERR-36 (já encerrada, incluindo a perda na concorrência `xmin` com um bloqueio ou suspensão simultâneos); `409` ERR-37. |
| Requisitos | RF030, RF074, RF075 · UC14 |

```http
POST /api/conversations/9d8c7b6a-5f4e-4d3c-b2a1-0f9e8d7c6b06/close HTTP/1.1
Authorization: Bearer <credencial>

HTTP/1.1 204 No Content
```

### 6.9. `NotificationsController` — `/api/notifications`

Caso de uso UC15. Serviço: `INotificationService`. Autorização: `CandidateOrRecruiter` (o Administrador não recebe notificações; DA, F011).

#### 6.9.1. `GET /api/notifications` — Consultar a área de notificações

| Campo | Conteúdo |
| --- | --- |
| Operação | `NotificationsController.List` → `INotificationService.ListAsync` |
| Resposta de sucesso | `200 OK` — `NotificationListDto`: `items`, da mais recente para a mais antiga, e `unreadCount` (RF034, RF080). |
| Acesso ao elemento | O cliente usa `type` e `targetId` para abrir o elemento a que a notificação se refere (RF036, RF082), segundo a tabela da secção 7.3. |
| Requisitos | RF034, RF036, RF080, RF082 · UC15 |

```http
GET /api/notifications HTTP/1.1
Authorization: Bearer <credencial do Recrutador>

HTTP/1.1 200 OK
Content-Type: application/json

{
  "items": [
    {
      "notificationId": "4f5a6b7c-8d9e-4f0a-b1c2-d3e4f5a6b713",
      "type": "NEW_MESSAGE",
      "text": "Nova mensagem de Ana Ferreira na conversa sobre a vaga Programador Backend .NET.",
      "targetId": "9d8c7b6a-5f4e-4d3c-b2a1-0f9e8d7c6b06",
      "createdAt": "2026-10-09T16:20:05Z",
      "isRead": false
    },
    {
      "notificationId": "4f5a6b7c-8d9e-4f0a-b1c2-d3e4f5a6b714",
      "type": "NEW_INTEREST",
      "text": "Novo interesse na vaga Programador Backend .NET.",
      "targetId": "5e6f7a8b-9c0d-4e1f-a2b3-c4d5e6f7a805",
      "createdAt": "2026-10-09T15:12:40Z",
      "isRead": true
    }
  ],
  "unreadCount": 1
}
```

#### 6.9.2. `POST /api/notifications/{notificationId}/read` — Marcar uma notificação como lida

| Campo | Conteúdo |
| --- | --- |
| Operação | `NotificationsController.MarkRead` → `INotificationService.MarkReadAsync` (`Notification.MarkRead`) |
| Parâmetros | `notificationId` (rota, `Guid`) |
| Resposta de sucesso | `204 No Content`. O número de não lidas desce uma unidade (RF035, RF081). Marcar uma notificação já lida não muda nada e devolve também `204`. |
| Erros | `403` ERR-08 (notificação de outro utilizador); `404` ERR-20. |
| Requisitos | RF035, RF081 · UC15 |

```http
POST /api/notifications/4f5a6b7c-8d9e-4f0a-b1c2-d3e4f5a6b713/read HTTP/1.1
Authorization: Bearer <credencial>

HTTP/1.1 204 No Content
```

### 6.10. `AdminAccountsController` — `/api/admin/accounts`

Caso de uso UC18. Serviço: `IAccountAdminService`. Autorização: `Admin` em todos os endpoints (RF104).

#### 6.10.1. `GET /api/admin/accounts` — Consultar as contas de utilizador

| Campo | Conteúdo |
| --- | --- |
| Operação | `AdminAccountsController.ListAccounts` → `IAccountAdminService.ListAccountsAsync` |
| Resposta de sucesso | `200 OK` — `AccountDto[]`: correio eletrónico, tipo e estado de todas as contas, por ordem alfabética do correio. |
| Requisitos | RF085 · UC18 |

```http
GET /api/admin/accounts HTTP/1.1
Authorization: Bearer <credencial do Administrador>

HTTP/1.1 200 OK
Content-Type: application/json

[
  { "userId": "3f2a9c1e-7b4d-4e8a-9c21-5d6e7f8a9b01", "email": "ana.ferreira@exemplo.pt", "userType": "CANDIDATE", "status": "ACTIVE" },
  { "userId": "8b1d2e3f-4a5b-4c6d-8e7f-9a0b1c2d3e02", "email": "recrutamento@lumen-exemplo.pt", "userType": "RECRUITER", "status": "ACTIVE" }
]
```

#### 6.10.2. `GET /api/admin/accounts/candidates` — Consultar a lista de Candidatos

| Campo | Conteúdo |
| --- | --- |
| Operação | `AdminAccountsController.ListCandidates` → `IAccountAdminService.ListCandidatesAsync` |
| Resposta de sucesso | `200 OK` — `CandidateAdminDto[]`: nome, correio, localidade, data de registo e estado, por ordem alfabética do nome. |
| Requisitos | RF088 · UC18 |

```http
GET /api/admin/accounts/candidates HTTP/1.1
Authorization: Bearer <credencial do Administrador>

HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "candidateId": "3f2a9c1e-7b4d-4e8a-9c21-5d6e7f8a9b01",
    "fullName": "Ana Ferreira",
    "email": "ana.ferreira@exemplo.pt",
    "locationName": "Guimarães (Braga)",
    "registeredAt": "2026-10-09T14:30:00Z",
    "status": "ACTIVE"
  }
]
```

#### 6.10.3. `POST /api/admin/accounts/{userId}/block` — Bloquear uma conta

| Campo | Conteúdo |
| --- | --- |
| Operação | `AdminAccountsController.Block` → `IAccountAdminService.BlockAsync` (`AppUser.Block`) |
| Parâmetros | `userId` (rota, `Guid`): conta de Candidato ou de Recrutador. |
| Resposta de sucesso | `204 No Content`. A conta passa a `BLOCKED`; numa transação, as conversas abertas da conta passam a `CLOSED` com `ACCOUNT_BLOCKED_OR_SUSPENDED` (RF075) e fica registado `ACCOUNT_BLOCKED` (RF112). Depois de confirmada, `ConversationClosed` é entregue às partes. Os interesses em espera mantêm-se (RF113). |
| Erros | `404` ERR-20; `409` ERR-30 (conta de Administrador, ou conta fora de `ACTIVE`). |
| Requisitos | RF086, RF075, RF112 · UC18 |

```http
POST /api/admin/accounts/d7e8f9a0-1b2c-4d3e-8f4a-5b6c7d8e9f20/block HTTP/1.1
Authorization: Bearer <credencial do Administrador>

HTTP/1.1 204 No Content
```

#### 6.10.4. `POST /api/admin/accounts/candidates/{candidateId}/suspend` — Suspender a conta de um Candidato

| Campo | Conteúdo |
| --- | --- |
| Operação | `AdminAccountsController.SuspendCandidate` → `IAccountAdminService.SuspendCandidateAsync` (`AppUser.Suspend`) |
| Parâmetros | `candidateId` (rota, `Guid`) |
| Resposta de sucesso | `204 No Content`. A conta passa a `SUSPENDED`, com os mesmos efeitos do bloqueio sobre as conversas (RF075); regista `ACCOUNT_SUSPENDED` (RF112). |
| Erros | `404` ERR-20 (inexistente ou não é Candidato); `409` ERR-30 (conta fora de `ACTIVE`). |
| Requisitos | RF089, RF075, RF112 · UC18 |

```http
POST /api/admin/accounts/candidates/f0e1d2c3-b4a5-4968-8776-a5b4c3d2e115/suspend HTTP/1.1
Authorization: Bearer <credencial do Administrador>

HTTP/1.1 204 No Content
```

#### 6.10.5. `POST /api/admin/accounts/{userId}/reactivate` — Reativar uma conta

| Campo | Conteúdo |
| --- | --- |
| Operação | `AdminAccountsController.Reactivate` → `IAccountAdminService.ReactivateAsync` (`AppUser.Reactivate`) |
| Parâmetros | `userId` (rota, `Guid`) |
| Resposta de sucesso | `204 No Content`. A conta `BLOCKED` ou `SUSPENDED` passa a `ACTIVE`; regista `ACCOUNT_REACTIVATED` (RF112). As conversas mantêm-se em modo apenas de consulta (RF075). |
| Erros | `404` ERR-20; `409` ERR-30 (conta já `ACTIVE`). |
| Requisitos | RF087, RF090, RF075, RF112 · UC18 |

```http
POST /api/admin/accounts/f0e1d2c3-b4a5-4968-8776-a5b4c3d2e115/reactivate HTTP/1.1
Authorization: Bearer <credencial do Administrador>

HTTP/1.1 204 No Content
```

### 6.11. `AdminCompaniesController` — `/api/admin/companies`

Casos de uso UC16 e UC17. Serviço: `ICompanyAdminService`. Autorização: `Admin` em todos os endpoints (RF104).

Transições do estado da Empresa (as restantes devolvem `409` ERR-30):

| Endpoint | Estado de origem | Estado de destino | Notificação ao Recrutador | Operação registada |
| --- | --- | --- | --- | --- |
| 6.11.3 Aprovar | `PENDING` | `APPROVED` | `COMPANY_APPROVED` (RF079) | `COMPANY_APPROVED` |
| 6.11.4 Recusar | `PENDING` | `REJECTED`, com motivo | `COMPANY_REJECTED`, com o motivo (RF079) | `COMPANY_REJECTED` |
| 6.11.5 Suspender | `APPROVED` | `SUSPENDED`; conversas a `CLOSED` com `COMPANY_SUSPENDED` (RF075) | — | `COMPANY_SUSPENDED` |
| 6.11.6 Reativar | `SUSPENDED` | `APPROVED`; conversas mantêm-se em consulta | — | `COMPANY_REACTIVATED` |

#### 6.11.1. `GET /api/admin/companies/pending` — Consultar as Empresas pendentes

| Campo | Conteúdo |
| --- | --- |
| Operação | `AdminCompaniesController.ListPending` → `ICompanyAdminService.ListPendingAsync` |
| Resposta de sucesso | `200 OK` — `PendingCompanyDto[]`: designação, NIF e data de submissão, da submissão mais antiga para a mais recente. |
| Requisitos | RF091 · UC16 |

```http
GET /api/admin/companies/pending HTTP/1.1
Authorization: Bearer <credencial do Administrador>

HTTP/1.1 200 OK
Content-Type: application/json

[
  { "companyId": "c4e5f6a7-b8c9-4d0e-a1f2-b3c4d5e6f703", "companyName": "Lumen Software, Lda.", "taxId": "509123456", "submittedAt": "2026-10-05T10:00:00Z" }
]
```

#### 6.11.2. `GET /api/admin/companies/{companyId}/registration` — Consultar os dados submetidos

| Campo | Conteúdo |
| --- | --- |
| Operação | `AdminCompaniesController.GetSubmittedData` → `ICompanyAdminService.GetSubmittedDataAsync` |
| Parâmetros | `companyId` (rota, `Guid`) |
| Resposta de sucesso | `200 OK` — `CompanyRegistrationDto`: os oito dados submetidos e a data de submissão. |
| Erros | `404` ERR-20. |
| Requisitos | RF092 · UC16 |

```http
GET /api/admin/companies/c4e5f6a7-b8c9-4d0e-a1f2-b3c4d5e6f703/registration HTTP/1.1
Authorization: Bearer <credencial do Administrador>

HTTP/1.1 200 OK
Content-Type: application/json

{
  "companyId": "c4e5f6a7-b8c9-4d0e-a1f2-b3c4d5e6f703",
  "companyName": "Lumen Software, Lda.",
  "taxId": "509123456",
  "industry": "TECHNOLOGY",
  "address": "Avenida da Liberdade, 100, 4710-000 Braga",
  "locationName": "Braga (Braga)",
  "contactEmail": "geral@lumen-exemplo.pt",
  "contactPhone": "253000111",
  "responsibleName": "Rui Matos",
  "submittedAt": "2026-10-05T10:00:00Z"
}
```

#### 6.11.3. `POST /api/admin/companies/{companyId}/approve` — Aprovar o registo

| Campo | Conteúdo |
| --- | --- |
| Operação | `AdminCompaniesController.Approve` → `ICompanyAdminService.ApproveAsync` (`Company.Approve`) |
| Parâmetros | `companyId` (rota, `Guid`) |
| Resposta de sucesso | `204 No Content`. O Recrutador passa a cumprir a política `ApprovedCompany` no pedido seguinte. |
| Erros | `404` ERR-20; `409` ERR-30 (Empresa fora de `PENDING`). |
| Requisitos | RF093, RF079, RF112 · UC16 |

```http
POST /api/admin/companies/c4e5f6a7-b8c9-4d0e-a1f2-b3c4d5e6f703/approve HTTP/1.1
Authorization: Bearer <credencial do Administrador>

HTTP/1.1 204 No Content
```

#### 6.11.4. `POST /api/admin/companies/{companyId}/reject` — Recusar o registo com motivo

| Campo | Conteúdo |
| --- | --- |
| Operação | `AdminCompaniesController.Reject` → `ICompanyAdminService.RejectAsync` (`Company.Reject`) |
| Parâmetros | `companyId` (rota, `Guid`) |
| Corpo do pedido | `RejectCompanyRequest`: `reason`, com 10 a 500 caracteres. |
| Resposta de sucesso | `204 No Content`. O motivo fica em `status_reason` e é copiado para a notificação (RF079). |
| Erros | `400` ERR-10 (motivo em falta ou fora do P12); `404` ERR-20; `409` ERR-30. |
| Requisitos | RF094, RF079, RF112; P12 · UC16 |

```http
POST /api/admin/companies/c4e5f6a7-b8c9-4d0e-a1f2-b3c4d5e6f703/reject HTTP/1.1
Authorization: Bearer <credencial do Administrador>
Content-Type: application/json

{ "reason": "A morada indicada não corresponde à certidão permanente da Empresa." }

HTTP/1.1 204 No Content
```

#### 6.11.5. `POST /api/admin/companies/{companyId}/suspend` — Suspender uma Empresa aprovada

| Campo | Conteúdo |
| --- | --- |
| Operação | `AdminCompaniesController.Suspend` → `ICompanyAdminService.SuspendAsync` (`Company.Suspend`) |
| Parâmetros | `companyId` (rota, `Guid`) |
| Resposta de sucesso | `204 No Content`. As vagas da Empresa deixam de ser apresentadas aos Candidatos (RF014); as conversas abertas passam a consulta e `ConversationClosed` é entregue às partes; o Recrutador recebe ERR-07 nas operações `ApprovedCompany`. |
| Erros | `404` ERR-20; `409` ERR-30 (Empresa fora de `APPROVED`). |
| Requisitos | RF095, RF075, RF112 · UC17 |

```http
POST /api/admin/companies/e1f2a3b4-c5d6-4e7f-8a9b-0c1d2e3f4a14/suspend HTTP/1.1
Authorization: Bearer <credencial do Administrador>

HTTP/1.1 204 No Content
```

#### 6.11.6. `POST /api/admin/companies/{companyId}/reactivate` — Reativar uma Empresa suspensa

| Campo | Conteúdo |
| --- | --- |
| Operação | `AdminCompaniesController.Reactivate` → `ICompanyAdminService.ReactivateAsync` (`Company.Reactivate`) |
| Parâmetros | `companyId` (rota, `Guid`) |
| Resposta de sucesso | `204 No Content`. A Empresa volta a `APPROVED`. |
| Erros | `404` ERR-20; `409` ERR-30 (Empresa fora de `SUSPENDED`). |
| Requisitos | RF116, RF075, RF112 · UC17 |

```http
POST /api/admin/companies/e1f2a3b4-c5d6-4e7f-8a9b-0c1d2e3f4a14/reactivate HTTP/1.1
Authorization: Bearer <credencial do Administrador>

HTTP/1.1 204 No Content
```

#### 6.11.7. `GET /api/admin/companies` — Consultar a lista global de Empresas

| Campo | Conteúdo |
| --- | --- |
| Operação | `AdminCompaniesController.ListCompanies` → `ICompanyAdminService.ListCompaniesAsync` |
| Resposta de sucesso | `200 OK` — `CompanyAdminDto[]`: designação, estado e número de vagas `PUBLISHED` de todas as Empresas, por ordem alfabética. |
| Requisitos | RF097 · UC17 |

```http
GET /api/admin/companies HTTP/1.1
Authorization: Bearer <credencial do Administrador>

HTTP/1.1 200 OK
Content-Type: application/json

[
  { "companyId": "c4e5f6a7-b8c9-4d0e-a1f2-b3c4d5e6f703", "companyName": "Lumen Software, Lda.", "status": "APPROVED", "publishedJobs": 4 }
]
```

#### 6.11.8. `GET /api/admin/companies/published-jobs` — Consultar a lista global de vagas publicadas

| Campo | Conteúdo |
| --- | --- |
| Operação | `AdminCompaniesController.ListPublishedJobs` → `ICompanyAdminService.ListPublishedJobsAsync` |
| Resposta de sucesso | `200 OK` — `PublishedJobAdminDto[]`: função, Empresa, data de publicação e número de interesses (todos os estados da decisão) de cada vaga `PUBLISHED`, da publicação mais recente para a mais antiga. |
| Requisitos | RF098 · UC17 |

```http
GET /api/admin/companies/published-jobs HTTP/1.1
Authorization: Bearer <credencial do Administrador>

HTTP/1.1 200 OK
Content-Type: application/json

[
  { "jobId": "5e6f7a8b-9c0d-4e1f-a2b3-c4d5e6f7a805", "title": "Programador Backend .NET", "companyName": "Lumen Software, Lda.", "publishedAt": "2026-10-09T15:00:00Z", "interestCount": 3 }
]
```

### 6.12. `AdminIndicatorsController` — `/api/admin/indicators`

Caso de uso UC19. Serviço: `IIndicatorService`.

#### 6.12.1. `GET /api/admin/indicators` — Consultar o painel de indicadores por período

| Campo | Conteúdo |
| --- | --- |
| Operação | `AdminIndicatorsController.Get` → `IIndicatorService.GetAsync` |
| Autorização | `Admin` |
| Parâmetros | `from` e `to` (consulta, `yyyy-MM-dd`, obrigatórios): datas inicial e final, inclusive, interpretadas no fuso `Europe/Lisbon` (de `from` 00:00 a `to` 23:59:59,999) (DAPI-07). |
| Resposta de sucesso | `200 OK` — `IndicatorsDto` com os sete indicadores do RF096. Os três primeiros referem-se ao estado na data final; os outros quatro contam acontecimentos no período. Só são usadas contagens, sem acesso ao texto das mensagens (RF103). |
| Erros | `400` ERR-10: datas em falta, mal formadas, `from > to` ou `to` posterior à data atual. |
| Requisitos | RF096, RF103 · UC19 |

```http
GET /api/admin/indicators?from=2026-10-01&to=2026-10-09 HTTP/1.1
Authorization: Bearer <credencial do Administrador>

HTTP/1.1 200 OK
Content-Type: application/json

{
  "activeCandidates": 24,
  "approvedCompanies": 5,
  "pendingCompanies": 2,
  "jobsFirstPublished": 51,
  "interestsExpressed": 87,
  "matchesConfirmed": 12,
  "conversationsStarted": 9
}
```

### 6.13. `ReferenceListsController` — `/api/reference-lists`

Caso de uso UC20. Serviço: `IReferenceListService`. As listas são devolvidas por ordem alfabética da designação. As localidades não têm operações de escrita (DA, F002).

#### 6.13.1. `GET /api/reference-lists/locations` — Consultar as localidades

| Campo | Conteúdo |
| --- | --- |
| Operação | `ReferenceListsController.ListLocations` → `IReferenceListService.ListLocationsAsync` |
| Autorização | Qualquer conta (modelo de classes, secção 6: «consulta com sessão iniciada»). O registo do Candidato (RF003) exige a localidade antes de existir sessão: contradição registada na secção 11, PA-04. |
| Resposta de sucesso | `200 OK` — `ReferenceItemDto[]`. `name` tem a forma «Localidade (Distrito)», para distinguir localidades com o mesmo nome (modelo de dados, `location`) (DAPI-08). As coordenadas não são enviadas. |
| Requisitos | RF003, RF006, RF041, RF050 · UC03, UC04, UC07, UC09 |

```http
GET /api/reference-lists/locations HTTP/1.1
Authorization: Bearer <credencial>

HTTP/1.1 200 OK
Content-Type: application/json

[
  { "id": "1a2b3c4d-5e6f-4a7b-8c9d-0e1f2a3b4c07", "name": "Braga (Braga)" },
  { "id": "1a2b3c4d-5e6f-4a7b-8c9d-0e1f2a3b4c08", "name": "Guimarães (Braga)" }
]
```

#### 6.13.2. `GET /api/reference-lists/skills` — Consultar as competências

| Campo | Conteúdo |
| --- | --- |
| Operação | `ReferenceListsController.ListSkills` → `IReferenceListService.ListSkillsAsync` |
| Autorização | Qualquer conta |
| Resposta de sucesso | `200 OK` — `ReferenceItemDto[]` |
| Requisitos | RF008, RF050 · UC04, UC09, UC20 |

```http
GET /api/reference-lists/skills HTTP/1.1
Authorization: Bearer <credencial>

HTTP/1.1 200 OK
Content-Type: application/json

[
  { "id": "7c6b5a49-3827-4615-a0b9-c8d7e6f5a409", "name": "C#" },
  { "id": "7c6b5a49-3827-4615-a0b9-c8d7e6f5a411", "name": "Docker" },
  { "id": "7c6b5a49-3827-4615-a0b9-c8d7e6f5a410", "name": "SQL" }
]
```

#### 6.13.3. `GET /api/reference-lists/benefits` — Consultar os benefícios

| Campo | Conteúdo |
| --- | --- |
| Operação | `ReferenceListsController.ListBenefits` → `IReferenceListService.ListBenefitsAsync` |
| Autorização | Qualquer conta |
| Resposta de sucesso | `200 OK` — `ReferenceItemDto[]` |
| Requisitos | RF050 · UC09, UC20 |

```http
GET /api/reference-lists/benefits HTTP/1.1
Authorization: Bearer <credencial>

HTTP/1.1 200 OK
Content-Type: application/json

[ { "id": "6b5a4938-2716-4504-9f8e-d7c6b5a4f312", "name": "Seguro de saúde" } ]
```

#### 6.13.4. `POST /api/reference-lists/skills` — Acrescentar uma competência

| Campo | Conteúdo |
| --- | --- |
| Operação | `ReferenceListsController.AddSkill` → `IReferenceListService.AddSkillAsync` |
| Autorização | `Admin` |
| Corpo do pedido | `ReferenceItemRequest`: `name`, não vazio, até 100 caracteres, único sem distinção de maiúsculas. |
| Resposta de sucesso | `201 Created` — `ReferenceItemDto`. Regista `SKILL_CREATED` (RF112). |
| Erros | `400` ERR-10 (designação vazia ou já existente). |
| Requisitos | RF099, RF112 · UC20 |

```http
POST /api/reference-lists/skills HTTP/1.1
Authorization: Bearer <credencial do Administrador>
Content-Type: application/json

{ "name": "Kubernetes" }

HTTP/1.1 201 Created
Content-Type: application/json

{ "id": "7c6b5a49-3827-4615-a0b9-c8d7e6f5a412", "name": "Kubernetes" }
```

#### 6.13.5. `PUT /api/reference-lists/skills/{skillId}` — Alterar a designação de uma competência

| Campo | Conteúdo |
| --- | --- |
| Operação | `ReferenceListsController.RenameSkill` → `IReferenceListService.RenameSkillAsync` (`Skill.Rename`) |
| Autorização | `Admin` |
| Parâmetros | `skillId` (rota, `Guid`) |
| Corpo do pedido | `ReferenceItemRequest`: `name` |
| Resposta de sucesso | `200 OK` — `ReferenceItemDto`. A nova designação aparece em todos os perfis e vagas que usam a competência. Regista `SKILL_RENAMED` (RF112). |
| Erros | `400` ERR-10 (vazia ou já existente noutra competência); `404` ERR-20. |
| Requisitos | RF100, RF112 · UC20 |

```http
PUT /api/reference-lists/skills/7c6b5a49-3827-4615-a0b9-c8d7e6f5a410 HTTP/1.1
Authorization: Bearer <credencial do Administrador>
Content-Type: application/json

{ "name": "c#" }

HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "message": "Os dados enviados não cumprem a validação automática.",
  "fields": [ { "field": "name", "reason": "A designação já existe na lista." } ]
}
```

#### 6.13.6. `POST /api/reference-lists/benefits` — Acrescentar um benefício

| Campo | Conteúdo |
| --- | --- |
| Operação | `ReferenceListsController.AddBenefit` → `IReferenceListService.AddBenefitAsync` |
| Autorização | `Admin` |
| Corpo do pedido | `ReferenceItemRequest`: `name`, com as regras de 6.13.4. |
| Resposta de sucesso | `201 Created` — `ReferenceItemDto`. Regista `BENEFIT_CREATED` (RF112). |
| Erros | `400` ERR-10. |
| Requisitos | RF101, RF112 · UC20 |

```http
POST /api/reference-lists/benefits HTTP/1.1
Authorization: Bearer <credencial do Administrador>
Content-Type: application/json

{ "name": "Horário flexível" }

HTTP/1.1 201 Created
Content-Type: application/json

{ "id": "6b5a4938-2716-4504-9f8e-d7c6b5a4f313", "name": "Horário flexível" }
```

#### 6.13.7. `PUT /api/reference-lists/benefits/{benefitId}` — Alterar a designação de um benefício

| Campo | Conteúdo |
| --- | --- |
| Operação | `ReferenceListsController.RenameBenefit` → `IReferenceListService.RenameBenefitAsync` (`Benefit.Rename`) |
| Autorização | `Admin` |
| Parâmetros | `benefitId` (rota, `Guid`) |
| Corpo do pedido | `ReferenceItemRequest`: `name` |
| Resposta de sucesso | `200 OK` — `ReferenceItemDto`. Regista `BENEFIT_RENAMED` (RF112). |
| Erros | `400` ERR-10; `404` ERR-20. |
| Requisitos | RF102, RF112 · UC20 |

```http
PUT /api/reference-lists/benefits/6b5a4938-2716-4504-9f8e-d7c6b5a4f312 HTTP/1.1
Authorization: Bearer <credencial do Administrador>
Content-Type: application/json

{ "name": "Seguro de saúde extensível ao agregado" }

HTTP/1.1 200 OK
Content-Type: application/json

{ "id": "6b5a4938-2716-4504-9f8e-d7c6b5a4f312", "name": "Seguro de saúde extensível ao agregado" }
```

---

## 7. Ligação persistente (hubs)

As mensagens e as notificações novas são entregues por uma ligação bidirecional persistente entre cada cliente e o backend, como exige a DA v02 (F010 e F011), com ASP.NET Core SignalR (arquitetura, AD-02 e D-08; modelo de classes, secção 6.1 e DC-07). Não há consulta periódica. Os hubs só enviam eventos do servidor para o cliente: não expõem métodos invocáveis, e o envio de mensagens, a marcação como lida e o encerramento continuam nos endpoints da secção 6.

### 7.1. Ligação e autenticação

| Aspeto | Regra |
| --- | --- |
| Hubs | `/hubs/messages` (`MessagesHub`) e `/hubs/notifications` (`NotificationsHub`). |
| Bibliotecas cliente | Área de gestão web: `@microsoft/signalr`. Aplicação móvel: cliente SignalR para Dart (a fixar no `pubspec.yaml`, com a ratificação de D-08). |
| Transporte | WebSockets sobre TLS (`wss://`), com recurso automático a Server-Sent Events e long polling se o WebSocket falhar, gerido pela biblioteca; o contrato dos eventos é o mesmo. |
| Protocolo | JSON, com as mesmas convenções da secção 2.3 (camelCase e enumerações em maiúsculas; `AddJsonProtocol` com as opções da API). |
| Negociação | `POST /hubs/{hub}/negotiate?negotiateVersion=1`, seguida da ligação. |
| Credencial | A mesma credencial JWT dos pedidos HTTP. Os navegadores não permitem cabeçalhos no WebSocket, pelo que a biblioteca a envia no parâmetro `access_token` da consulta (`accessTokenFactory`); o backend lê-o apenas nas rotas `/hubs/**` (evento `OnMessageReceived` do JWT Bearer). |
| Autorização | Política `CandidateOrRecruiter` e conta ativa, verificadas em `OnConnectedAsync`. O Administrador é recusado na ligação (RF103). Credencial inválida ou expirada → a negociação devolve `401`. |
| Associação ao utilizador | Cada ligação fica associada ao `sub` da credencial (`IUserIdProvider`). Os eventos são enviados com `Clients.User(userId)`, pelo que chegam a todas as ligações abertas do mesmo utilizador e só a ele (RNF006). |
| Registo de diagnóstico | O parâmetro `access_token` é retirado dos registos de pedidos HTTP e do SignalR, tal como o cabeçalho `Authorization` e o texto das mensagens (RNF017). |
| Expiração | A credencial é validada na ligação; ao fim das 8 horas, a reconexão falha com `401` e o cliente pede novo início de sessão (RNF005). |
| Reconexão | `withAutomaticReconnect`. Depois de reconectar, o cliente volta a obter a lista de conversas (6.8.1), o histórico aberto (6.8.2) e a área de notificações (6.9.1), para recuperar o que foi emitido enquanto estava desligado (modelo de classes, secção 6.1). |
| Momento da entrega | O `IRealtimePublisher` só é chamado depois de confirmada a transação, para que nunca se entregue um elemento que não ficou gravado; a entrega cumpre os 5 segundos do P17. |

```javascript
// Área de gestão web — exemplo de ligação ao hub de mensagens
const connection = new signalR.HubConnectionBuilder()
  .withUrl("https://<servidor>/hubs/messages", { accessTokenFactory: () => token })
  .withAutomaticReconnect()
  .build();
connection.on("MessageReceived", (message) => { /* acrescentar a mensagem; ver PA-02 */ });
connection.on("ConversationClosed", (matchId) => { /* passar a conversa a modo apenas de consulta */ });
await connection.start();
```

### 7.2. `MessagesHub` — `/hubs/messages`

| Evento | Argumentos | Destinatários | Origem | Requisitos |
| --- | --- | --- | --- | --- |
| `MessageReceived` | `message` (`MessageDto`) | As duas partes da conversa (o remetente recebe-o nas suas outras ligações abertas) | `POST /api/conversations/{matchId}/messages` (6.8.3) | RF029, RF073, RF070; P17 |
| `ConversationClosed` | `matchId` (`Guid`) | As duas partes | Encerramento por uma das partes (6.8.4); bloqueio ou suspensão de conta (6.10.3, 6.10.4); suspensão de Empresa (6.11.5) | RF030, RF074, RF075 |

Os eventos e os argumentos são os do modelo de classes v01 (secção 6.1; `IRealtimePublisher.PublishMessageAsync(recipientId, MessageDto)`). O `MessageDto` não identifica a conversa: com o contrato v01, o cliente só consegue apresentar a mensagem na conversa que tem aberta e tem de voltar a obter a lista de conversas (6.8.1) para atualizar os contadores das restantes. Esta limitação afeta o RF029 e o RF073 e está registada na secção 11, PA-02. Ao receber `ConversationClosed`, o cliente volta a obter a lista de conversas para ler o motivo (`closeReason`).

Exemplo de mensagem do protocolo JSON do SignalR (o carácter final `0x1E` é o separador do protocolo):

```json
{
  "type": 1,
  "target": "MessageReceived",
  "arguments": [
    {
      "messageId": "2e3f4a5b-6c7d-4e8f-9a0b-1c2d3e4f5a13",
      "senderId": "3f2a9c1e-7b4d-4e8a-9c21-5d6e7f8a9b01",
      "content": "Bom dia. Sim, de manhã é ideal para mim.",
      "sentAt": "2026-10-09T16:20:05Z",
      "readAt": null
    }
  ]
}
```

### 7.3. `NotificationsHub` — `/hubs/notifications`

| Evento | Argumentos | Destinatário | Requisitos |
| --- | --- | --- | --- |
| `NotificationReceived` | `notification` (`NotificationDto`), `unreadCount` (`int`, número de não lidas já com esta) | O destinatário da notificação | RF110, RF111, RF034, RF080; P17 |

Tipos de notificação, texto gerado pelo `NotificationService` e elemento de acesso (`targetId`) para o RF036 e o RF082:

| `type` | Destinatário | Acontecimento (endpoint) | `text` | `targetId` | O cliente abre | Requisitos |
| --- | --- | --- | --- | --- | --- | --- |
| `MATCH_CONFIRMED` | Candidato | Aceitação (6.6.4) | Novo match: {função} — {Empresa}. | `matchId` | Lista de matches, no match | RF031, RF036 |
| `MATCH_CONFIRMED` | Recrutador | Aceitação (6.6.4) | Novo match: {Candidato} — {função}. | `matchId` | Lista de matches, no match | RF077, RF082 |
| `NEW_MESSAGE` | Destinatário da mensagem | Envio (6.8.3) | Nova mensagem de {remetente} na conversa sobre a vaga {função}. | `matchId` | Conversa | RF032, RF078 |
| `NEW_INTEREST` | Recrutador | Interesse (6.3.5) | Novo interesse na vaga {função}. | `jobId` | Candidatos em espera da vaga | RF076, RF082 |
| `COMPANY_APPROVED` | Recrutador | Aprovação (6.11.3) | O registo da Empresa {designação} foi aprovado. | `companyId` | Estado do pedido de registo | RF079, RF082 |
| `COMPANY_REJECTED` | Recrutador | Recusa (6.11.4) | O registo da Empresa {designação} foi recusado. Motivo: {motivo}. | `companyId` | Estado do pedido de registo | RF079, RF082 |
| `INTEREST_QUOTA_RESTORED` | Candidato | Tarefa periódica `QuotaRestoreWorker` | A sua quota de interesses foi reposta: tem 10 interesses disponíveis. | `null` | Área de exploração de vagas | RF033, RF036 |

O texto da notificação `NEW_MESSAGE` identifica a conversa e não reproduz o conteúdo da mensagem. Os nomes de Candidato só aparecem em notificações do Recrutador depois de existir match ou interesse.

```json
{
  "type": 1,
  "target": "NotificationReceived",
  "arguments": [
    {
      "notificationId": "4f5a6b7c-8d9e-4f0a-b1c2-d3e4f5a6b713",
      "type": "NEW_MESSAGE",
      "text": "Nova mensagem de Ana Ferreira na conversa sobre a vaga Programador Backend .NET.",
      "targetId": "9d8c7b6a-5f4e-4d3c-b2a1-0f9e8d7c6b06",
      "createdAt": "2026-10-09T16:20:05Z",
      "isRead": false
    },
    1
  ]
}
```

### 7.4. Sequência do envio de uma mensagem

```mermaid
sequenceDiagram
    autonumber
    participant M as Aplicação móvel<br/>(Candidato)
    participant API as ConversationsController
    participant S as ConversationService
    participant DB as PostgreSQL
    participant P as IRealtimePublisher
    participant W as Área de gestão web<br/>(Recrutador)
    M->>API: POST /api/conversations/{matchId}/messages
    API->>S: SendAsync(userId, matchId, request)
    S->>DB: verifica match e conversa aberta,<br/>grava message, last_message_at e notificação NEW_MESSAGE
    DB-->>S: transação confirmada
    S->>P: PublishMessageAsync(...) e PublishNotificationAsync(...)
    P-->>W: MessagesHub · MessageReceived(message)
    P-->>M: MessagesHub · MessageReceived(message)
    P-->>W: NotificationsHub · NotificationReceived(notification, unreadCount)
    S-->>API: MessageDto
    API-->>M: 201 Created · MessageDto
    Note over P,W: entrega em ≤ 5 s após o envio (P17)
```

---

## 8. Formatos dos DTOs

Os DTOs são os `record` do modelo de classes (secção 7). Os nomes das propriedades JSON são os dos atributos em camelCase. Na coluna «Obrig.», «Sim» num pedido significa que o campo tem de estar presente e não vazio; numa resposta, que nunca é `null`. As regras são aplicadas pelos atributos de validação do ASP.NET Core e, de novo, nos serviços (RNF008).

### 8.1. Autenticação

| DTO | Campo | Tipo JSON | Obrig. | Regras |
| --- | --- | --- | --- | --- |
| `RegisterCandidateRequest` | `fullName` | texto | Sim | Até 150 caracteres. |
| | `email` | texto | Sim | local@domínio, até 255; único (sem distinção de maiúsculas). |
| | `phoneNumber` | texto | Sim | Exatamente 9 algarismos. |
| | `locationId` | `Guid` | Sim | Localidade da lista. |
| | `password` | texto | Sim | P05: pelo menos 8 caracteres, uma letra e um algarismo; até 128. |
| | `acceptTerms` | booleano | Sim | Tem de ser `true`. |
| `CreateRecruiterAccountRequest` | `email`, `password`, `acceptTerms` | | Sim | Como no `RegisterCandidateRequest`. |
| `LoginRequest` | `email`, `password` | texto | Sim | Só se verifica que não estão vazios; o formato não é revelado (RNF009). |
| `LoginResponse` | `token` | texto | Sim | Credencial JWT. |
| | `expiresAt` | data e hora | Sim | Emissão + 8 horas. |
| | `userType` | `UserType` | Sim | |
| | `userId` | `Guid` | Sim | |
| `ChangePasswordRequest` | `currentPassword` | texto | Sim | Tem de coincidir com a atual. |
| | `newPassword` | texto | Sim | P05. |

### 8.2. Perfil profissional e exploração

| DTO | Campo | Tipo JSON | Obrig. | Regras |
| --- | --- | --- | --- | --- |
| `CandidateProfileDto` | `candidateId`, `fullName`, `email`, `phoneNumber`, `locationId`, `locationName` | `Guid` / texto | Sim | Dados de registo e localidade. |
| | `desiredRole`, `availability`, `experienceSummary` | texto / `Availability` / texto | Não | |
| | `photoUrl` | texto | Não | URL da API (secção 2.6). |
| | `hasCv` | booleano | Sim | |
| | `links` | texto[] | Sim | 0 a 3, pela ordem de `position`. |
| | `skills` | `ProfileSkillDto[]` | Sim | |
| | `preferences` | `SearchPreferencesDto` | Sim | |
| `ProfileSkillDto` | `candidateSkillId` | `Guid` | Sim | Usado em 6.2.5. |
| | `name` | texto | Sim | Designação da lista ou texto «Outro». |
| | `isCustom` | booleano | Sim | `true` nas competências «Outro». |
| `UpdateCandidateProfileRequest` | `desiredRole` | texto | Não | Até 150. |
| | `locationId` | `Guid` | Sim | Localidade da lista. |
| | `availability` | `Availability` | Não | P14. |
| | `experienceSummary` | texto | Não | Até 1000 (P08). |
| `SearchPreferencesDto` | `maxDistanceKm` | inteiro | Não | 1 a 500 (P06); `null` = sem filtro. |
| | `minSalaryExpectation` | número | Não | ≥ 0, 2 casas decimais; `null` = sem filtro. |
| | `workModes` | `WorkMode[]` | Sim | Sem repetidos; `[]` = todos. |
| | `contractTypes` | `ContractType[]` | Sim | Sem repetidos; `[]` = todos. |
| `AddSkillRequest` | `skillId` | `Guid` | Um dos dois | Competência da lista, ainda não associada. |
| | `customLabel` | texto | Um dos dois | P07. |
| `SetLinksRequest` | `urls` | texto[] | Sim | 0 a 3; cada um com `http://` ou `https://`, até 500. |
| `JobCardDto` | `jobId`, `title`, `companyName`, `minSalary`, `maxSalary`, `locationName`, `distanceKm`, `contractType`, `workMode`, `isUrgent`, `photoUrls` | | Sim | `distanceKm` inteiro em km (RF015); `photoUrls` 0 a 5. |
| | `logoUrl` | texto | Não | Só quando registado (RF013). |
| `JobDetailDto` | `card` | `JobCardDto` | Sim | |
| | `description` | texto | Sim | |
| | `skills`, `benefits` | texto[] | Sim | Designações. |
| | `companyId` | `Guid` | Sim | Para 6.3.3. |
| `CompanyPageDto` | `companyId`, `companyName`, `photoUrls` | | Sim | `photoUrls` 0 a 6, pela ordem da galeria. |
| | `description`, `website`, `logoUrl` | texto | Não | |
| `QuotaDto` | `available` | inteiro | Sim | 0 a 10. |
| | `blockedUntil` | data e hora | Não | Só com `available = 0`. |

### 8.3. Empresa e vagas

| DTO | Campo | Tipo JSON | Obrig. | Regras |
| --- | --- | --- | --- | --- |
| `CompanyRegistrationRequest` | `companyName` | texto | Sim | Até 200. |
| | `taxId` | texto | Sim | 9 algarismos; único; imutável depois da aprovação. |
| | `industry` | `Industry` | Sim | P15. |
| | `address` | texto | Sim | Até 255. |
| | `locationId` | `Guid` | Sim | Localidade da lista. |
| | `contactEmail` | texto | Sim | local@domínio, até 255. |
| | `contactPhone` | texto | Sim | 9 algarismos. |
| | `responsibleName` | texto | Sim | Até 150. |
| `CompanyRegistrationDto` | os oito dados (com `locationName` em vez de `locationId`), `companyId`, `submittedAt` | | Sim | |
| `CompanyStatusDto` | `companyId`, `status`, `submittedAt` | | Sim | |
| | `rejectionReason` | texto | Não | Só no estado `REJECTED`. |
| `CompanyPageRequest` | `description` | texto | Não | Até 1000 (P08). |
| | `website` | texto | Não | `http://` ou `https://`, até 500 (P10). |
| `JobRequest` | `title` | texto | Sim | Até 150. |
| | `description` | texto | Sim | Até 5000 (limite técnico do pedido; a coluna é `text`). |
| | `minSalary`, `maxSalary` | número | Sim | > 0, até 99 999 999,99; `minSalary ≤ maxSalary` (RF051). |
| | `locationId` | `Guid` | Sim | Localidade da lista. |
| | `contractType`, `workMode` | enumeração | Sim | P09, P16. |
| | `skillIds` | `Guid[]` | Sim | Pelo menos 1, sem repetidos, da lista. |
| | `benefitIds` | `Guid[]` | Não | Sem repetidos, da lista; ausente = `[]`. |
| | `isUrgent` | booleano | Não | Por omissão `false`. |
| | `expiresAt` | data e hora | Não | Posterior ao instante do pedido. |
| `JobDto` | campos do `JobRequest` (com `skills` e `benefits` em `ReferenceItemDto[]`), `jobId`, `locationName`, `status`, `photoUrls`, `waitingCandidates` | | Sim | |
| | `expiresAt`, `publishedAt` | data e hora | Não | |
| `ReferenceItemDto` | `id`, `name` | `Guid` / texto | Sim | |
| `ReferenceItemRequest` | `name` | texto | Sim | Até 100; única na lista, sem distinção de maiúsculas. |

### 8.4. Avaliação, matches, conversas e notificações

| DTO | Campo | Tipo JSON | Obrig. | Regras |
| --- | --- | --- | --- | --- |
| `WaitingCandidateDto` | `matchId`, `candidateId`, `fullName`, `interestAt` | | Sim | `interestAt` = `candidate_action_at`. |
| `CandidateFullProfileDto` | `matchId`, `fullName`, `locationName`, `links`, `skills`, `hasCv` | | Sim | Sem contactos nem preferências. |
| | `photoUrl`, `desiredRole`, `availability`, `experienceSummary` | | Não | |
| `SkillTagDto` | `name`, `matchesJob` | texto / booleano | Sim | `matchesJob` só pode ser `true` em competências da lista (modelo de dados, 6.8). |
| `MatchDto` | `matchId`, `jobTitle`, `companyName`, `counterpartName`, `contactEmail`, `contactPhone`, `matchedAt` | | Sim | Contactos da outra parte (secção 6.7.1). |
| `ConversationSummaryDto` | `matchId`, `counterpartName`, `jobTitle`, `companyName`, `status`, `unreadCount` | | Sim | |
| | `closeReason` | `ConversationCloseReason` | Não | Só com `status = CLOSED`. |
| | `lastMessageAt` | data e hora | Não | `null` sem mensagens. |
| `MessageDto` | `messageId`, `senderId`, `content`, `sentAt` | | Sim | `senderId` = `userId` do remetente. |
| | `readAt` | data e hora | Não | Leitura pelo destinatário. |
| `SendMessageRequest` | `content` | texto | Sim | 1 a 1000 (P03). |
| `NotificationDto` | `notificationId`, `type`, `text`, `createdAt`, `isRead` | | Sim | |
| | `targetId` | `Guid` | Não | Secção 7.3. |
| `NotificationListDto` | `items`, `unreadCount` | | Sim | |

### 8.5. Supervisão

| DTO | Campo | Tipo JSON | Obrig. | Regras |
| --- | --- | --- | --- | --- |
| `AccountDto` | `userId`, `email`, `userType`, `status` | | Sim | |
| `CandidateAdminDto` | `candidateId`, `fullName`, `email`, `locationName`, `registeredAt`, `status` | | Sim | `registeredAt` = `app_user.created_at`. |
| `PendingCompanyDto` | `companyId`, `companyName`, `taxId`, `submittedAt` | | Sim | |
| `CompanyAdminDto` | `companyId`, `companyName`, `status`, `publishedJobs` | | Sim | |
| `PublishedJobAdminDto` | `jobId`, `title`, `companyName`, `publishedAt`, `interestCount` | | Sim | |
| `RejectCompanyRequest` | `reason` | texto | Sim | 10 a 500 (P12). |
| `IndicatorsDto` | `activeCandidates`, `approvedCompanies`, `pendingCompanies`, `jobsFirstPublished`, `interestsExpressed`, `matchesConfirmed`, `conversationsStarted` | inteiro | Sim | ≥ 0 (RF096). |
| `ApiErrorDto` | `message` | texto | Sim | Secção 4.1. |
| | `fields` | `FieldErrorDto[]` | Sim | `[]` quando o erro não é de um campo. |
| `FieldErrorDto` | `field` | texto | Sim | Nome JSON do campo, com índice nas listas. |
| | `reason` | texto | Sim | Motivo para esse campo. |

---

## 9. Rastreabilidade

### 9.1. Requisitos funcionais

Todos os requisitos funcionais abrangem o componente Backend. A tabela indica, para cada um, o endpoint ou evento que o concretiza, ou o comportamento interno do servidor quando o requisito não tem operação própria na interface do servidor.

| Requisitos | Concretização na API |
| --- | --- |
| RF001, RF038, RF083 | 6.1.3 (com `X-Client-App`, ERR-02 a ERR-05) |
| RF002, RF040, RF084 | 6.1.5 |
| RF003, RF004, RF005 | 6.1.1; localidades em 6.13.1 (com sessão iniciada; ver PA-04) |
| RF006 | 6.2.2; consulta em 6.2.1 |
| RF007, RF023 | 6.2.3 |
| RF008, RF009 | 6.2.4, 6.2.5; lista em 6.13.2 |
| RF010 | 6.2.7 |
| RF011 | 6.2.8; obtenção do CV pelo próprio Candidato: ver PA-10 |
| RF012 | 6.2.6 |
| RF013, RF014, RF015 | 6.3.1 (filtro e distância calculados no servidor; RF015 sem endpoint próprio); apresentação do logótipo e das fotografias depende de PA-01 |
| RF016 | 6.3.2 |
| RF017 | 6.3.3; apresentação das imagens depende de PA-01 |
| RF018 | 6.3.4 |
| RF019, RF020 | 6.3.5 (ERR-31, ERR-32) |
| RF021 | Tarefa periódica `QuotaRestoreWorker`; verificação em 6.3.5 e 6.3.6 |
| RF022 | 6.3.6 |
| RF024, RF025 | 6.7.1; estado da vaga do match: ver PA-08 |
| RF026, RF069 | 6.8.3 |
| RF027, RF071 | 6.8.1 |
| RF028, RF072 | 6.8.2 |
| RF029, RF073 | Evento `MessageReceived` (7.2); limitação PA-02 |
| RF030, RF074 | 6.8.4; evento `ConversationClosed` |
| RF031, RF077 | Notificação `MATCH_CONFIRMED` criada em 6.6.4; evento `NotificationReceived` |
| RF032, RF078 | Notificação `NEW_MESSAGE` criada em 6.8.3 |
| RF033 | Notificação `INTEREST_QUOTA_RESTORED` da tarefa periódica |
| RF034, RF080 | 6.9.1 |
| RF035, RF081 | 6.9.2 |
| RF036, RF082 | `type` e `targetId` de `NotificationDto` (7.3); navegação no cliente |
| RF037 | 6.1.2 |
| RF039 | Política `ApprovedCompany` (ERR-07), também na lista de matches do Recrutador (6.7.1); estado em 6.4.3 |
| RF041, RF042, RF043 | 6.4.1 |
| RF044 | 6.4.3 |
| RF045 | 6.4.2; pré-preenchimento do formulário depende de PA-03 |
| RF046 | 6.4.5; pré-preenchimento depende de PA-03 |
| RF047 | 6.4.6 |
| RF048 | 6.4.7 |
| RF049 | 6.4.4; pré-preenchimento depende de PA-03 |
| RF050, RF051 | 6.5.2; RF051 também em 6.5.3 |
| RF052 | 6.5.9 |
| RF053 | 6.5.4 |
| RF054 | 6.5.3 |
| RF055 | 6.5.5 |
| RF056 | 6.5.7 |
| RF057 | Tarefa periódica `JobExpirationWorker` (sem endpoint) |
| RF058, RF114 | 6.5.8 (ERR-38) |
| RF059 | Verificação de titularidade em 6.5.1 a 6.5.9 (ERR-08) |
| RF060 | 6.6.1; contagem em `JobDto.waitingCandidates`; dados apresentados na lista: ver PA-07 |
| RF061 | 6.6.2, 6.6.3; apresentação da fotografia depende de PA-01 |
| RF062 | `SkillTagDto.matchesJob` em 6.6.2 |
| RF063, RF066, RF068 | 6.6.4 |
| RF064 | 6.6.5 |
| RF065 | Efeito interno de 6.6.4 e 6.6.5 (`decided_by`, `recruiter_action_at`) |
| RF067 | 6.7.1; estado da vaga do match: ver PA-08 |
| RF070 | Regra de 6.8.2 a 6.8.4 (ERR-37); hub só para as partes |
| RF075 | Efeito interno de 6.10.3, 6.10.4 e 6.11.5; evento `ConversationClosed`; `closeReason` em 6.8.1 |
| RF076 | Notificação `NEW_INTEREST` criada em 6.3.5 |
| RF079 | Notificações `COMPANY_APPROVED` e `COMPANY_REJECTED` criadas em 6.11.3 e 6.11.4 |
| RF085 | 6.10.1 |
| RF086 | 6.10.3 |
| RF087, RF090 | 6.10.5 |
| RF088 | 6.10.2 |
| RF089 | 6.10.4 |
| RF091 | 6.11.1 |
| RF092 | 6.11.2 |
| RF093 | 6.11.3 |
| RF094 | 6.11.4 |
| RF095 | 6.11.5 |
| RF096 | 6.12.1 |
| RF097 | 6.11.7 |
| RF098 | 6.11.8 |
| RF099, RF100 | 6.13.4, 6.13.5 |
| RF101, RF102 | 6.13.6, 6.13.7 |
| RF103 | Política `CandidateOrRecruiter` em 6.8 e nos hubs (ERR-06); 6.12.1 só com contagens |
| RF104 | Política `Admin` em `/api/admin/**` e na escrita das listas |
| RF105, RF106, RF107 | 6.1.4 |
| RF108, RF109 | Efeito de 6.8.2 |
| RF110, RF111 | Evento `NotificationReceived` (7.3) |
| RF112 | Efeito interno das operações indicadas em cada endpoint (`operation_log`; interesse, recusa e decisão na linha de `match`) |
| RF113 | Ausência de expiração: nenhum endpoint nem tarefa altera um interesse `WAITING` além de 6.6.4 e 6.6.5 |
| RF115 | 6.5.6 |
| RF116 | 6.11.6 |
| RF117 | 6.6.4, 6.6.5 (ERR-35) |
| RF118 | Efeito de 6.6.2; apresentação da data: ver PA-09 |

Os 118 requisitos funcionais têm uma operação da interface do servidor, um evento ou um comportamento interno que os concretiza. Em nove deles (RF003, RF013, RF017, RF029, RF045, RF046, RF049, RF061, RF073), o funcionamento completo depende da correção dos pontos em aberto PA-01 a PA-04 (secção 11), que residem no modelo de classes e na especificação de requisitos. Os RF015, RF021, RF033, RF057, RF065, RF112, RF113 e RF118 não têm endpoint próprio, porque são comportamentos internos do servidor desencadeados por outros endpoints ou pelas tarefas periódicas.

### 9.2. Requisitos não funcionais

| Requisito | Concretização na API |
| --- | --- |
| RNF001, RNF002, RNF015 | Limites de tempo verificados por pedido direto em `m3` (P19); listas sem paginação dimensionadas para os dados de demonstração (L-02). |
| RNF003 | 6.1.1, 6.1.2, 6.1.5: palavras-passe só em resumo; nunca devolvidas em nenhum DTO. |
| RNF004, RNF005 | Secções 3.1 e 3.4: credencial em todas as operações reservadas, expiração de 8 horas, ERR-01; também nos hubs. |
| RNF006 | Identificador sempre da credencial (2.5); políticas e titularidade (3.3); ERR-06, ERR-08. |
| RNF007 | 6.6.3: CV só para o Recrutador de uma vaga com o Candidato em espera; nenhum URL público de CV. A obtenção pelo próprio Candidato, que o RNF007 admite, não tem operação no modelo de classes v01 (PA-10). |
| RNF008 | Validação no servidor de todos os DTOs de pedido (secção 8), com ERR-10 e ERR-11. |
| RNF009 | ERR-02, mensagem única. |
| RNF013 | Respostas de sucesso só depois de confirmada a transação; o mesmo para os eventos (7.1). |
| RNF016 | Validação da assinatura dos ficheiros (2.6). |
| RNF017 | Respostas de erro sem dados internos (4.1); `access_token`, `Authorization` e texto das mensagens fora dos registos de diagnóstico (7.1). |

### 9.3. Controllers, hubs e casos de uso

| Controller ou hub | Endpoints ou eventos | Casos de uso |
| --- | --- | --- |
| `AuthController` | 5 | UC01, UC02, UC03, UC07 |
| `CandidateProfileController` | 8 | UC04, UC05 |
| `JobExplorationController` | 6 | UC05, UC06 |
| `CompanyController` | 7 | UC07, UC08 |
| `JobsController` | 9 | UC09, UC10 |
| `CandidateEvaluationController` | 5 | UC11, UC12 |
| `MatchesController` | 1 | UC13 |
| `ConversationsController` | 4 | UC14 |
| `NotificationsController` | 2 | UC15 |
| `AdminAccountsController` | 5 | UC18 |
| `AdminCompaniesController` | 8 | UC16, UC17 |
| `AdminIndicatorsController` | 1 | UC19 |
| `ReferenceListsController` | 7 | UC20 |
| `MessagesHub` | 2 eventos | UC14 |
| `NotificationsHub` | 1 evento | UC15 |

Todas as operações dos 13 controllers da secção 6 do modelo de classes estão documentadas, e não há endpoints sem operação correspondente.

---

## 10. Decisões da documentação da API

| ID | Decisão | Fundamentação |
| --- | --- | --- |
| DAPI-01 | Enumerações em JSON com os valores do PostgreSQL (`SERVICE_PROVISION`), e não em PascalCase nem em números. | Um único vocabulário entre base de dados, API, clientes e collection Postman; os números quebrariam se a ordem das enumerações mudasse. |
| DAPI-02 | Ponto de acesso indicado no cabeçalho `X-Client-App` do início de sessão. | A operação `LoginAsync` do modelo de classes recebe um `ClientApp` que o `LoginRequest` não contém; um cabeçalho mantém o DTO inalterado e serve as duas aplicações e os pedidos diretos. |
| DAPI-03 | Unicidade de dados de formulário em `400` com o campo; `409` só para conflitos de estado. | RF004, RF037 e RF042 tratam o endereço ou o NIF já registados como parte da validação automática, com indicação do campo. |
| DAPI-04 | `403` (e não `404`) para elementos de outro utilizador ou Empresa. | UC12, exceção E3 («rejeita a operação por falta de autorização»); os identificadores são `Guid` aleatórios, pelo que a resposta não facilita a enumeração. |
| DAPI-05 | Três consultas em `GET` com efeito registado no servidor: 6.3.6 (reposição da quota vencida), 6.6.2 (primeira abertura do perfil) e 6.8.2 (marcação das mensagens como lidas). | Os três efeitos são idempotentes e decorrem do próprio ato de consultar, como os requisitos os descrevem (RF021, RF118, RF108, RF109); o modelo de classes modela-os como operações de consulta (`GetQuota`, `OpenProfile`, `Open`). Um `POST` obrigaria o cliente a dois pedidos para uma só consulta. |
| DAPI-06 | Ações de mudança de estado como `POST …/{id}/<ação>`. | Cada transição tem pré-condições e efeitos próprios (notificações, `operation_log`, conversas), que um `PATCH` genérico do estado esconderia. |
| DAPI-07 | Datas dos indicadores interpretadas no fuso `Europe/Lisbon`. | O RF096 fala em datas do calendário do Administrador; o armazenamento continua em UTC. |
| DAPI-08 | Localidades apresentadas como «Localidade (Distrito)» no `ReferenceItemDto`. | O `ReferenceItemDto` só tem `name`; o modelo de dados admite localidades com o mesmo nome em distritos diferentes. |
| DAPI-09 | Rotas, métodos, códigos de estado, mensagens de erro e cabeçalhos fixados neste documento; operações, DTOs, eventos e autorização seguem sem alterações o modelo de classes v01. | O modelo de classes remete para a documentação da API apenas as rotas, os códigos de estado e os exemplos (secção 11); alterar o resto exige uma nova versão do modelo de classes (secção 11, pontos em aberto). |
| DAPI-10 | Carregamentos de ficheiro único em `PUT` com `204` e `Location`; acrescentos a galerias em `POST` com `201` e `Location`. | As operações do modelo de classes devolvem `IActionResult` e o serviço devolve o URL; o cabeçalho evita criar DTOs novos. |
| DAPI-11 | Em caso de divergência entre a documentação gerada pelo Swagger e este documento, prevalece este documento até à aprovação de uma nova versão. | Este documento é o contrato acordado entre o backend e os dois clientes (objetivo da `I092`); o Swagger é gerado a partir da implementação e reproduz também os erros dela. |

---

## 11. Pontos em aberto e limitações

### 11.1. Pontos em aberto

Lacunas encontradas na elaboração deste documento, nos artefactos de que ele depende. Este documento não as resolve, para não alterar artefactos aprovados sem decisão formal: documenta o contrato v01 tal como está e indica aqui o impacto e a proposta de correção. A classificação segue a secção 5.3 do Plano de Qualidade. Os defeitos de tarefas já concluídas (`Done`) são defeitos escapados (métrica M-05) e são comunicados para registo como não conformidades (Plano de Qualidade, secção 10). As incoerências entre dois artefactos aprovados em que nenhum deles está em falta isoladamente (PA-07 a PA-09) são classificadas como propostas de melhoria e não constituem não conformidade.

| ID | Ponto | Impacto | Proposta | Origem e classificação | Decisão formal |
| --- | --- | --- | --- | --- | --- |
| PA-01 | Não há operação para servir os ficheiros referidos pelos URL dos DTOs (`photoUrl`, `logoUrl`, `photoUrls`), embora o modelo de classes diga que são «endereços da API que servem o ficheiro depois de verificar a autorização» (secção 7.2). | Os clientes não conseguem apresentar fotografias nem logótipos (RF013, RF017, RF061). | Acrescentar `FilesController` com `GET /api/files/{fileId}`: imagens de Empresa e de vaga para qualquer conta; fotografia de perfil só para o próprio Candidato e para Recrutadores de vagas onde tenha interesse; o CV continua só em 6.6.3. Identificador do ficheiro: (a) o nome gerado pelo `LocalFileStorage`, sem mudar o modelo de dados (recomendada); (b) uma tabela de ficheiros. | Modelo de classes (`I036`) · defeito substancial | Sim, na escolha entre (a) e (b), porque (b) altera o modelo de dados. |
| PA-02 | `IRealtimePublisher.PublishMessageAsync(recipientId, MessageDto)` e o `MessageDto` não identificam a conversa. | O cliente não sabe em que conversa mostrar a mensagem recebida nem que contador atualizar (RF029, RF073). | Acrescentar `Guid matchId` a `PublishMessageAsync` e enviar `MessageReceived(matchId, message)`. | Modelo de classes (`I036`) · defeito substancial | Não; correção de desenho. |
| PA-03 | O Recrutador não tem operação para consultar os dados de registo nem a página de apresentação da própria Empresa: `GetCompanyPage` é do Candidato e `GetSubmittedData` do Administrador. | A área de gestão web não consegue preencher os formulários de 6.4.2, 6.4.4 e 6.4.5 com os valores atuais (RF045, RF046, RF049). | Acrescentar ao `CompanyController`: `GET /api/recruiter/company` (`CompanyRegistrationDto`, com `LocationId` acrescentado ao DTO), com a política `Recruiter`, e não `ApprovedCompany`, porque os dados de registo são lidos com a Empresa em qualquer estado (pendente, recusada para correção, RF045, aprovada ou suspensa; ecrãs W05, W06 e W26 do protótipo da `I040`), com `404` sem Empresa registada; e `GET /api/recruiter/company/page` (`CompanyPageDto`), com a política `ApprovedCompany`, igual à da alteração da página (6.4.5). | Modelo de classes (`I036`) · defeito substancial | Não; correção de desenho. |
| PA-04 | A especificação de requisitos (secção 2.2) só considera não reservados o registo, a criação de conta e o início de sessão, e o modelo de classes exige sessão para consultar as listas pré-definidas; mas o registo do Candidato (RF003) exige escolher a localidade da lista antes de haver sessão. | Com os artefactos v01, a aplicação móvel não consegue apresentar a lista de localidades no registo. | Acrescentar a consulta da lista de localidades às operações não reservadas (especificação de requisitos, secção 2.2) e tornar anónimo `GET /api/reference-lists/locations` (modelo de classes, secção 6). Alternativas rejeitadas: lista fixa na aplicação móvel (desatualiza-se); localidade em texto livre (contraria RF003 e RF015). | Especificação de requisitos (`I020`, `I023`) e modelo de classes (`I036`) · defeito substancial | Sim, porque altera requisitos aprovados. |
| PA-05 | `ApiErrorDto` só tem `message` e `fields`, sem código de erro. | Os clientes distinguem situações com o mesmo código HTTP pelo texto da mensagem (por exemplo, ERR-31 e ERR-32 são ambos `409`). | Acrescentar ao `ApiErrorDto` `string Code` (os identificadores ERR-xx da secção 4.3) e um campo opcional com o instante de reposição da quota no ERR-31, para o cliente calcular o tempo em falta sem novo pedido. Até lá, as mensagens da secção 4.3 são fixas e o instante obtém-se em 6.3.6. | Modelo de classes (`I036`) · proposta de melhoria | Não. |
| PA-06 | A tecnologia da ligação persistente (D-08, ASP.NET Core SignalR) e o armazenamento de ficheiros no servidor (AD-01) estão por ratificar em reunião formal. | Se for escolhida outra biblioteca, a secção 7 muda; os endpoints da secção 6 não. | Ratificar D-08 e AD-01 e registar a decisão em ata (Regulamento da UC, secção 10.1). | Arquitetura (`I033`, `I034`) · decisão pendente, não é defeito | Sim. |
| PA-07 | `WaitingCandidateDto` não tem a função pretendida nem a localidade do Candidato, que o ecrã W16 «Candidatos em espera» do protótipo apresenta em colunas próprias. | A lista de candidatos em espera não consegue apresentar as colunas «Função pretendida» e «Localidade» do protótipo aceite. | Acrescentar `string? DesiredRole` e `string LocationName` ao `WaitingCandidateDto`. | Incoerência entre o modelo de classes (`I036`) e o protótipo do Recrutador aceite na `I040` (`m2-s02-i040-20261005-prototipo-baixa-fidelidade-v01.pdf`), detetada na comparação com o modelo de classes do frontend web (`I090`) · proposta de melhoria: nenhum dos artefactos está em falta isoladamente, porque o RF060 só exige a data do interesse, e a `I036` não teve os protótipos como documento de origem (as duas tarefas decorreram em paralelo no Sprint 02); não é não conformidade | Não |
| PA-08 | `MatchDto` não tem o estado da vaga, que o ecrã W21 «Matches da Empresa» do protótipo apresenta na coluna «Estado da vaga». | A lista de matches do Recrutador não consegue distinguir os matches de vagas encerradas, como no protótipo aceite. | Acrescentar `JobStatus JobStatus` ao `MatchDto`. | Incoerência entre o modelo de classes (`I036`) e o protótipo do Recrutador aceite na `I040` (`m2-s02-i040-20261005-prototipo-baixa-fidelidade-v01.pdf`), detetada na comparação com o modelo de classes do frontend web (`I090`) · proposta de melhoria: nenhum dos artefactos está em falta isoladamente, porque o RF067 só exige a vaga, o Candidato e os contactos, e a `I036` não teve os protótipos como documento de origem (as duas tarefas decorreram em paralelo no Sprint 02); não é não conformidade | Não |
| PA-09 | `CandidateFullProfileDto` não tem a data da primeira abertura do perfil, que o ecrã W17 «Perfil completo do Candidato» do protótipo apresenta («Abertura do perfil registada hoje às 10:32»). | O perfil completo não consegue apresentar a indicação de abertura do protótipo aceite. | Acrescentar `DateTimeOffset ProfileOpenedAt` (valor de `match.profile_opened_at`) ao `CandidateFullProfileDto`. | Incoerência entre o modelo de classes (`I036`) e o protótipo do Recrutador aceite na `I040` (`m2-s02-i040-20261005-prototipo-baixa-fidelidade-v01.pdf`), detetada na comparação com o modelo de classes do frontend web (`I090`) · proposta de melhoria: nenhum dos artefactos está em falta isoladamente, porque o RF118 exige o registo da abertura, não a sua apresentação, e a `I036` não teve os protótipos como documento de origem (as duas tarefas decorreram em paralelo no Sprint 02); não é não conformidade | Não |
| PA-10 | Não há operação para o Candidato obter o próprio CV: o `CandidateProfileController` só tem `UploadCv`, e o modelo de dados (secção 7.2) diz «acesso ao CV apenas pelo Recrutador». | O RNF007 admite expressamente o pedido do próprio Candidato, que não consegue confirmar o CV que enviou (RF011). | Acrescentar `GET /api/candidate/profile/cv` (`application/pdf`; `404` sem CV): operação `GetCv` no `CandidateProfileController` e `GetCvAsync` no `ICandidateProfileService`; alinhar a frase do modelo de dados com o RNF007. | Modelo de classes (`I036`) e modelo de dados (`I035`) · defeito | Não |

Encaminhamento: as decisões de PA-01, PA-04 e PA-06 são levadas à próxima reunião formal, preparadas nos termos da secção 15.1 do Regulamento Interno; as correções são feitas em novas versões do modelo de classes, da especificação de requisitos e da arquitetura e, por fim, numa v02 deste documento, através de Issues a criar no Backlog Refinement seguinte.

### 11.2. Limitações

| ID | Limitação | Consequência |
| --- | --- | --- |
| L-01 | O contrato não tem prefixo de versão (`/api/v1`). | Uma alteração incompatível exige atualizar ao mesmo tempo o backend e os dois clientes. |
| L-02 | As listas não têm paginação nem filtros. | Adequado aos volumes dos dados de demonstração (P18); com volumes maiores, a paginação teria de ser acrescentada aos DTOs e aos repositórios. |
| L-03 | A credencial descartada no fim de sessão continua válida até expirar (8 horas). | Risco aceite no modelo de classes (secção 11); uma lista de credenciais revogadas pode ser acrescentada sem mudar o contrato. |
| L-04 | Uma ligação ao hub aberta antes de um bloqueio ou suspensão só é rejeitada na reconexão seguinte. | Não recebe elementos novos, porque as conversas da conta passam a consulta e o envio é rejeitado na API; o pedido HTTP seguinte devolve ERR-03 ou ERR-04. |
| L-05 | A ordem dos cartões de vaga não é fixada pelos requisitos. | Fica definida na implementação de `ListEligibleForCandidateAsync` em `m3`. |
| L-06 | O documento depende do modelo de classes (`I036`), da arquitetura (`I033`) e do modelo de dados (`I035`). | Uma alteração a qualquer deles obriga a rever as secções afetadas e a acrescentar uma linha ao histórico de versões. |
