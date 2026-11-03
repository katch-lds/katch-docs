newman

Katch API — m3 (I065)

□ 01 Listas pré-definidas
└ 6.13.1 Consultar as localidades (anónimo)
  GET https://localhost:7180/api/reference-lists/locations [200 OK, 2.38kB, 160ms]
  √  Código de estado 200
  √  Estrutura da resposta: ReferenceItemDto[]
  √  Lista com localidades

└ 6.1.3 Iniciar sessão — Administrador (web)
  POST https://localhost:7180/api/auth/login [200 OK, 529B, 223ms]
  √  Código de estado 200
  √  Estrutura da resposta: LoginResponse
  √  Conta de Administrador

└ 6.13.2 Consultar as competências
  GET https://localhost:7180/api/reference-lists/skills [200 OK, 2.33kB, 21ms]
  √  Código de estado 200
  √  Estrutura da resposta: ReferenceItemDto[]
  √  Pelo menos duas competências

└ 6.13.3 Consultar os benefícios
  GET https://localhost:7180/api/reference-lists/benefits [200 OK, 1.25kB, 12ms]
  √  Código de estado 200
  √  Estrutura da resposta: ReferenceItemDto[]
  √  Pelo menos um benefício

└ 6.13.4 Acrescentar uma competência
  POST https://localhost:7180/api/reference-lists/skills [201 Created, 242B, 40ms]
  √  Código de estado 201
  √  Estrutura da resposta: ReferenceItemDto
  √  Designação gravada

└ 6.13.5 Alterar a designação da competência
  PUT https://localhost:7180/api/reference-lists/skills/922adaab-65bd-4393-a1cd-e71170c8fd52 [200 OK, 248B, 28ms]
  √  Código de estado 200
  √  Estrutura da resposta: ReferenceItemDto
  √  Mesmo identificador e nova designação

└ 6.13.6 Acrescentar um benefício
  POST https://localhost:7180/api/reference-lists/benefits [201 Created, 240B, 42ms]
  √  Código de estado 201
  √  Estrutura da resposta: ReferenceItemDto

└ 6.13.7 Alterar a designação do benefício
  PUT https://localhost:7180/api/reference-lists/benefits/734ede2d-1589-4620-895b-e48e5454dd9c [200 OK, 246B, 35ms]
  √  Código de estado 200
  √  Estrutura da resposta: ReferenceItemDto
  √  Mesmo identificador e nova designação

└ 6.13.4 Erro: designação repetida (ERR-10)
  POST https://localhost:7180/api/reference-lists/skills [400 Bad Request, 339B, 4.6s]
  √  Código de estado 400
  √  Estrutura da resposta: ApiErrorDto
  √  Código de erro ERR-10
  √  Campo name: designação repetida

□ 02 Contas e sessão
└ 6.1.1 Registar-se como Candidato (Candidato 1)
  POST https://localhost:7180/api/auth/candidates [201 Created, 577B, 250ms]
  √  Código de estado 201
  √  Estrutura da resposta: LoginResponse
  √  Cabeçalho Location
  √  Conta de Candidato

└ 6.1.1 Registar-se como Candidato (Candidato 2)
  POST https://localhost:7180/api/auth/candidates [201 Created, 577B, 263ms]
  √  Código de estado 201
  √  Estrutura da resposta: LoginResponse
  √  Cabeçalho Location
  √  Conta de Candidato

└ 6.1.2 Criar conta de Recrutador
  POST https://localhost:7180/api/auth/recruiters [201 Created, 543B, 258ms]
  √  Código de estado 201
  √  Estrutura da resposta: LoginResponse
  √  Conta de Recrutador
  √  Sem cabeçalho Location

└ 6.1.3 Iniciar sessão — Candidato 1 (mobile)
  POST https://localhost:7180/api/auth/login [200 OK, 538B, 245ms]
  √  Código de estado 200
  √  Estrutura da resposta: LoginResponse
  √  Mesma conta do registo

└ 6.1.3 Iniciar sessão — Recrutador (web)
  POST https://localhost:7180/api/auth/login [200 OK, 538B, 174ms]
  √  Código de estado 200
  √  Estrutura da resposta: LoginResponse

