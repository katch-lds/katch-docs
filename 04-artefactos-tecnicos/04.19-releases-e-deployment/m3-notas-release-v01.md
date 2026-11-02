# Notas de release do backend — m3-v01

| Campo | Valor |
| --- | --- |
| Projeto | Katch — LDS, LEI, 2026/2027, turma LEI3T2, grupo 11 |
| Documento | `m3-notas-release-v01.md` |
| Componente | Backend (API ASP.NET Core e base de dados PostgreSQL) |
| Versão entregue | **m3-v01** |
| Tag Git | `v0.3.0` (anotada, «Release m3-v01 do backend (Milestone m3, Issue I106)») |
| Commit | `3f4a070946035440e8ad9c746b83a7b904478881` — merge do PR #37 na `main`, 2026-11-02 15:30:06 UTC |
| Repositório | `katch-lds/katch-backend` |
| Pipeline CI do commit | Execução 41472193023 do GitHub Actions (evento `push` na `main`): `build-and-test` e `database-scripts` com sucesso |
| Cópia arquivada | `projeto/04-artefactos-tecnicos/04.12-backend/m3-v01` (árvore integral) e `projeto/04-artefactos-tecnicos/04.13-scripts-de-base-de-dados/m3-v01` (scripts SQL) |
| Instalação | `projeto/04-artefactos-tecnicos/04.17-manual-de-instalacao-e-configuracao/m3-manual-instalacao-configuracao-v01.pdf` |
| Issue | I106 (Sprint 06) |

## Histórico de versões

| Versão | Data | Alteração | Issue |
| --- | --- | --- | --- |
| v01 | 2026-11-02 | Primeira versão: release m3-v01 do backend. | I106 |

## 1. Resumo

Primeira release do backend, entregue na milestone m3. Inclui:

- a API REST em ASP.NET Core 10 com **72 endpoints em 13 controllers**, que implementam o contrato da documentação da API v02 (`m3-documentacao-api-v02.md`, secção 6) e o âmbito de backend da Declaração de Âmbito v04 (F001 a F010);
- a persistência em PostgreSQL 18 com EF Core: **17 tabelas** do modelo de dados v02, criadas por **10 migrations**;
- os scripts SQL de criação, reposição, dados iniciais fictícios (P18) e verificação, gerados a partir das migrations ou escritos à mão;
- duas tarefas periódicas: reposição da quota de interesses (`QuotaRestoreWorker`) e encerramento das vagas na data-limite (`JobExpirationWorker`);
- **2000 testes automáticos**: 1229 unitários e 771 de integração (SQLite em memória), todos a passar;
- a pipeline CI (GitHub Actions) com compilação sem avisos, verificação da formatação, testes e execução dos scripts SQL sobre PostgreSQL 18.

