# Correspondência entre os endpoints do Swagger e os pedidos da collection Postman — I065

| Campo | Informação |
| --- | --- |
| Grupo | G11 |
| Sprint / Issue | S06 / `I065` — Criar a collection Postman da API |
| Collection | `m3-collection-postman-v01.json` (pasta `03-artefactos-de-qualidade-de-produto/03.01-backend/03.01.05-postman`) |
| Ambiente | `m3-ambiente-postman-local-v01.json` |
| Backend | `katch-backend`, `main` no commit `3f4a070` (2026-11-02 15:30), com as Issues I052 a I062, I105 e I113 a I118 integradas |
| Origem dos endpoints | Swagger gerado a partir dos controllers (`[Route]` e `[Http*]` de `src/Katch.Backend/Controllers`), comparado com a secção 5 da documentação da API v02 |
| Resultado | 72 endpoints no Swagger, 72 na documentação da API v02 e 72 cobertos pela collection, com 107 pedidos (incluindo 13 pedidos de erro) |

## Tabela

| N.º API v02 | Método | Rota no Swagger | Operação | Pedidos da collection (pasta) |
| --- | --- | --- | --- | --- |
| 6.1.1 | `POST` | `/api/auth/candidates` | `AuthController.RegisterCandidate` | 6.1.1 Registar-se como Candidato (Candidato 1) (02)<br>6.1.1 Registar-se como Candidato (Candidato 2) (02) |
| 6.1.2 | `POST` | `/api/auth/recruiters` | `AuthController.CreateRecruiterAccount` | 6.1.2 Criar conta de Recrutador (02) |
| 6.1.3 | `POST` | `/api/auth/login` | `AuthController.Login` | 6.1.3 Iniciar sessão — Administrador (web) (01)<br>6.1.3 Iniciar sessão — Candidato 1 (mobile) (02)<br>6.1.3 Iniciar sessão — Recrutador (web) (02)<br>6.1.3 Iniciar sessão com a nova palavra-passe — Candidato 1 (mobile) (02)<br>6.1.3 Erro: palavra-passe errada (ERR-02) (02)<br>6.1.3 Erro: Candidato pelo ponto de acesso web (ERR-05) (02)<br>6.1.3 Iniciar sessão — Candidato de demonstração (mobile) (13)<br>6.1.3 Iniciar sessão — Recrutador de demonstração (web) (13) |
| 6.1.4 | `POST` | `/api/auth/logout` | `AuthController.Logout` | 6.1.4 Terminar sessão (Candidato 1) (12) |
| 6.1.5 | `PUT` | `/api/auth/password` | `AuthController.ChangePassword` | 6.1.5 Alterar a própria palavra-passe — Candidato 1 (02) |
| 6.2.1 | `GET` | `/api/candidate/profile` | `CandidateProfileController.Get` | 6.2.1 Consultar o perfil profissional (06)<br>6.2.1 Erro: pedido de conta bloqueada (ERR-03) (10) |
| 6.2.2 | `PUT` | `/api/candidate/profile` | `CandidateProfileController.Update` | 6.2.2 Gravar os dados do perfil profissional (06) |
| 6.2.3 | `PUT` | `/api/candidate/profile/preferences` | `CandidateProfileController.UpdatePreferences` | 6.2.3 Definir as preferências de procura (06) |
| 6.2.4 | `POST` | `/api/candidate/profile/skills` | `CandidateProfileController.AddSkill` | 6.2.4 Acrescentar uma competência da lista (1) (06)<br>6.2.4 Acrescentar uma competência da lista (2) (06) |
| 6.2.5 | `DELETE` | `/api/candidate/profile/skills/{skillId:guid}` | `CandidateProfileController.RemoveSkill` | 6.2.5 Retirar uma competência da lista (2) (06) |
| 6.2.6 | `PUT` | `/api/candidate/profile/links` | `CandidateProfileController.SetLinks` | 6.2.6 Indicar as hiperligações profissionais (06) |
| 6.2.7 | `PUT` | `/api/candidate/profile/photo` | `CandidateProfileController.UploadPhoto` | 6.2.7 Carregar a fotografia de perfil (06) |
| 6.2.8 | `PUT` | `/api/candidate/profile/cv` | `CandidateProfileController.UploadCv` | 6.2.8 Enviar o curriculum vitae (06) |
| 6.2.9 | `GET` | `/api/candidate/profile/cv` | `CandidateProfileController.GetCv` | 6.2.9 Obter o próprio curriculum vitae (06) |
| 6.2.10 | `POST` | `/api/candidate/profile/custom-skills` | `CandidateProfileController.AddCustomSkill` | 6.2.10 Acrescentar uma competência «Outro» (06) |
| 6.2.11 | `DELETE` | `/api/candidate/profile/custom-skills/{customSkillId:guid}` | `CandidateProfileController.RemoveCustomSkill` | 6.2.11 Retirar uma competência «Outro» (06) |
| 6.3.1 | `GET` | `/api/candidate/jobs/next` | `JobExplorationController.GetNextCard` | 6.3.1 Obter o cartão de vaga seguinte (07) |
| 6.3.2 | `GET` | `/api/candidate/jobs/{jobId:guid}` | `JobExplorationController.GetDetail` | 6.3.2 Consultar o detalhe de uma vaga (A) (07) |
| 6.3.3 | `GET` | `/api/candidate/jobs/companies/{companyId:guid}` | `JobExplorationController.GetCompanyPage` | 6.3.3 Consultar a página de apresentação da Empresa (07) |
| 6.3.4 | `POST` | `/api/candidate/jobs/{jobId}/decline` | `JobExplorationController.Decline` | 6.3.4 Recusar uma vaga (B) (07) |
| 6.3.5 | `POST` | `/api/candidate/jobs/{jobId}/interest` | `JobExplorationController.ExpressInterest` | 6.3.5 Manifestar interesse numa vaga (A) — Candidato 1 (07)<br>6.3.5 Manifestar interesse numa vaga (A) — Candidato 2 (07)<br>6.3.5 Erro: segundo interesse na mesma vaga (ERR-32) (07) |
| 6.3.6 | `GET` | `/api/candidate/jobs/quota` | `JobExplorationController.GetQuota` | 6.3.6 Consultar os interesses disponíveis (07) |
| 6.4.1 | `POST` | `/api/recruiter/company` | `CompanyController.Register` | 6.4.1 Registar a Empresa (03) |
| 6.4.2 | `POST` | `/api/recruiter/company/resubmission` | `CompanyController.Resubmit` | 6.4.2 Corrigir e submeter novamente o pedido (03) |
| 6.4.3 | `GET` | `/api/recruiter/company/status` | `CompanyController.GetStatus` | 6.4.3 Consultar o estado do pedido (pendente) (03)<br>6.4.3 Consultar o estado do pedido (recusado) (03) |
| 6.4.4 | `PUT` | `/api/recruiter/company` | `CompanyController.UpdateRegistration` | 6.4.4 Alterar os dados de registo da Empresa aprovada (04) |
| 6.4.5 | `PUT` | `/api/recruiter/company/page` | `CompanyController.UpdatePage` | 6.4.5 Gravar a página de apresentação (04) |
| 6.4.6 | `PUT` | `/api/recruiter/company/logo` | `CompanyController.UploadLogo` | 6.4.6 Carregar o logótipo (04) |
| 6.4.7 | `POST` | `/api/recruiter/company/photos` | `CompanyController.AddGalleryPhoto` | 6.4.7 Acrescentar uma fotografia à galeria (04) |
| 6.4.8 | `GET` | `/api/recruiter/company` | `CompanyController.GetRegistration` | 6.4.8 Consultar os dados de registo da própria Empresa (03) |
| 6.4.9 | `GET` | `/api/recruiter/company/page` | `CompanyController.GetPage` | 6.4.9 Consultar a página de apresentação da própria Empresa (04) |
| 6.5.1 | `GET` | `/api/recruiter/jobs` | `JobsController.List` | 6.5.1 Erro: vagas com a Empresa pendente (ERR-07) (03)<br>6.5.1 Listar as vagas da Empresa (05)<br>6.5.1 Erro: vagas com a Empresa suspensa (ERR-07) (11)<br>6.5.1 Listar as vagas (Recrutador de demonstração) (13) |
| 6.5.2 | `POST` | `/api/recruiter/jobs` | `JobsController.Create` | 6.5.2 Criar uma vaga (Programador Postman A) (05)<br>6.5.2 Criar uma vaga (Analista Postman B) (05)<br>6.5.2 Criar uma vaga (Vaga Postman C a eliminar) (05) |
| 6.5.3 | `PUT` | `/api/recruiter/jobs/{jobId:guid}` | `JobsController.Update` | 6.5.3 Alterar uma vaga (A) (05) |
| 6.5.4 | `POST` | `/api/recruiter/jobs/{jobId:guid}/publish` | `JobsController.Publish` | 6.5.4 Publicar uma vaga (A) (05)<br>6.5.4 Publicar uma vaga (B) (05) |
| 6.5.5 | `POST` | `/api/recruiter/jobs/{jobId:guid}/suspend` | `JobsController.Suspend` | 6.5.5 Suspender uma vaga (A) (05) |
| 6.5.6 | `POST` | `/api/recruiter/jobs/{jobId:guid}/republish` | `JobsController.Republish` | 6.5.6 Voltar a publicar uma vaga suspensa (A) (05) |
| 6.5.7 | `POST` | `/api/recruiter/jobs/{jobId:guid}/close` | `JobsController.Close` | 6.5.7 Encerrar uma vaga (A) (12) |
| 6.5.8 | `DELETE` | `/api/recruiter/jobs/{jobId:guid}` | `JobsController.Delete` | 6.5.8 Eliminar uma vaga sem interesses (C) (05) |
| 6.5.9 | `POST` | `/api/recruiter/jobs/{jobId:guid}/photos` | `JobsController.AddPhoto` | 6.5.9 Acrescentar uma fotografia à vaga (A) (05) |
| 6.6.1 | `GET` | `/api/recruiter/jobs/{jobId:guid}/waiting-candidates` | `CandidateEvaluationController.ListWaiting` | 6.6.1 Consultar os candidatos em espera (A) (08) |
| 6.6.2 | `GET` | `/api/recruiter/interests/{matchId:guid}/profile` | `CandidateEvaluationController.OpenProfile` | 6.6.2 Abrir o perfil completo do Candidato (2) (08)<br>6.6.2 Abrir o perfil completo do Candidato (1) (08) |
| 6.6.3 | `GET` | `/api/recruiter/interests/{matchId:guid}/cv` | `CandidateEvaluationController.GetCv` | 6.6.3 Obter o curriculum vitae (1) (08) |
| 6.6.4 | `POST` | `/api/recruiter/interests/{matchId:guid}/accept` | `CandidateEvaluationController.Accept` | 6.6.4 Aceitar o Candidato (1) (08)<br>6.6.4 Erro: decidir de novo (ERR-35) (08) |
| 6.6.5 | `POST` | `/api/recruiter/interests/{matchId:guid}/reject` | `CandidateEvaluationController.Reject` | 6.6.5 Erro: recusar sem abrir o perfil (ERR-34) (08)<br>6.6.5 Recusar o Candidato (2) (08) |
| 6.7.1 | `GET` | `/api/matches` | `MatchesController.List` | 6.7.1 Consultar os matches (Candidato 1) (09)<br>6.7.1 Consultar os matches (Recrutador) (09)<br>6.7.1 Consultar os matches (Candidato de demonstração) (13) |
| 6.8.1 | `GET` | `/api/conversations` | `ConversationsController.List` | 6.8.1 Consultar a lista de conversas (Candidato 1) (09)<br>6.8.1 Consultar a lista de conversas (Recrutador) (09)<br>6.8.1 Erro: Administrador nas conversas (ERR-06) (11)<br>6.8.1 Consultar a lista de conversas (Recrutador de demonstração) (13) |
| 6.8.2 | `GET` | `/api/conversations/{matchId:guid}/messages` | `ConversationsController.Open` | 6.8.2 Abrir uma conversa (Candidato 1) (09)<br>6.8.2 Abrir uma conversa com after (Recrutador) (09) |
| 6.8.3 | `POST` | `/api/conversations/{matchId:guid}/messages` | `ConversationsController.Send` | 6.8.3 Enviar uma mensagem (Recrutador) (09)<br>6.8.3 Enviar uma mensagem (Candidato 1) (09)<br>6.8.3 Erro: mensagem numa conversa encerrada (ERR-36) (09) |
| 6.8.4 | `POST` | `/api/conversations/{matchId:guid}/close` | `ConversationsController.Close` | 6.8.4 Encerrar uma conversa (Candidato 1) (09) |
| 6.9.1 | `GET` | `/api/files/{kind}/{fileName}` | `FilesController.Get` | 6.9.1 Obter o logótipo da Empresa (04)<br>6.9.1 Obter a própria fotografia de perfil (06) |
| 6.10.1 | `GET` | `/api/admin/accounts` | `AdminAccountsController.ListAccounts` | 6.10.1 Consultar as contas de utilizador (10) |
| 6.10.2 | `GET` | `/api/admin/accounts/candidates` | `AdminAccountsController.ListCandidates` | 6.10.2 Consultar a lista de Candidatos (10) |
| 6.10.3 | `POST` | `/api/admin/accounts/{userId}/block` | `AdminAccountsController.Block` | 6.10.3 Bloquear uma conta (Candidato 2) (10) |
| 6.10.4 | `POST` | `/api/admin/accounts/candidates/{candidateId}/suspend` | `AdminAccountsController.SuspendCandidate` | 6.10.4 Suspender a conta de um Candidato (2) (10) |
| 6.10.5 | `POST` | `/api/admin/accounts/{userId}/reactivate` | `AdminAccountsController.Reactivate` | 6.10.5 Reativar uma conta bloqueada (Candidato 2) (10)<br>6.10.5 Reativar uma conta suspensa (Candidato 2) (10) |
| 6.11.1 | `GET` | `/api/admin/companies/pending` | `AdminCompaniesController.ListPending` | 6.11.1 Consultar as Empresas pendentes (03) |
| 6.11.2 | `GET` | `/api/admin/companies/{companyId}/registration` | `AdminCompaniesController.GetSubmittedData` | 6.11.2 Consultar os dados submetidos (03) |
| 6.11.3 | `POST` | `/api/admin/companies/{companyId}/approve` | `AdminCompaniesController.Approve` | 6.11.3 Aprovar o registo (03)<br>6.11.3 Erro: aprovar de novo (ERR-30) (03) |
| 6.11.4 | `POST` | `/api/admin/companies/{companyId}/reject` | `AdminCompaniesController.Reject` | 6.11.4 Recusar o registo com motivo (03) |
| 6.11.5 | `POST` | `/api/admin/companies/{companyId}/suspend` | `AdminCompaniesController.Suspend` | 6.11.5 Suspender uma Empresa aprovada (11) |
| 6.11.6 | `POST` | `/api/admin/companies/{companyId}/reactivate` | `AdminCompaniesController.Reactivate` | 6.11.6 Reativar uma Empresa suspensa (11) |
| 6.11.7 | `GET` | `/api/admin/companies` | `AdminCompaniesController.ListCompanies` | 6.11.7 Consultar a lista global de Empresas (11) |
| 6.11.8 | `GET` | `/api/admin/companies/published-jobs` | `AdminCompaniesController.ListPublishedJobs` | 6.11.8 Consultar a lista global de vagas publicadas (11) |
| 6.12.1 | `GET` | `/api/admin/indicators` | `AdminIndicatorsController.Get` | 6.12.1 Consultar o painel de indicadores por período (11)<br>6.12.1 Erro: data final no futuro (ERR-10) (11) |
| 6.13.1 | `GET` | `/api/reference-lists/locations` | `ReferenceListsController.ListLocations` | 6.13.1 Consultar as localidades (anónimo) (01) |
| 6.13.2 | `GET` | `/api/reference-lists/skills` | `ReferenceListsController.ListSkills` | 6.13.2 Consultar as competências (01) |
| 6.13.3 | `GET` | `/api/reference-lists/benefits` | `ReferenceListsController.ListBenefits` | 6.13.3 Consultar os benefícios (01) |
| 6.13.4 | `POST` | `/api/reference-lists/skills` | `ReferenceListsController.AddSkill` | 6.13.4 Acrescentar uma competência (01)<br>6.13.4 Erro: designação repetida (ERR-10) (01) |
| 6.13.5 | `PUT` | `/api/reference-lists/skills/{skillId:guid}` | `ReferenceListsController.RenameSkill` | 6.13.5 Alterar a designação da competência (01) |
| 6.13.6 | `POST` | `/api/reference-lists/benefits` | `ReferenceListsController.AddBenefit` | 6.13.6 Acrescentar um benefício (01) |
| 6.13.7 | `PUT` | `/api/reference-lists/benefits/{benefitId:guid}` | `ReferenceListsController.RenameBenefit` | 6.13.7 Alterar a designação do benefício (01) |

A pasta indicada entre parênteses é o número da pasta da collection (01 a 13). Os pedidos marcados «Erro» verificam um caso de erro do catálogo (secção 4.3 da API v02) com o código HTTP, a estrutura `ApiErrorDto` e o código do erro.