└ 6.1.5 Alterar a própria palavra-passe — Candidato 1
  PUT https://localhost:7180/api/auth/password [204 No Content, 81B, 491ms]
  √  Código de estado 204
  √  Resposta sem corpo

└ 6.1.3 Iniciar sessão com a nova palavra-passe — Candidato 1 (mobile)
  POST https://localhost:7180/api/auth/login [200 OK, 538B, 234ms]
  √  Código de estado 200
  √  Estrutura da resposta: LoginResponse

└ 6.1.3 Erro: palavra-passe errada (ERR-02)
  POST https://localhost:7180/api/auth/login [401 Unauthorized, 292B, 2s]
  √  Código de estado 401
  √  Estrutura da resposta: ApiErrorDto
  √  Código de erro ERR-02

└ 6.1.3 Erro: Candidato pelo ponto de acesso web (ERR-05)
  POST https://localhost:7180/api/auth/login [403 Forbidden, 269B, 1757ms]
  √  Código de estado 403
  √  Estrutura da resposta: ApiErrorDto
  √  Código de erro ERR-05

□ 03 Registo e aprovação da Empresa
└ 6.4.1 Registar a Empresa
  POST https://localhost:7180/api/recruiter/company [201 Created, 333B, 38ms]
  √  Código de estado 201
  √  Estrutura da resposta: CompanyStatusDto
  √  Cabeçalho Location
  √  Empresa pendente

└ 6.4.3 Consultar o estado do pedido (pendente)
  GET https://localhost:7180/api/recruiter/company/status [200 OK, 286B, 13ms]
  √  Código de estado 200
  √  Estrutura da resposta: CompanyStatusDto
  √  Estado PENDING sem motivo

└ 6.4.8 Consultar os dados de registo da própria Empresa
  GET https://localhost:7180/api/recruiter/company [200 OK, 599B, 8ms]
  √  Código de estado 200
  √  Estrutura da resposta: CompanyRegistrationDto
  √  NIF registado

└ 6.5.1 Erro: vagas com a Empresa pendente (ERR-07)
  GET https://localhost:7180/api/recruiter/jobs [403 Forbidden, 283B, 13ms]
  √  Código de estado 403
  √  Estrutura da resposta: ApiErrorDto
  √  Código de erro ERR-07

└ 6.11.1 Consultar as Empresas pendentes
  GET https://localhost:7180/api/admin/companies/pending [200 OK, 638B, 17ms]
  √  Código de estado 200
  √  Estrutura da resposta: PendingCompanyDto[]
  √  A Empresa registada está na lista

└ 6.11.2 Consultar os dados submetidos
  GET https://localhost:7180/api/admin/companies/f31932b1-c42e-4513-9707-6f0c51724170/registration [200 OK, 599B, 13ms]
  √  Código de estado 200
  √  Estrutura da resposta: CompanyRegistrationDto
  √  Empresa e localidade

└ 6.11.4 Recusar o registo com motivo
  POST https://localhost:7180/api/admin/companies/f31932b1-c42e-4513-9707-6f0c51724170/reject [204 No Content, 81B, 34ms]
  √  Código de estado 204
  √  Resposta sem corpo

└ 6.4.3 Consultar o estado do pedido (recusado)
  GET https://localhost:7180/api/recruiter/company/status [200 OK, 351B, 14ms]
  √  Código de estado 200
  √  Estrutura da resposta: CompanyStatusDto
  √  Estado REJECTED com o motivo

└ 6.4.2 Corrigir e submeter novamente o pedido
  POST https://localhost:7180/api/recruiter/company/resubmission [200 OK, 287B, 35ms]
  √  Código de estado 200
  √  Estrutura da resposta: CompanyStatusDto
  √  De novo pendente, sem motivo

└ 6.11.3 Aprovar o registo
  POST https://localhost:7180/api/admin/companies/f31932b1-c42e-4513-9707-6f0c51724170/approve [204 No Content, 81B, 17ms]
  √  Código de estado 204
  √  Resposta sem corpo

└ 6.11.3 Erro: aprovar de novo (ERR-30)
  POST https://localhost:7180/api/admin/companies/f31932b1-c42e-4513-9707-6f0c51724170/approve [409 Conflict, 273B, 1755ms]
  √  Código de estado 409
  √  Estrutura da resposta: ApiErrorDto
  √  Código de erro ERR-30