A versão resulta de 37 Pull Requests integrados entre 2026-09-21 e 2026-11-02 (36 diretamente na `main` e o #15 na branch da I052, que entrou na `main` pelo #22), com 162 commits sem contar os de merge. Os frontends web e mobile não fazem parte desta release (secção 5, L-01).

## 2. Compatibilidade

| Elemento | Versão exigida ou usada | Notas |
| --- | --- | --- |
| SDK do .NET | **10.0.401 ou superior** na banda 10.0.4xx | `global.json`: `"version": "10.0.401"`, `"rollForward": "latestFeature"`. Testado com o 10.0.402 (máquina Windows) e com o SDK do `actions/setup-dotnet` a partir do mesmo `global.json` (CI). |
| Runtime | ASP.NET Core 10 (`net10.0`) | C# 14 (`LangVersion` 14.0), `Nullable` ativo. .NET 10 é LTS. |
| PostgreSQL | **18** | Base com codificação UTF8 e locale diferente de `C` (o CHECK `ck_candidate_custom_skill_label` usa `[[:alnum:]]`). Testado com o 18.3 em Windows (`Portuguese_Portugal.1252`) e com a imagem `postgres:18` na CI (`en_US.utf8`). |
| Pacotes principais | EF Core 10.0.13, Npgsql.EntityFrameworkCore.PostgreSQL 10.0.3, EFCore.NamingConventions 10.0.1, Microsoft.AspNetCore.Authentication.JwtBearer 10.0.13, Swashbuckle.AspNetCore 10.3.0 | Versões fixadas em `Directory.Packages.props` (Central Package Management, com fixação das dependências indiretas). |
| Ferramenta das migrations | `dotnet-ef` 10.0.13 | Manifesto local `.config/dotnet-tools.json` (`dotnet tool restore`). |
| Testes | xUnit v3 3.2.2, Microsoft.NET.Test.Sdk 18.11.0, Microsoft.EntityFrameworkCore.Sqlite 10.0.13, coverlet.collector 10.1.0 | Os testes de integração não precisam de PostgreSQL. |
| Análise estática | Roslyn Analyzers (`latest-recommended`) e StyleCop.Analyzers 1.2.0-beta.556, com avisos tratados como erros | Configurados em `Directory.Build.props`, `.editorconfig` e `stylecop.json`. |
| Sistema operativo | Windows 10/11 (manual e ensaio); Ubuntu (`ubuntu-latest`, CI) | O manual usa o Windows PowerShell 5.1. |
| Clientes | Contrato da API v02, sem prefixo de versão | Os frontends (m4) têm de usar o mesmo contrato (API v02, L-01). |

## 3. Alterações por Issue

Cada linha corresponde a uma Issue de m3 com código nesta versão. O Pull Request e o commit de merge identificam a integração na `main`.

### 3.1. Base, pipeline e base de dados

| Issue | Alteração | PR | Merge |
| --- | --- | --- | --- |
| I042 | Solução `Katch.Backend.sln` (API, testes unitários e de integração), `KatchDbContext` com Npgsql, nomes em `snake_case`, tipos enumerados do PostgreSQL, regras de compilação comuns (`Directory.Build.props`) e migration `InitialCreate`. | #3 | `0812b5c` |
| I097 | Pipeline CI base `.github/workflows/ci.yml`: restore, compilação em Release com `-warnaserror`, `dotnet format --verify-no-changes`, testes com recolha de cobertura e publicação dos resultados. | #7 | `133e0b8` |
| I099 | Pasta `database/`: `00-reset-schema.sql` e `01-create-schema.sql` gerados das migrations, `02-seed-reference-data.sql` (30 localidades, 30 competências, 12 benefícios), `03-seed-demo-data.sql` (dados fictícios do P18), `04-verify.sql` e instruções; trabalho `database-scripts` na CI, que executa os scripts duas vezes sobre PostgreSQL 18 e confirma a reposição. | #19 | `9022903` |

### 3.2. Domínio e persistência

| Issue | Alteração | PR | Merge |
| --- | --- | --- | --- |
| I043 | Entidade `AppUser` com as regras de estado da conta (ativa, bloqueada, suspensa) e migration `AddAppUser`. | #4 | `7308f88` |
| I046 | Entidade `Recruiter` e migration `AddRecruiter`. | #5 | `572c814` |
| I044 | Entidades `Candidate`, `Location` e `CandidateLink`, regras da quota de interesses (`ConsumeInterest`, `RestoreQuotaIfDue`) e migration `AddCandidate`. | #6 | `df327bb` |
| I047 | Entidade `Admin` e conta de Administrador inicial (`admin@katch.pt`) criada pela migration `AddAdmin`, com a palavra-passe lida de `Katch__AdminPassword`. | #9 | `c1f7f69` |
| I045 | Entidade `Company` com os estados do registo e migration `AddCompany`. | #10 | `2c9c470` |
| I048 | Entidade `Job` com o filtro global das vagas eliminadas e migration `AddJob`. | #12 | `b58d477` |
| I050 | Entidade `Match` com as transições de estado e migration `AddMatch`. | #14 | `7c6c0bb` |
| I049 | Listas de competências e de benefícios e respetivas associações a Candidatos e vagas; migration `AddSkillsAndBenefits`. | #16 | `3fdd5c5` |
| I098 | Entidades `Message` e `OperationLog` (registo de autoria das operações) e migration `AddMessageAndOperationLog`. | #17 | `ed3d6a3` |

### 3.3. Autenticação, registo e acesso

| Issue | Alteração | PR | Merge |
| --- | --- | --- | --- |
| I051 | Credencial JWT (HS256, segredo em `Jwt__Secret`, validado no arranque), políticas `Candidate`, `Recruiter`, `CandidateOrRecruiter`, `Admin` e `ApprovedCompany`, autenticação exigida por omissão, verificação do estado da conta em cada pedido e formato único dos erros (`ApiErrorDto`). | #11 | `f0d6e84` |
| I052 | `POST /api/auth/candidates` (registo do Candidato) e `GET /api/reference-lists/locations` anónimo. | #13 | `1ebf46f` |
| I053 | `POST /api/auth/recruiters` e registo da Empresa: `POST /api/recruiter/company`, `POST .../resubmission` e `GET .../status`, com `COMPANY_SUBMITTED` no `operation_log`. | #18 | `d103254` |
| I054 | `POST /api/auth/login` (com o cabeçalho `X-Client-App`), `POST /api/auth/logout` e `PUT /api/auth/password`; esquema Bearer e cabeçalho `X-Client-App` no Swagger. Integrado na branch da I052 pelo #15 (`2f831c7`) e na `main` pelo #22. | #15, #22 | `f7b17d7` |

### 3.4. Candidato: perfil, exploração e interesses

| Issue | Alteração | PR | Merge |
| --- | --- | --- | --- |
| I061 | Bloqueio de 24 horas ao esgotar a quota (`QuotaExhaustedException`), `QuotaService` e tarefa periódica `QuotaRestoreWorker` (`BackgroundJobs__IntervalSeconds`, 60 s por omissão, validado no arranque). | #20 | `b3743ea` |
| I057 | Recusa, interesse e quota: `POST /api/candidate/jobs/{jobId}/decline`, `POST .../interest` e `GET /api/candidate/jobs/quota`, com os erros ERR-31 a ERR-33 e repetição em caso de conflito de concorrência. | #21 | `49fef05` |
| I062 | Consulta das competências da lista e gestão das competências do perfil: `POST`/`DELETE /api/candidate/profile/skills` e `POST`/`DELETE .../custom-skills` (opção «Outro»). | #23 | `6975b7e` |
| I113 | Perfil profissional do Candidato (`GET`/`PUT /api/candidate/profile`, `preferences`, `links`, `photo`, `cv`, `GET cv`) e infraestrutura de ficheiros (armazenamento local, validação, URLs, erros ERR-11 a ERR-13, `GET /api/files/{kind}/{fileName}`). | #26 | `19558f1` |
| I059 | Cartão de vaga seguinte (`GET /api/candidate/jobs/next`, filtro do RF014 e distância de Haversine calculados na consulta) e detalhe da vaga (`GET /api/candidate/jobs/{jobId}`). | #35 | `f685f03` |

### 3.5. Empresa e vagas

| Issue | Alteração | PR | Merge |
| --- | --- | --- | --- |
| I055 | Aprovação e recusa do registo das Empresas pelo Administrador: `GET /api/admin/companies/pending`, `GET .../{companyId}/registration`, `POST .../approve` e `POST .../reject` (com motivo). | #25 | `d5c371c` |
| I056 | Gestão das vagas pelo Recrutador da Empresa aprovada: `GET`/`POST /api/recruiter/jobs`, `PUT`, `publish`, `suspend`, `republish`, `close` e `DELETE` (recusado com interesses registados, ERR-38). | #28 | `567c5c5` |
| I114 | Página e dados de registo da Empresa: `GET`/`PUT /api/recruiter/company`, `GET`/`PUT .../page`, `PUT .../logo`, `POST .../photos` (máximo 6, ERR-39) e página vista pelo Candidato (`GET /api/candidate/jobs/companies/{companyId}`). | #29 | `a5fef8a` |
| I105 | Fotografias da vaga: `POST /api/recruiter/jobs/{jobId}/photos` (máximo 5), com atualização atómica; corpo multipart no Swagger. | #30 | `55e59e3` |
| I115 | Encerramento automático das vagas publicadas na data-limite (`JobExpirationWorker`, `JOB_CLOSED` no `operation_log`). | #32 | `fea6b27` |

### 3.6. Avaliação, matches e conversas

| Issue | Alteração | PR | Merge |
| --- | --- | --- | --- |
| I116 | Consulta dos matches (`GET /api/matches`) e conversas (`GET /api/conversations`, `GET`/`POST .../{matchId}/messages`, `POST .../close`), com consulta periódica pelo parâmetro `after`; erros ERR-08, ERR-36 e ERR-37. | #31 | `dab43ff` |
| I060 | Candidatos em espera por vaga (`GET /api/recruiter/jobs/{jobId}/waiting-candidates`), perfil completo (`GET /api/recruiter/interests/{matchId}/profile`, com registo da abertura) e CV (`GET .../cv`). | #34 | `fe3472b` |
| I058 | Decisão do Recrutador: `POST /api/recruiter/interests/{matchId}/accept` e `POST .../reject`, com o erro ERR-34 e o `MatchDto` da aceitação. | #33 | `e3e0fc0` |

### 3.7. Administração

| Issue | Alteração | PR | Merge |
| --- | --- | --- | --- |
| I117 | Administração das contas (`GET /api/admin/accounts`, `GET .../candidates`, `block`, `suspend` do Candidato, `reactivate`) e das Empresas (`suspend`, `reactivate`, lista global e vagas publicadas). | #24 | `2047342` |
| I118 | Painel de indicadores por período (`GET /api/admin/indicators?from=&to=`) e manutenção das listas pré-definidas (`POST`/`PUT /api/reference-lists/skills` e `.../benefits`). | #27 | `6adf101` |

### 3.8. Testes

| Issue | Alteração | PR | Merge |
| --- | --- | --- | --- |
| I064 | Testes de integração do backend (771 nesta versão, sobre SQLite em memória, com a API real em `WebApplicationFactory`). | #36 | `2dcdd75` |
| I063 | Testes unitários do backend (1229 nesta versão), com duplos de teste para os controllers. | #37 | `3f4a070` |

### 3.9. Issues de m3 sem código nesta versão

| Issue | Situação |
| --- | --- |
| I100 a I104 | Concluídas em m3 (estado *Done* no Backlog de Projeto v06). São artefactos de documentação (redução do âmbito, especificação, modelo de dados, modelo de classes e API v02) que esta versão implementa; não alteram o repositório do backend. |
| I065 | Collection Postman da API. Estado *Pendente* no Backlog de Projeto v06; nada integrado na `main` até ao commit da release (secção 5, L-03). |
| I066 | Análise estática com SonarQube e pipeline CI/CD do backend (inclui as antigas I067 e I068). Estado *Pendente* no Backlog de Projeto v06; nada integrado na `main` até ao commit da release (secção 5, L-04). |
| I106, I108, I110, I111, I112 | Artefactos de entrega de m3 (manual e notas de release, CCA e README, RTM, modelos dos frontends, controlo final). Não alteram o repositório do backend. |

## 4. Correções

### 4.1. Correções na `main`

| PR | Data | Problema | Correção |
| --- | --- | --- | --- |
| #38 (`5918eaf`) | 2026-11-02 | Depois da integração da I058 (#33), os testes unitários da `main` não compilavam: o duplo `FakeUserRepository` não implementava `CountActiveCandidatesAtAsync`, acrescentado pela I118. | Implementação do método no duplo de teste (`hotfix/i058-fake-user-repository`). |
| #39 (`621a2c7`) | 2026-11-02 | Depois da integração da I059 (#35), a `main` não compilava (`fileUrlBuilder` inexistente no `JobExplorationService`, após a I114) e a pipeline falhou (execução 41469290309). | Correção do serviço e junção dos testes da I059 e da página da Empresa (`hotfix/i059-job-exploration-tests`). |

### 4.2. Correções feitas durante a revisão das Issues

Defeitos e melhorias apontados pelos Revisores e corrigidos antes da integração (commits `fix` nas branches de cada Issue):

| Issue | Correção |
| --- | --- |
| I043 | Validação dos 255 caracteres; bloqueio e suspensão só a partir de uma conta ativa (D01). |
| I044 | Substituição das hiperligações pela posição e não pelo índice da lista. |
| I048 | Designer da migration `AddJob` atualizado com a entidade `Admin`. |
| I051 | Emissor, destinatário e validade da credencial fixos (M03); a verificação do estado da conta ignora os endpoints anónimos. |
| I055 | Comentários XML corrigidos e repostos depois do merge com a `main`. |
| I056 | Melhorias M01, M02 e M05 da revisão. |
| I057 | O interesse e a quota repetem até ao fim do conflito de concorrência. |
| I058 | Rotas `accept` e `reject` restringidas a GUID (M01). |
| I060 | Restrição `:guid` nos identificadores das rotas (API v02, secção 2.5); resolução dos conflitos com a I056. |
| I062 | Recusa do CHECK da designação convertida em ERR-10; algarismos 0 a 9; `:guid` nas remoções. |
| I098 | `Message.MarkRead` passa a `void`, como no modelo de classes. |
| I105 | Fotografia acrescentada com atualização atómica e movida para o `JobService`. |
| I114 | Corrida entre suspensão e fotografias convertida em ERR-07 e ERR-39 (M01). |
| I115, I117 | Resolução dos conflitos de merge com a I056 e com a I055/I113. |

Não há defeitos conhecidos em aberto nesta versão: a pipeline do commit da release está verde e o ensaio de instalação de raiz (manual, anexo A) não encontrou falhas no código.

## 5. Limitações conhecidas

| ID | Limitação | Origem |
| --- | --- | --- |
| L-01 | **Sem frontends.** A interface web do Recrutador e do Administrador e a aplicação móvel do Candidato não fazem parte desta versão; a API é usada pelo Swagger ou por pedidos HTTP. | Backlog de Projeto: I069 a I080 em m4 (*Pendente*). |
| L-02 | **Sem notificações nem tempo real.** A F011 (notificações na plataforma) e a ligação persistente da F010 foram retiradas do âmbito; não existe a tabela `notification` (a CI confirma a ausência) e as conversas usam consulta periódica (3 em 3 segundos, parâmetro `after`). | Declaração de Âmbito v04; I100 (*Done*). |
| L-03 | **Sem collection Postman.** A coleção de pedidos da API não está incluída nesta versão. | I065 (*Pendente*). |
| L-04 | **Pipeline sem SonarQube, sem limiar de cobertura e sem entrega contínua.** A `ci.yml` desta versão compila com avisos como erros, verifica a formatação, corre os testes (recolhe a cobertura, sem a avaliar) e executa os scripts SQL. Os quality gates QG-04 (cobertura ≥ 70 %) e a parte SonarQube do QG-05 ainda não bloqueiam a pipeline. | I066 (*Pendente*). |
| L-05 | **Elementos fora do âmbito** da Declaração de Âmbito, não implementados por decisão: autenticação multifator, fornecedores externos de identidade, reposição da palavra-passe por correio eletrónico; confirmação do registo por correio ou SMS; validação do NIF junto de entidades oficiais e documentos comprovativos; extração de dados do CV; importação ou publicação em portais externos e renovação automática das vagas; recomendação por afinidade, GPS, distância por percurso e mapas; desbloqueio antecipado da quota; anulação de matches; exportação de indicadores; ficheiros, chamadas e edição de mensagens nas conversas. | Declaração de Âmbito v04, secção 2. |
| L-06 | **Limitações do contrato da API v02:** sem prefixo de versão (`/api/v1`); listas sem paginação nem filtros; a credencial continua válida até expirar (8 horas) depois do fim de sessão; um pedido a cada 3 segundos por cliente com uma conversa aberta. | API v02, secção 11.2 (L-01, L-02, L-03 e L-07). |
| L-07 | **Ambiente de desenvolvimento.** O Swagger só é publicado no ambiente `Development`; a API corre em HTTP local (`localhost:5180`) ou HTTPS com o certificado de desenvolvimento; os ficheiros enviados ficam no sistema de ficheiros local. A configuração de produção e o deployment são de m4. | I085 (m4, *Pendente*); Regulamento da UC, 04.19 (`m4-guia-deployment-v01.pdf`). |
| L-08 | **README do repositório com valores desatualizados:** o exemplo de resultado dos testes indica 109 testes (são 2000), a secção 3.6 ainda diz que a lista de endpoints do Swagger pode estar vazia e a secção 10 refere F001 a F010. O manual de instalação v01 tem os valores certos desta versão. | Verificado na versão entregue (I106). |
| L-09 | **Palavra-passe de demonstração desconhecida.** As 27 contas do `03-seed-demo-data.sql` e o Administrador criado pelo `01-create-schema.sql` usam uma palavra-passe de demonstração guardada só como resumo. O `database/README.md` remete-a para as instruções de 04.13, mas ela não consta do repositório nem desta entrega e o autor dos scripts (I099) confirmou que não a conhece. Contorno testado: o manual (secção 7.5) define uma palavra-passe à escolha, copiando para essas contas o resumo de uma conta registada pela API. Pelo caminho das migrations, o Administrador usa a palavra-passe de `Katch__AdminPassword`. | Confirmado com o Executor da I099 em 2026-11-02; correção da referência no `database/README.md` fica para a versão seguinte. |
| L-10 | **Caminhos longos no Windows.** O clone ou a cópia numa pasta com mais de cerca de 150 caracteres falha com `Filename too long` (o ficheiro mais longo tem 108 caracteres). | Ensaio de instalação (manual, secções 4 e 11). |

## 6. Instruções relevantes

Os passos completos estão no manual de instalação e configuração v01. Os pontos que dependem desta versão são:

### 6.1. Variáveis de ambiente

| Variável | Obrigatória | Uso |
| --- | --- | --- |
| `ConnectionStrings__Katch` | Sim (API e `dotnet ef database update`) | Cadeia de ligação Npgsql: `Host=localhost;Port=<porto>;Database=katch;Username=postgres;Password=<palavra-passe>`. Sem ela, a API não arranca. |
| `Jwt__Secret` | Sim (API) | Segredo HS256 com pelo menos 32 bytes; validado no arranque. |
| `Katch__AdminPassword` | Ao aplicar a migration `AddAdmin` (base nova) e ao gerar o `01-create-schema.sql` | Palavra-passe do Administrador `admin@katch.pt` (P05: 8 ou mais caracteres, com uma letra e um algarismo). |
| `BackgroundJobs__IntervalSeconds` | Não | Intervalo das tarefas periódicas, 1 a 86400 segundos (60 por omissão). |
| `FileStorage__RootPath` | Não | Pasta dos ficheiros enviados (por omissão `%LOCALAPPDATA%\Katch\files`). |

Nenhum valor secreto está em ficheiros versionados: o `appsettings.json` só configura os registos de atividade e o `.gitignore` exclui `.env` e `appsettings.Development.json`.

### 6.2. Migrations e scripts

Esta versão tem 10 migrations, aplicadas por esta ordem:

1. `20261015163645_InitialCreate` (I042)
2. `20261016184300_AddAppUser` (I043)
3. `20261016184614_AddRecruiter` (I046)
4. `20261016184923_AddCandidate` (I044)
5. `20261021200000_AddAdmin` (I047)
6. `20261022141118_AddCompany` (I045)
7. `20261023003000_AddJob` (I048)
8. `20261023161721_AddMatch` (I050)
9. `20261023165549_AddSkillsAndBenefits` (I049)
10. `20261024180929_AddMessageAndOperationLog` (I098)

- Aplicar: `dotnet tool restore` e `dotnet ef database update --project src/Katch.Backend`, com `ConnectionStrings__Katch` e `Katch__AdminPassword`.
- O modelo não tem alterações pendentes (`dotnet ef migrations has-pending-model-changes`: «No changes have been made to the model since the last migration»).
- `database/01-create-schema.sql` e `database/00-reset-schema.sql` foram gerados das mesmas 10 migrations e registam-nas no `__EFMigrationsHistory`, por isso os dois caminhos (migrations ou scripts) são intercambiáveis. Depois de qualquer migration nova, os dois scripts têm de ser gerados de novo (`database/README.md`).
- As listas pré-definidas (`02-seed-reference-data.sql`) são necessárias para usar a API: sem localidades não há registos.
- Por ser a primeira release, não há atualização a partir de versões anteriores.

### 6.3. Verificação da versão

- `git describe --tags` no clone tem de mostrar `v0.3.0`, e `git log -1 --format=%H` tem de mostrar `3f4a070946035440e8ad9c746b83a7b904478881`.
- `dotnet test`: 1229 testes unitários e 771 de integração, 0 falhas.
- Swagger em `http://localhost:5180/swagger`: 72 operações em 62 caminhos e 13 grupos.

## 7. Conteúdo da cópia arquivada

| Pasta | Conteúdo |
| --- | --- |
| `04.12-backend/m3-v01` | Árvore integral do commit `3f4a070` (418 ficheiros versionados: solução, `src`, `tests`, `database`, `.config`, `.github`, ficheiros de configuração e README), extraída da tag `v0.3.0` com `git archive`. Sem `bin`, `obj`, `TestResults`, `.vs`, ficheiros locais (`LOCAL-NOTAS.md`) nem segredos. |
| `04.13-scripts-de-base-de-dados/m3-v01` | Os cinco scripts de `database/` (`00-reset-schema.sql` a `04-verify.sql`) e as instruções de execução (`README.md`), iguais aos da cópia do backend. |

A solução foi compilada e os 2000 testes foram executados a partir de uma cópia byte a byte de `04.12-backend/m3-v01` (registo na evidência da I106).
