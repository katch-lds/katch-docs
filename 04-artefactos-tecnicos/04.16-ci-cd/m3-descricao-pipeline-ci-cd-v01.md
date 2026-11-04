# Descrição da Pipeline CI/CD do Backend — Katch

**Unidade curricular:** Laboratório de Desenvolvimento de Software — LDS  
**Milestone:** `m3`  
**Ficheiro:** `m3-descricao-pipeline-ci-cd-v01.md`  
**Pasta de arquivo:** `04-artefactos-tecnicos/04.16-ci-cd`

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
| Issue | `I066` — Configurar a análise estática (SonarQube) e a pipeline CI/CD do backend e documentar a pipeline (Sprint 06) |
| Executor / Revisor / Auditor | João Borguem / Miguel Santos / Roberto Baptista |
| Documentos de origem | `m2-convencoes-codigo-quality-gates-v01.md` (`I008`; secções 5 a 7, QG-01 a QG-05), Regulamento Interno do Grupo v01 (secções 10, 10.4, 11, 11.3, 11.4 e 11.6), pipeline base da `I097` |
| Ficheiro descrito | `.github/workflows/ci.yml` do repositório `katch-lds/katch-backend`, no commit `8d3c91f` (ramo da `I066`, PR #40) |
| Cópia arquivada | `04-artefactos-tecnicos/04.16-ci-cd/m3-v01` |

### 1.1. Histórico de versões

| Versão | Data | Descrição das alterações | Issue |
| --- | --- | --- | --- |
| v01 | 2026-11-03 | Criação do documento: gatilhos, ambientes, etapas, testes, análise estática, quality gates QG-01 a QG-05, artefactos, bloqueios, tratamento de falhas e credenciais da pipeline do backend. | `I066` |

---

## 2. Objeto

A pipeline do backend verifica automaticamente cada Merge Request para a `main` do `katch-backend` contra os quality gates do grupo aplicáveis ao backend (QG-01 a QG-05; convenções de código e quality gates, secção 7) e impede a integração quando algum falha. Completa a pipeline base da `I097`, que já fazia o build, a formatação, os testes e a verificação dos scripts da base de dados, com o limiar de cobertura (QG-04) e a análise SonarQube com Quality Gate (QG-05).

A pipeline é de integração contínua (CI). A entrega contínua (CD) do backend não faz parte de `m3`: não há publicação automática num servidor. Os artefactos de cada execução (resultados dos testes, relatório de cobertura e relatório SonarQube) ficam disponíveis na própria execução do GitHub Actions (secção 8).

---

## 3. Gatilhos

| Evento | Quando | Objetivo |
| --- | --- | --- |
| `pull_request` para a `main` | Ao abrir, atualizar ou reabrir um Merge Request | Verificar o resultado da integração antes do merge (RI, secção 10.4). É este o evento que bloqueia a integração. |
| `push` na `main` | Depois de cada merge | Confirmar que a `main` continua a cumprir os quality gates. |
| `workflow_dispatch` | Manualmente, em *Actions* | Repetir a verificação sem alterar o código. |

Execuções concorrentes do mesmo ramo são canceladas pela mais recente (`concurrency`), para não verificar versões ultrapassadas. A pipeline tem só permissão de leitura do repositório (`permissions: contents: read`).

---

## 4. Ambientes

| Job | Ambiente | Serviços | Ferramentas |
| --- | --- | --- | --- |
| `build-and-test` | Runner `ubuntu-latest` do GitHub Actions | — | SDK do .NET pela versão do `global.json` (10.0.401 ou superior), ferramentas locais do `.config/dotnet-tools.json`: `dotnet-reportgenerator-globaltool` 5.5.11 |
| `static-analysis` | Runner `ubuntu-latest` | Contentor `sonarqube:26.10.0.133078-community` (SonarQube Community Build), na porta 9000 | SDK do .NET, Java 21 (Temurin), `dotnet-sonarscanner` 11.3.0 |
| `database-scripts` | Runner `ubuntu-latest` | Contentor `postgres:18` | `psql` |

A instância SonarQube é self-hosted pelo grupo e efémera: é criada no início do job `static-analysis` e apagada no fim. É configurada em cada execução pelo script `scripts/sonarqube/configure.sh` (secção 6.2), o que torna a configuração reproduzível e versionada com o código. A mesma imagem pode ser usada localmente com `scripts/sonarqube/docker-compose.yml`, para consultar o painel.

Os testes de integração usam SQLite em memória (`WebApplicationFactory`) e não precisam de base de dados externa.

---

## 5. Etapas

### 5.1. Job `build-and-test` (QG-01 a QG-04)

| N.º | Passo | Comando | Quality gate |
| --- | --- | --- | --- |
| 1 | Obter o código | `actions/checkout@v4` | — |
| 2 | Instalar o .NET | `actions/setup-dotnet@v4` com o `global.json` | — |
| 3 | Restore | `dotnet restore` e `dotnet tool restore` | — |
| 4 | QG-01 Build (analisadores como erro) | `dotnet build --no-restore --configuration Release -warnaserror` | QG-01; QG-05 (Roslyn Analyzers e StyleCop) |
| 5 | QG-01 Verificar formatação | `dotnet format --verify-no-changes --no-restore` | QG-01 (convenções de código, RI 11.3) |
| 6 | QG-02 Testes unitários | `dotnet test tests/Katch.Backend.UnitTests` com o logger `trx` (`unitarios.trx`) e a recolha de cobertura | QG-02 |
| 7 | QG-03 Testes de integração | `dotnet test tests/Katch.Backend.IntegrationTests` com o logger `trx` (`integracao.trx`) e a recolha de cobertura | QG-03 |
| 8 | QG-04 Relatório de cobertura | `reportgenerator` sobre os relatórios Cobertura dos dois projetos de testes, com os filtros do âmbito do QG-04 | QG-04 |
| 9 | QG-04 Cobertura de linhas ≥ 70% | Lê `linecoverage` do `Summary.json` e falha se for inferior a `COVERAGE_THRESHOLD` (70) | QG-04 |
| 10 | Publicar resultados dos testes | Artefacto `test-results` (sempre, mesmo com falhas) | — |
| 11 | Publicar relatório de cobertura | Artefacto `coverage-report` (sempre) | — |

### 5.2. Job `static-analysis` (QG-05)

| N.º | Passo | Comando | Quality gate |
| --- | --- | --- | --- |
| 1 | Obter o código | `actions/checkout@v4` com o histórico completo (`fetch-depth: 0`), para a atribuição das linhas (SCM) | — |
| 2 | Instalar o .NET e o Java | `actions/setup-dotnet@v4` e `actions/setup-java@v4` (Temurin 21) | — |
| 3 | Restore | `dotnet restore` e `dotnet tool restore` | — |
| 4 | Configurar a instância SonarQube | `bash scripts/sonarqube/configure.sh` (secção 6.2) | — |
| 5 | QG-05 Iniciar a análise | `dotnet-sonarscanner begin` com a chave `katch-backend`, a versão (commit abreviado), os relatórios de cobertura (OpenCover) e de testes (`trx`), as exclusões de cobertura do âmbito do QG-04, os falsos positivos justificados (secção 6.4) e `sonar.qualitygate.wait=true` | QG-05 |
| 6 | QG-05 Build analisado | `dotnet build --no-restore --configuration Release` | QG-05 |
| 7 | QG-05 Testes para a cobertura importada | `dotnet test` dos dois projetos, com cobertura OpenCover e `trx` | QG-05 |
| 8 | QG-05 Concluir a análise e verificar o Quality Gate | `dotnet-sonarscanner end`: envia a análise, espera pelo processamento e falha se o Quality Gate falhar | QG-05 |
| 9 | Exportar o relatório SonarQube | `bash scripts/sonarqube/export-report.sh` (sempre que houver token): Quality Gate, medidas, Quality Profiles, problemas e hotspots em JSON e um resumo em Markdown | — |
| 10 | Publicar o relatório SonarQube | Artefacto `sonarqube-report` (sempre) | — |

### 5.3. Job `database-scripts`

Inalterado desde a `I099`: cria o esquema e carrega os dados iniciais duas vezes (idempotência), verifica as 17 tabelas, a ausência da tabela `notification` e os volumes do P18, repõe a base e cria-a de novo. Não é um quality gate do Plano de Qualidade, mas é um check obrigatório do Merge Request, porque os scripts da base de dados fazem parte da entrega (CCA `OF-M3-003`).

### 5.4. Dependências entre jobs

Os três jobs correm em paralelo e são independentes: um job que falha não impede os outros de correr, para que cada execução mostre todos os quality gates que falham. A integração exige os três com sucesso (secção 7).

---

## 6. Testes, cobertura e análise estática

### 6.1. Testes e cobertura (QG-02 a QG-04)

Os testes unitários e de integração são xUnit v3 e correm em passos separados, cada um com o seu relatório `trx`, para que o QG-02 e o QG-03 se distingam na execução. A cobertura é recolhida pelo coverlet (`coverlet.collector`), com a configuração de `coverlet.runsettings`: formatos Cobertura (para o reportgenerator) e OpenCover (para o SonarQube), sem código gerado nem propriedades automáticas.

O âmbito do QG-04 é o da secção 6 das convenções de código: inclui `Controllers`, `Services`, `Repositories` e `Domain` e exclui as migrations, o `Program.cs` e os DTOs. É aplicado no reportgenerator (`COVERAGE_FILE_FILTERS`) e nas exclusões de cobertura do SonarQube (`sonar.coverage.exclusions`), para que as duas ferramentas meçam o mesmo. O limiar é de 70% de cobertura de linhas (`COVERAGE_THRESHOLD`).

### 6.2. Configuração do SonarQube (`scripts/sonarqube/configure.sh`)

1. Espera que a instância responda `UP` em `/api/system/status` (até 7,5 minutos).
2. Substitui a palavra-passe inicial do administrador pela do secret `SONAR_ADMIN_PASSWORD`.
3. Cria o Quality Gate «Katch QG-05», retira as condições de código novo que o SonarQube acrescenta por omissão, deixa só as condições do grupo e define-o como predefinido.
4. Define como predefinido de C# o Quality Profile «Sonar way», se existir com esse nome, e confirma que o predefinido pertence à família «Sonar way». Na versão 26.10 da Community Build, o perfil base de C# chama-se «Sonar way core».
5. Gera um token, mascarado nos logs, e guarda-o em `SONAR_TOKEN` para os passos seguintes do job.

O script é idempotente e pode ser usado na instância local, com `SONAR_ADMIN_PASSWORD` definida e o token guardado em `.sonar-token`.

### 6.3. Quality Gate «Katch QG-05»

As condições aplicam-se ao código total, e não só ao código novo: a instância é nova em cada execução e, numa primeira análise, as condições de código novo do «Sonar way» não se aplicariam. As classificações são as do modo MQR (Multi-Quality Rule), o predefinido da instância.

| Métrica | Condição de falha | Significado |
| --- | --- | --- |
| `software_quality_reliability_rating` | Pior do que B | Algum problema de fiabilidade de impacto médio ou superior |
| `software_quality_security_rating` | Pior do que A | Algum problema de segurança de impacto baixo ou superior |
| `software_quality_maintainability_rating` | Pior do que A | Rácio de dívida técnica acima de 5% |
| `duplicated_lines_density` | Acima de 3% | Linhas duplicadas acima de 3% |

A cobertura não é uma condição do Quality Gate, porque é verificada pelo QG-04 no job `build-and-test`; assim, cada causa de bloqueio aparece num só quality gate.

### 6.4. Falsos positivos

A instância é efémera, por isso os falsos positivos não podem ser marcados no painel. São registados no `ci.yml`, em `sonar.issue.ignore.multicriteria`, por regra e por ficheiro concreto, e justificados no relatório de análise estática (`m3-relatorio-analise-estatica-backend-v01.pdf`):

| Regra | Ficheiros | Justificação resumida |
| --- | --- | --- |
| S6964 (propriedade de tipo valor em pedido) | `JobRequest`, `UpdateCandidateProfileRequest`, `CompanyRegistrationRequest`, `CreateRecruiterAccountRequest`, `RegisterCandidateRequest` | O valor por omissão de um campo em falta é tratado na validação (ERR-10, «O campo é obrigatório.» e `acceptTerms`). |
| S6932 (ler o formulário em vez de `IFormFile`) | `CandidateProfileController`, `CompanyController`, `JobsController` | O `UploadRequestFilterAttribute` verifica o tipo e a dimensão do pedido antes do model binding (ERR-13 e ERR-12, API v02, secção 2.6); o ficheiro é validado pelo `FileValidator`. |
| S2068 (credencial no código) | `FieldReasons` | São mensagens de erro com a palavra «palavra-passe», não credenciais. |

---

## 7. Quality gates e bloqueios

| Quality gate | Job e passo | Condição de bloqueio | Evidência na execução |
| --- | --- | --- | --- |
| QG-01 Construção | `build-and-test`, «QG-01 Build» e «QG-01 Verificar formatação» | Erro de compilação, aviso de analisador (Roslyn ou StyleCop) ou formatação por corrigir | Log do passo |
| QG-02 Testes unitários | `build-and-test`, «QG-02 Testes unitários» | Um teste unitário falha | Log do passo e `unitarios.trx` (artefacto `test-results`) |
| QG-03 Testes de integração | `build-and-test`, «QG-03 Testes de integração» | Um teste de integração falha | Log do passo e `integracao.trx` |
| QG-04 Cobertura | `build-and-test`, «QG-04 Cobertura de linhas ≥ 70%» | Cobertura de linhas abaixo de 70% no âmbito do QG-04 | Anotação «QG-04 cobertura insuficiente», resumo da execução e artefacto `coverage-report` |
| QG-05 Linter e análise estática | `build-and-test`, «QG-01 Build» (Roslyn e StyleCop como erro); `static-analysis`, «QG-05 Concluir a análise e verificar o Quality Gate» | Ocorrência de severidade Error nos analisadores, ou Quality Gate «Katch QG-05» falhado | «QUALITY GATE STATUS: FAILED» no log, resumo da execução e artefacto `sonarqube-report` |

**Bloqueio da integração.** A regra de proteção da `main` exige os checks `build-and-test`, `static-analysis` e `database-scripts` com sucesso antes do merge; a aprovação pelo Revisor e o merge por Merge Commit seguem o RI (secção 10). Com um check falhado, o GitHub mostra *Merging is blocked* e o botão de merge fica indisponível. Um passo que falha termina o seu job com erro; os passos seguintes desse job não correm, exceto os de publicação de artefactos (`if: always()`).

---

## 8. Artefactos

| Artefacto | Job | Conteúdo | Uso |
| --- | --- | --- | --- |
| `test-results` | `build-and-test` | `unitarios.trx`, `integracao.trx` e os relatórios de cobertura de cada projeto | Diagnóstico de testes falhados |
| `coverage-report` | `build-and-test` | Relatório HTML, `Summary.json`, `Cobertura.xml` e `SummaryGithub.md` (também no resumo da execução) | Verificação do QG-04 |
| `sonarqube-report` | `static-analysis` | `quality-gate.json`, `medidas.json`, `quality-profiles.json`, `problemas.json`, `hotspots.json`, `instancia.json` e `resumo.md` (também no resumo da execução) | Registo da análise da instância efémera; base do relatório de análise estática |

Os artefactos ficam guardados na execução do GitHub Actions durante o período de retenção do repositório (90 dias por omissão).

---

## 9. Tratamento de falhas

| Situação | O que acontece | O que fazer |
| --- | --- | --- |
| Quality gate falhado | O job termina com erro; o Merge Request fica bloqueado | Corrigir no ramo e fazer push: a pipeline corre de novo automaticamente. |
| Quality Gate do SonarQube falhado | O passo «QG-05 Concluir a análise» termina com erro | Consultar o resumo da execução ou o `problemas.json` do artefacto; corrigir, ou justificar um falso positivo com nova entrada em `sonar.issue.ignore.multicriteria`, aprovada na revisão. |
| A instância SonarQube não arranca | O passo «Configurar a instância SonarQube» falha ao fim de 7,5 minutos | Repetir a execução (*Re-run jobs*); se repetir, verificar a imagem e o estado do GitHub Actions. |
| Secret em falta ou fraco | O script termina com «Defina SONAR_ADMIN_PASSWORD» ou o SonarQube recusa a palavra-passe | Pedir ao administrador do repositório que crie ou corrija o secret (secção 10). |
| Falha transitória (rede, runner) | Um passo falha sem relação com o código | Repetir a execução; o histórico fica registado no Merge Request. |

Uma falha nunca é contornada desativando um quality gate ou alterando um limiar no Merge Request: qualquer alteração a ferramentas, regras, exclusões ou limiares exige uma nova versão do Regulamento Interno e das convenções de código (convenções, secção 2).

---

## 10. Credenciais

| Credencial | Onde está | Uso |
| --- | --- | --- |
| `SONAR_ADMIN_PASSWORD` | Secret do repositório `katch-backend` (*Settings → Secrets and variables → Actions*), gerido pelo administrador do repositório | Substituir a palavra-passe inicial do administrador da instância efémera; mascarado nos logs |
| Token SonarQube | Gerado em cada execução pelo `configure.sh`, guardado só no ambiente do job (`SONAR_TOKEN`) e mascarado | Análise e exportação do relatório; deixa de existir com a instância |
| Base de dados do job `database-scripts` | Variáveis do próprio job (`postgres`/`postgres`) | Contentor temporário do runner, sem dados reais nem acesso externo |

Nenhuma credencial está no repositório nem nos documentos (RI, secção 14.2). O secret deve ter pelo menos 12 caracteres, com maiúscula, minúscula, algarismo e símbolo, por exigência do SonarQube.

---

## 11. Ficheiros

| Ficheiro no `katch-backend` | Conteúdo |
| --- | --- |
| `.github/workflows/ci.yml` | Pipeline |
| `scripts/sonarqube/configure.sh` | Configuração da instância SonarQube |
| `scripts/sonarqube/export-report.sh` | Exportação do relatório SonarQube |
| `scripts/sonarqube/docker-compose.yml` | Instância SonarQube local |
| `coverlet.runsettings` | Configuração da recolha de cobertura |
| `.config/dotnet-tools.json` | Ferramentas locais (versões fixas) |
| `Directory.Build.props`, `.editorconfig`, `stylecop.json` | Analisadores Roslyn e StyleCop como erro e convenções de código (desde a `I042` e a `I097`) |

Cópia arquivada destes ficheiros, tal como estão no commit `8d3c91f`, diretamente na pasta `04-artefactos-tecnicos/04.16-ci-cd/m3-v01`: `ci.yml`, `configure.sh`, `export-report.sh`, `docker-compose.yml`, `coverlet.runsettings` e `dotnet-tools.json`. Os ficheiros de configuração dos analisadores (`Directory.Build.props`, `.editorconfig` e `stylecop.json`) não mudaram nesta Issue e não são copiados. As evidências das execuções estão em `m3-relatorio-evidencias-pipeline-ci-cd-v01.md`.