□ 04 Dados e página da Empresa
└ 6.4.4 Alterar os dados de registo da Empresa aprovada
  PUT https://localhost:7180/api/recruiter/company [200 OK, 287B, 40ms]
  √  Código de estado 200
  √  Estrutura da resposta: CompanyStatusDto
  √  Continua aprovada

└ 6.4.5 Gravar a página de apresentação
  PUT https://localhost:7180/api/recruiter/company/page [200 OK, 410B, 31ms]
  √  Código de estado 200
  √  Estrutura da resposta: CompanyPageDto
  √  Descrição e sítio gravados

└ 6.4.6 Carregar o logótipo
  PUT https://localhost:7180/api/recruiter/company/logo [204 No Content, 153B, 38ms]
  √  Código de estado 204
  √  Resposta sem corpo
  √  Cabeçalho Location

└ 6.4.7 Acrescentar uma fotografia à galeria
  POST https://localhost:7180/api/recruiter/company/photos [201 Created, 170B, 37ms]
  √  Código de estado 201
  √  Cabeçalho Location

└ 6.4.9 Consultar a página de apresentação da própria Empresa
  GET https://localhost:7180/api/recruiter/company/page [200 OK, 531B, 17ms]
  √  Código de estado 200
  √  Estrutura da resposta: CompanyPageDto
  √  Logótipo e uma fotografia na galeria

└ 6.9.1 Obter o logótipo da Empresa
  GET https://localhost:7180/api/files/COMPANY_LOGO/dd6072ba98e9402dadb8f51589898ded.png [200 OK, 187B, 24ms]
  √  Código de estado 200
  √  Tipo de conteúdo image/png
  √  Conteúdo não vazio

□ 05 Gestão de vagas
└ 6.5.2 Criar uma vaga (Programador Postman A)
  POST https://localhost:7180/api/recruiter/jobs [201 Created, 782B, 46ms]
  √  Código de estado 201
  √  Estrutura da resposta: JobDto
  √  Vaga não publicada, sem Location

└ 6.5.3 Alterar uma vaga (A)
  PUT https://localhost:7180/api/recruiter/jobs/e5f85fef-0a52-4790-8044-bb5b72d83c87 [200 OK, 776B, 45ms]
  √  Código de estado 200
  √  Estrutura da resposta: JobDto
  √  Dados alterados

└ 6.5.4 Publicar uma vaga (A)
  POST https://localhost:7180/api/recruiter/jobs/e5f85fef-0a52-4790-8044-bb5b72d83c87/publish [200 OK, 802B, 41ms]
  √  Código de estado 200
  √  Estrutura da resposta: JobDto
  √  Estado PUBLISHED

└ 6.5.9 Acrescentar uma fotografia à vaga (A)
  POST https://localhost:7180/api/recruiter/jobs/e5f85fef-0a52-4790-8044-bb5b72d83c87/photos [201 Created, 166B, 36ms]
  √  Código de estado 201
  √  Cabeçalho Location

└ 6.5.5 Suspender uma vaga (A)
  POST https://localhost:7180/api/recruiter/jobs/e5f85fef-0a52-4790-8044-bb5b72d83c87/suspend [200 OK, 860B, 22ms]
  √  Código de estado 200
  √  Estrutura da resposta: JobDto
  √  Estado SUSPENDED

└ 6.5.6 Voltar a publicar uma vaga suspensa (A)
  POST https://localhost:7180/api/recruiter/jobs/e5f85fef-0a52-4790-8044-bb5b72d83c87/republish [200 OK, 861B, 46ms]
  √  Código de estado 200
  √  Estrutura da resposta: JobDto
  √  Estado PUBLISHED

└ 6.5.2 Criar uma vaga (Analista Postman B)
  POST https://localhost:7180/api/recruiter/jobs [201 Created, 779B, 39ms]
  √  Código de estado 201
  √  Estrutura da resposta: JobDto
  √  Vaga não publicada, sem Location

└ 6.5.4 Publicar uma vaga (B)
  POST https://localhost:7180/api/recruiter/jobs/a64a2bcd-7e08-473f-b0ae-8317238a4927/publish [200 OK, 800B, 45ms]
  √  Código de estado 200
  √  Estrutura da resposta: JobDto
  √  Estado PUBLISHED

└ 6.5.2 Criar uma vaga (Vaga Postman C a eliminar)
  POST https://localhost:7180/api/recruiter/jobs [201 Created, 786B, 33ms]
  √  Código de estado 201
  √  Estrutura da resposta: JobDto
  √  Vaga não publicada, sem Location

└ 6.5.8 Eliminar uma vaga sem interesses (C)
  DELETE https://localhost:7180/api/recruiter/jobs/d1e594c1-edf6-4908-ab45-719bb95bfb2c [204 No Content, 81B, 37ms]
  √  Código de estado 204
  √  Resposta sem corpo

└ 6.5.1 Listar as vagas da Empresa
  GET https://localhost:7180/api/recruiter/jobs [200 OK, 1.51kB, 23ms]
  √  Código de estado 200
  √  Estrutura da resposta: JobDto[]
  √  Vagas A e B na lista, sem a eliminada
  √  A vaga A tem a fotografia

□ 06 Perfil do Candidato
└ 6.2.1 Consultar o perfil profissional
  GET https://localhost:7180/api/candidate/profile [200 OK, 636B, 30ms]
  √  Código de estado 200
  √  Estrutura da resposta: CandidateProfileDto
  √  Perfil do Candidato 1, sem CV

└ 6.2.2 Gravar os dados do perfil profissional
  PUT https://localhost:7180/api/candidate/profile [200 OK, 717B, 37ms]
  √  Código de estado 200
  √  Estrutura da resposta: CandidateProfileDto
  √  Dados gravados

└ 6.2.3 Definir as preferências de procura
  PUT https://localhost:7180/api/candidate/profile/preferences [200 OK, 260B, 21ms]
  √  Código de estado 200
  √  Estrutura da resposta: SearchPreferencesDto
  √  Preferências gravadas

└ 6.2.4 Acrescentar uma competência da lista (1)
  POST https://localhost:7180/api/candidate/profile/skills [201 Created, 810B, 40ms]
  √  Código de estado 201
  √  Estrutura da resposta: CandidateProfileDto
  √  Competência no perfil

└ 6.2.4 Acrescentar uma competência da lista (2)
  POST https://localhost:7180/api/candidate/profile/skills [201 Created, 888B, 47ms]
  √  Código de estado 201
  √  Estrutura da resposta: CandidateProfileDto

└ 6.2.5 Retirar uma competência da lista (2)
  DELETE https://localhost:7180/api/candidate/profile/skills/03bc9d07-6ef2-5f2e-8ecf-4bd13d2696c3 [204 No Content, 81B, 21ms]
  √  Código de estado 204
  √  Resposta sem corpo

└ 6.2.10 Acrescentar uma competência «Outro»
  POST https://localhost:7180/api/candidate/profile/custom-skills [201 Created, 885B, 33ms]
  √  Código de estado 201
  √  Estrutura da resposta: CandidateProfileDto
  √  Competência «Outro» no perfil

└ 6.2.11 Retirar uma competência «Outro»
  DELETE https://localhost:7180/api/candidate/profile/custom-skills/a5494449-7d57-47f2-bf59-eb93f299ee56 [204 No Content, 81B, 29ms]
  √  Código de estado 204
  √  Resposta sem corpo

└ 6.2.6 Indicar as hiperligações profissionais
  PUT https://localhost:7180/api/candidate/profile/links [200 OK, 886B, 40ms]
  √  Código de estado 200
  √  Estrutura da resposta: CandidateProfileDto
  √  Duas hiperligações
  √  Só a competência 1 da lista e nenhuma «Outro»

└ 6.2.7 Carregar a fotografia de perfil
  PUT https://localhost:7180/api/candidate/profile/photo [204 No Content, 154B, 21ms]
  √  Código de estado 204
  √  Resposta sem corpo
  √  Cabeçalho Location

└ 6.9.1 Obter a própria fotografia de perfil
  GET https://localhost:7180/api/files/PROFILE_PHOTO/c49cc62508b94ae6b7bf6acfc96f3290.png [200 OK, 187B, 11ms]
  √  Código de estado 200
  √  Tipo de conteúdo image/png
  √  Conteúdo não vazio

└ 6.2.8 Enviar o curriculum vitae
  PUT https://localhost:7180/api/candidate/profile/cv [204 No Content, 81B, 17ms]
  √  Código de estado 204
  √  Resposta sem corpo
  √  Sem cabeçalho Location

└ 6.2.9 Obter o próprio curriculum vitae
  GET https://localhost:7180/api/candidate/profile/cv [200 OK, 746B, 13ms]
  √  Código de estado 200
  √  Tipo de conteúdo application/pdf
  √  Conteúdo não vazio

□ 07 Exploração de vagas
└ 6.3.6 Consultar os interesses disponíveis
  GET https://localhost:7180/api/candidate/jobs/quota [200 OK, 195B, 10ms]
  √  Código de estado 200
  √  Estrutura da resposta: QuotaDto
  √  Quota disponível e sem bloqueio

└ 6.3.1 Obter o cartão de vaga seguinte
  GET https://localhost:7180/api/candidate/jobs/next [200 OK, 590B, 40ms]
  √  Código de estado 200 ou 204
  √  Estrutura da resposta: JobCardDto (200) ou sem corpo (204)

└ 6.3.2 Consultar o detalhe de uma vaga (A)
  GET https://localhost:7180/api/candidate/jobs/e5f85fef-0a52-4790-8044-bb5b72d83c87 [200 OK, 798B, 22ms]
  √  Código de estado 200
  √  Estrutura da resposta: JobDetailDto
  √  Vaga A da Empresa registada

└ 6.3.3 Consultar a página de apresentação da Empresa
  GET https://localhost:7180/api/candidate/jobs/companies/f31932b1-c42e-4513-9707-6f0c51724170 [200 OK, 531B, 19ms]
  √  Código de estado 200
  √  Estrutura da resposta: CompanyPageDto
  √  Página com logótipo e galeria

└ 6.3.4 Recusar uma vaga (B)
  POST https://localhost:7180/api/candidate/jobs/a64a2bcd-7e08-473f-b0ae-8317238a4927/decline [204 No Content, 81B, 29ms]
  √  Código de estado 204
  √  Resposta sem corpo

└ 6.3.5 Manifestar interesse numa vaga (A) — Candidato 1
  POST https://localhost:7180/api/candidate/jobs/e5f85fef-0a52-4790-8044-bb5b72d83c87/interest [200 OK, 194B, 41ms]
  √  Código de estado 200
  √  Estrutura da resposta: QuotaDto
  √  Um interesse a menos

└ 6.3.5 Manifestar interesse numa vaga (A) — Candidato 2
  POST https://localhost:7180/api/candidate/jobs/e5f85fef-0a52-4790-8044-bb5b72d83c87/interest [200 OK, 194B, 62ms]
  √  Código de estado 200
  √  Estrutura da resposta: QuotaDto

└ 6.3.5 Erro: segundo interesse na mesma vaga (ERR-32)
  POST https://localhost:7180/api/candidate/jobs/e5f85fef-0a52-4790-8044-bb5b72d83c87/interest [409 Conflict, 242B, 2s]
  √  Código de estado 409
  √  Estrutura da resposta: ApiErrorDto
  √  Código de erro ERR-32

□ 08 Avaliação de candidatos
└ 6.6.1 Consultar os candidatos em espera (A)
  GET https://localhost:7180/api/recruiter/jobs/e5f85fef-0a52-4790-8044-bb5b72d83c87/waiting-candidates [200 OK, 627B, 23ms]
  √  Código de estado 200
  √  Estrutura da resposta: WaitingCandidateDto[]
  √  Os dois Candidatos em espera

└ 6.6.5 Erro: recusar sem abrir o perfil (ERR-34)
  POST https://localhost:7180/api/recruiter/interests/14c3e9e2-1488-4fb7-8328-6e6869efc032/reject [409 Conflict, 281B, 1655ms]
  √  Código de estado 409
  √  Estrutura da resposta: ApiErrorDto
  √  Código de erro ERR-34

└ 6.6.2 Abrir o perfil completo do Candidato (2)
  GET https://localhost:7180/api/recruiter/interests/14c3e9e2-1488-4fb7-8328-6e6869efc032/profile [200 OK, 431B, 60ms]
  √  Código de estado 200
  √  Estrutura da resposta: CandidateFullProfileDto

└ 6.6.5 Recusar o Candidato (2)
  POST https://localhost:7180/api/recruiter/interests/14c3e9e2-1488-4fb7-8328-6e6869efc032/reject [204 No Content, 81B, 36ms]
  √  Código de estado 204
  √  Resposta sem corpo

└ 6.6.2 Abrir o perfil completo do Candidato (1)
  GET https://localhost:7180/api/recruiter/interests/ac2a2c56-0b9a-42a5-a1bb-40c0c5dcd159/profile [200 OK, 680B, 47ms]
  √  Código de estado 200
  √  Estrutura da resposta: CandidateFullProfileDto
  √  Perfil com CV e a competência da vaga assinalada

└ 6.6.3 Obter o curriculum vitae (1)
  GET https://localhost:7180/api/recruiter/interests/ac2a2c56-0b9a-42a5-a1bb-40c0c5dcd159/cv [200 OK, 846B, 1500ms]
  √  Código de estado 200
  √  Tipo de conteúdo application/pdf
  √  Conteúdo não vazio

└ 6.6.4 Aceitar o Candidato (1)
  POST https://localhost:7180/api/recruiter/interests/ac2a2c56-0b9a-42a5-a1bb-40c0c5dcd159/accept [200 OK, 602B, 42ms]
  √  Código de estado 200
  √  Estrutura da resposta: MatchDto
  √  Match da vaga A

└ 6.6.4 Erro: decidir de novo (ERR-35)
  POST https://localhost:7180/api/recruiter/interests/ac2a2c56-0b9a-42a5-a1bb-40c0c5dcd159/accept [409 Conflict, 256B, 2.2s]
  √  Código de estado 409
  √  Estrutura da resposta: ApiErrorDto
  √  Código de erro ERR-35

□ 09 Matches e conversas
└ 6.7.1 Consultar os matches (Candidato 1)
  GET https://localhost:7180/api/matches [200 OK, 598B, 12ms]
  √  Código de estado 200
  √  Estrutura da resposta: MatchDto[]
  √  Match da vaga A

└ 6.7.1 Consultar os matches (Recrutador)
  GET https://localhost:7180/api/matches [200 OK, 603B, 21ms]
  √  Código de estado 200
  √  Estrutura da resposta: MatchDto[]
  √  Match com o Candidato 1

└ 6.8.1 Consultar a lista de conversas (Candidato 1)
  GET https://localhost:7180/api/conversations [200 OK, 430B, 23ms]
  √  Código de estado 200
  √  Estrutura da resposta: ConversationSummaryDto[]
  √  Conversa aberta do match

└ 6.8.3 Enviar uma mensagem (Recrutador)
  POST https://localhost:7180/api/conversations/ac2a2c56-0b9a-42a5-a1bb-40c0c5dcd159/messages [201 Created, 380B, 27ms]
  √  Código de estado 201
  √  Estrutura da resposta: MessageDto
  √  Enviada pelo Recrutador

└ 6.8.2 Abrir uma conversa (Candidato 1)
  GET https://localhost:7180/api/conversations/ac2a2c56-0b9a-42a5-a1bb-40c0c5dcd159/messages [200 OK, 401B, 41ms]
  √  Código de estado 200
  √  Estrutura da resposta: MessageDto[]
  √  Mensagem do Recrutador na conversa

└ 6.8.3 Enviar uma mensagem (Candidato 1)
  POST https://localhost:7180/api/conversations/ac2a2c56-0b9a-42a5-a1bb-40c0c5dcd159/messages [201 Created, 390B, 26ms]
  √  Código de estado 201
  √  Estrutura da resposta: MessageDto

└ 6.8.2 Abrir uma conversa com after (Recrutador)
  GET https://localhost:7180/api/conversations/ac2a2c56-0b9a-42a5-a1bb-40c0c5dcd159/messages?after=2026-11-03T00:02:21.7735966Z [200 OK, 410B, 43ms]
  √  Código de estado 200
  √  Estrutura da resposta: MessageDto[]
  √  Só as mensagens depois de after

└ 6.8.4 Encerrar uma conversa (Candidato 1)
  POST https://localhost:7180/api/conversations/ac2a2c56-0b9a-42a5-a1bb-40c0c5dcd159/close [204 No Content, 81B, 30ms]
  √  Código de estado 204
  √  Resposta sem corpo

└ 6.8.1 Consultar a lista de conversas (Recrutador)
  GET https://localhost:7180/api/conversations [200 OK, 503B, 25ms]
  √  Código de estado 200
  √  Estrutura da resposta: ConversationSummaryDto[]
  √  Conversa encerrada pelo Candidato

└ 6.8.3 Erro: mensagem numa conversa encerrada (ERR-36)
  POST https://localhost:7180/api/conversations/ac2a2c56-0b9a-42a5-a1bb-40c0c5dcd159/messages [409 Conflict, 260B, 2s]
  √  Código de estado 409
  √  Estrutura da resposta: ApiErrorDto
  √  Código de erro ERR-36

□ 10 Administração de contas
└ 6.10.1 Consultar as contas de utilizador
  GET https://localhost:7180/api/admin/accounts [200 OK, 5.02kB, 14ms]
  √  Código de estado 200
  √  Estrutura da resposta: AccountDto[]
  √  Contas criadas na lista

└ 6.10.2 Consultar a lista de Candidatos
  GET https://localhost:7180/api/admin/accounts/candidates [200 OK, 6.07kB, 25ms]
  √  Código de estado 200
  √  Estrutura da resposta: CandidateAdminDto[]
  √  Candidato 2 ativo na lista

└ 6.10.3 Bloquear uma conta (Candidato 2)
  POST https://localhost:7180/api/admin/accounts/9565b0bc-76e8-47a1-8e4d-3c4dcbf99528/block [204 No Content, 81B, 26ms]
  √  Código de estado 204
  √  Resposta sem corpo

└ 6.2.1 Erro: pedido de conta bloqueada (ERR-03)
  GET https://localhost:7180/api/candidate/profile [403 Forbidden, 230B, 10ms]
  √  Código de estado 403
  √  Estrutura da resposta: ApiErrorDto
  √  Código de erro ERR-03

└ 6.10.5 Reativar uma conta bloqueada (Candidato 2)
  POST https://localhost:7180/api/admin/accounts/9565b0bc-76e8-47a1-8e4d-3c4dcbf99528/reactivate [204 No Content, 81B, 21ms]
  √  Código de estado 204
  √  Resposta sem corpo

└ 6.10.4 Suspender a conta de um Candidato (2)
  POST https://localhost:7180/api/admin/accounts/candidates/9565b0bc-76e8-47a1-8e4d-3c4dcbf99528/suspend [204 No Content, 81B, 32ms]
  √  Código de estado 204
  √  Resposta sem corpo

└ 6.10.5 Reativar uma conta suspensa (Candidato 2)
  POST https://localhost:7180/api/admin/accounts/9565b0bc-76e8-47a1-8e4d-3c4dcbf99528/reactivate [204 No Content, 81B, 20ms]
  √  Código de estado 204
  √  Resposta sem corpo

□ 11 Supervisão de Empresas e indicadores
└ 6.11.7 Consultar a lista global de Empresas
  GET https://localhost:7180/api/admin/companies [200 OK, 1.53kB, 15ms]
  √  Código de estado 200
  √  Estrutura da resposta: CompanyAdminDto[]
  √  Empresa aprovada com 2 vagas publicadas

└ 6.11.8 Consultar a lista global de vagas publicadas
  GET https://localhost:7180/api/admin/companies/published-jobs [200 OK, 10.35kB, 29ms]
  √  Código de estado 200
  √  Estrutura da resposta: PublishedJobAdminDto[]
  √  Vaga A com 2 interesses

└ 6.11.5 Suspender uma Empresa aprovada
  POST https://localhost:7180/api/admin/companies/f31932b1-c42e-4513-9707-6f0c51724170/suspend [204 No Content, 81B, 21ms]
  √  Código de estado 204
  √  Resposta sem corpo

└ 6.5.1 Erro: vagas com a Empresa suspensa (ERR-07)
  GET https://localhost:7180/api/recruiter/jobs [403 Forbidden, 283B, 11ms]
  √  Código de estado 403
  √  Estrutura da resposta: ApiErrorDto
  √  Código de erro ERR-07

└ 6.11.6 Reativar uma Empresa suspensa
  POST https://localhost:7180/api/admin/companies/f31932b1-c42e-4513-9707-6f0c51724170/reactivate [204 No Content, 81B, 26ms]
  √  Código de estado 204
  √  Resposta sem corpo

└ 6.12.1 Consultar o painel de indicadores por período
  GET https://localhost:7180/api/admin/indicators?from=2026-10-04&to=2026-11-03 [200 OK, 308B, 43ms]
  √  Código de estado 200
  √  Estrutura da resposta: IndicatorsDto
  √  Contagens da execução incluídas

└ 6.12.1 Erro: data final no futuro (ERR-10)
  GET https://localhost:7180/api/admin/indicators?from=2026-10-04&to=2999-12-31 [400 Bad Request, 353B, 1992ms]
  √  Código de estado 400
  √  Estrutura da resposta: ApiErrorDto
  √  Código de erro ERR-10

└ 6.8.1 Erro: Administrador nas conversas (ERR-06)
  GET https://localhost:7180/api/conversations [403 Forbidden, 282B, 5ms]
  √  Código de estado 403
  √  Estrutura da resposta: ApiErrorDto
  √  Código de erro ERR-06

□ 12 Encerramento e fim de sessão
└ 6.5.7 Encerrar uma vaga (A)
  POST https://localhost:7180/api/recruiter/jobs/e5f85fef-0a52-4790-8044-bb5b72d83c87/close [200 OK, 857B, 35ms]
  √  Código de estado 200
  √  Estrutura da resposta: JobDto
  √  Estado CLOSED

└ 6.1.4 Terminar sessão (Candidato 1)
  POST https://localhost:7180/api/auth/logout [204 No Content, 81B, 11ms]
  √  Código de estado 204
  √  Resposta sem corpo

□ 13 Dados de demonstração
└ 6.1.3 Iniciar sessão — Candidato de demonstração (mobile)
  POST https://localhost:7180/api/auth/login [200 OK, 538B, 242ms]
  √  Código de estado 200
  √  Estrutura da resposta: LoginResponse

└ 6.7.1 Consultar os matches (Candidato de demonstração)
  GET https://localhost:7180/api/matches [200 OK, 505B, 20ms]
  √  Código de estado 200
  √  Estrutura da resposta: MatchDto[]
  √  Pelo menos um match de demonstração

└ 6.1.3 Iniciar sessão — Recrutador de demonstração (web)
  POST https://localhost:7180/api/auth/login [200 OK, 538B, 273ms]
  √  Código de estado 200
  √  Estrutura da resposta: LoginResponse

└ 6.5.1 Listar as vagas (Recrutador de demonstração)
  GET https://localhost:7180/api/recruiter/jobs [200 OK, 9.56kB, 23ms]
  √  Código de estado 200
  √  Estrutura da resposta: JobDto[]
  √  Vagas publicadas de demonstração

└ 6.8.1 Consultar a lista de conversas (Recrutador de demonstração)
  GET https://localhost:7180/api/conversations [200 OK, 736B, 19ms]
  √  Código de estado 200
  √  Estrutura da resposta: ConversationSummaryDto[]
  √  Pelo menos uma conversa de demonstração

┌─────────────────────────┬───────────────────┬───────────────────┐
│                         │          executed │            failed │
├─────────────────────────┼───────────────────┼───────────────────┤
│              iterations │                 1 │                 0 │
├─────────────────────────┼───────────────────┼───────────────────┤
│                requests │               107 │                 0 │
├─────────────────────────┼───────────────────┼───────────────────┤
│            test-scripts │               107 │                 0 │
├─────────────────────────┼───────────────────┼───────────────────┤
│      prerequest-scripts │                 2 │                 0 │
├─────────────────────────┼───────────────────┼───────────────────┤
│              assertions │               300 │                 0 │
├─────────────────────────┴───────────────────┴───────────────────┤
│ total run duration: 37.8s                                       │
├─────────────────────────────────────────────────────────────────┤
│ total data received: 65.39kB (approx)                           │
├─────────────────────────────────────────────────────────────────┤
│ average response time: 250ms [min: 5ms, max: 4.6s, s.d.: 669ms] │
└─────────────────────────────────────────────────────────────────┘
