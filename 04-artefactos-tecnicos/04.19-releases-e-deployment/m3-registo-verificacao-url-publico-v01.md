I119 - Evidencia da publicacao do backend no Railway

URL publico:        https://katch-backend-production.up.railway.app
Versao publicada:   tag v0.3.0 (anotada), commit 3f4a070946035440e8ad9c746b83a7b904478881 (clone limpo, HEAD confirmado)
Plataforma:         Railway, projeto easygoing-freedom, ambiente production, regiao US West; servico katch-backend + PostgreSQL 18.6 (imagem postgres-ssl:18)
Build:              Dockerfile e .dockerignore acrescentados SO na copia enviada com "railway up" (fora do git e da tag): dotnet publish do Katch.Backend.csproj (SDK/aspnet 10.0), arranque em http://0.0.0.0:$PORT. O resto da arvore e a da tag.
Variaveis (so no Railway): ConnectionStrings__Katch (por referencia ao servico Postgres), Jwt__Secret (48 bytes aleatorios, 65 caracteres), ASPNETCORE_ENVIRONMENT=Development (necessario para o Swagger na v0.3.0). Nenhum segredo no repositorio.
Migrations:         dotnet ef database update (10 migrations; ultima 20261024180929_AddMessageAndOperationLog), por tunel SSH (railway connect --tunnel-only), sem acesso publico a base.
Seeds:              database/02-seed-reference-data.sql e database/03-seed-demo-data.sql (psql 18).
Confirmacao por query direta: __EFMigrationsHistory = 10 linhas; 04-verify.sql: 17 tabelas, existe_notification=f, 20 candidatos ativos, 5 empresas aprovadas, 2 pendentes, 50 vagas publicadas, 1 administrador, 0 resumos fora do formato; base com locale en_US.utf8.
Incidente:          1.o login com 500 (InvalidCastException em AccountStatus): a API arrancou antes das migrations e o Npgsql guardou em cache os tipos enum em falta; resolvido com "railway redeploy" (sem alteracao de codigo).
Nota:               pedidos feitos a partir da rede do grupo (PC do Executor). A verificacao a partir de fora da rede do grupo (dados moveis ou outro local) fica por registar abaixo.

Verificacao a https://katch-backend-production.up.railway.app em 2026-11-04 22:11 (hora local)

Swagger JSON                                               GET    /swagger/v1/swagger.json  ->  200
    {
  "openapi": "3.0.4",
  "info": {
    "title": "Katch.Backend",
    "version": "1.0"
  },
  "paths": {
    "/api/admin/accounts": {
      "get": {
        "tags": [
          "AdminAccounts"
        ],
        "summary ...
Lista de localidades (anonimo)                             GET    /api/reference-lists/locations  ->  200
    [{"id":"82ac4427-afc2-5c18-bcda-1f5bb7511c5c","name":"Almada (Setúbal)"},{"id":"0ee69348-6e11-50ec-9e79-49fbe69ea6f0","name":"Amarante (Porto)"},{"id":"672b1ca0-3721-5b2f-bd2f-93c5aa7e0456","name":"Aveiro (Aveiro)"},{"id ...
Registo Candidato: sem aceitar condicoes (erro)            POST   /api/auth/candidates  ->  400
    {"code":"ERR-10","message":"Os dados enviados não cumprem a validação automática.","blockedUntil":null,"fields":[{"field":"acceptTerms","reason":"É obrigatório aceitar as condições de utilização."}]}
Registo Candidato (sucesso)                                POST   /api/auth/candidates  ->  201
    {"token":"<oculto>","expiresAt":"2026-11-05T06:11:26Z","userType":"CANDIDATE","userId":"a9bef0fc-5d87-423b-aeb1-905d93af4d52"}
Registo Candidato: correio repetido (erro)                 POST   /api/auth/candidates  ->  400
    {"code":"ERR-10","message":"Os dados enviados não cumprem a validação automática.","blockedUntil":null,"fields":[{"field":"email","reason":"O endereço de correio eletrónico já está registado."}]}
Login Candidato: palavra-passe errada (erro)               POST   /api/auth/login  ->  401
    {"code":"ERR-02","message":"O endereço de correio eletrónico ou a palavra-passe estão incorretos.","blockedUntil":null,"fields":[]}
Login Candidato: sem X-Client-App (erro)                   POST   /api/auth/login  ->  400
    {"code":"ERR-10","message":"Os dados enviados não cumprem a validação automática.","blockedUntil":null,"fields":[{"field":"X-Client-App","reason":"O campo é obrigatório."}]}
Login Candidato no ponto de acesso web (erro)              POST   /api/auth/login  ->  403
    {"code":"ERR-05","message":"Esta conta não tem acesso por este ponto de acesso.","blockedUntil":null,"fields":[]}
Login Candidato (sucesso)                                  POST   /api/auth/login  ->  200
    {"token":"<oculto>","expiresAt":"2026-11-05T06:11:28Z","userType":"CANDIDATE","userId":"a9bef0fc-5d87-423b-aeb1-905d93af4d52"}
Registo Recrutador: palavra-passe fraca (erro)             POST   /api/auth/recruiters  ->  400
    {"code":"ERR-10","message":"Os dados enviados não cumprem a validação automática.","blockedUntil":null,"fields":[{"field":"password","reason":"A palavra-passe tem de ter pelo menos 8 caracteres, uma letra e um algarismo. ...
Registo Recrutador (sucesso)                               POST   /api/auth/recruiters  ->  201
    {"token":"<oculto>","expiresAt":"2026-11-05T06:11:28Z","userType":"RECRUITER","userId":"d5459ea2-89cd-4364-bbf6-40e4fc574593"}
Login Recrutador (sucesso)                                 POST   /api/auth/login  ->  200
    {"token":"<oculto>","expiresAt":"2026-11-05T06:11:29Z","userType":"RECRUITER","userId":"d5459ea2-89cd-4364-bbf6-40e4fc574593"}
Vagas sem Empresa registada (erro de politica)             GET    /api/recruiter/jobs  ->  403
    {"code":"ERR-07","message":"A opera\u00E7\u00E3o exige a Empresa aprovada. Estado atual da Empresa: n\u00E3o registada.","blockedUntil":null,"fields":[]}
Registo da Empresa: NIF invalido (erro)                    POST   /api/recruiter/company  ->  400
    {"code":"ERR-10","message":"Os dados enviados não cumprem a validação automática.","blockedUntil":null,"fields":[{"field":"taxId","reason":"O número de identificação fiscal tem de ter 9 algarismos."}]}
Registo da Empresa (sucesso)                               POST   /api/recruiter/company  ->  201
    {"companyId":"499fc45d-b695-494b-a1be-f0505e7d59d7","status":"PENDING","rejectionReason":null,"submittedAt":"2026-11-04T22:11:30.5657363Z"}
Estado da Empresa (pendente)                               GET    /api/recruiter/company/status  ->  200
    {"companyId":"499fc45d-b695-494b-a1be-f0505e7d59d7","status":"PENDING","rejectionReason":null,"submittedAt":"2026-11-04T22:11:30.565736Z"}
Criar vaga com Empresa pendente (erro ERR-07)              POST   /api/recruiter/jobs  ->  403
    {"code":"ERR-07","message":"A opera\u00E7\u00E3o exige a Empresa aprovada. Estado atual da Empresa: pendente.","blockedUntil":null,"fields":[]}
Login Administrador (sucesso)                              POST   /api/auth/login  ->  200
    {"token":"<oculto>","expiresAt":"2026-11-05T06:11:31Z","userType":"ADMIN","userId":"2b72eb4e-10b8-41c3-b786-9bed027d59ad"}
Admin: lista de empresas pendentes                         GET    /api/admin/companies/pending  ->  200
    [{"companyId":"daff30f4-2485-5ce0-a790-fd25b4f60008","companyName":"Minho Metalomecânica, Lda.","taxId":"599900068","submittedAt":"2026-10-06T22:03:57.425109Z"},{"companyId":"34df8701-e1b0-51d9-aa2e-e9f09e1c7451","compan ...
Candidato a usar rota de Administrador (erro 403)          GET    /api/admin/companies/pending  ->  403
    {"code":"ERR-06","message":"A opera\u00E7\u00E3o n\u00E3o est\u00E1 dispon\u00EDvel para este tipo de conta.","blockedUntil":null,"fields":[]}
Sem credencial (erro 401)                                  GET    /api/admin/companies/pending  ->  401
    {"code":"ERR-01","message":"A sess\u00E3o expirou ou n\u00E3o \u00E9 v\u00E1lida. Inicie sess\u00E3o novamente.","blockedUntil":null,"fields":[]}
Admin: pendentes (para obter o id)                         GET    /api/admin/companies/pending  ->  200
    [{"companyId":"daff30f4-2485-5ce0-a790-fd25b4f60008","companyName":"Minho Metalomecânica, Lda.","taxId":"599900068","submittedAt":"2026-10-06T22:03:57.425109Z"},{"companyId":"34df8701-e1b0-51d9-aa2e-e9f09e1c7451","compan ...
Admin: aprovar a Empresa (sucesso)                         POST   /api/admin/companies/499fc45d-b695-494b-a1be-f0505e7d59d7/approve  ->  204
    
Admin: aprovar outra vez (erro)                            POST   /api/admin/companies/499fc45d-b695-494b-a1be-f0505e7d59d7/approve  ->  409
    {"code":"ERR-30","message":"A operação não é permitida no estado atual: aprovada.","blockedUntil":null,"fields":[]}
Criar vaga: salario minimo > maximo (erro)                 POST   /api/recruiter/jobs  ->  400
    {"code":"ERR-10","message":"Os dados enviados não cumprem a validação automática.","blockedUntil":null,"fields":[{"field":"minSalary","reason":"O valor mínimo não pode ser superior ao valor máximo."}]}
Criar vaga (sucesso)                                       POST   /api/recruiter/jobs  ->  201
    {"jobId":"778ca963-1be7-4e47-9553-9bcec9a334ba","title":"Programador de Avaliacao","description":"Vaga ficticia criada para a avaliacao da milestone m3.","minSalary":1200,"maxSalary":1800,"locationId":"82ac4427-afc2-5c18 ...
Recrutador: listagem das suas vagas                        GET    /api/recruiter/jobs  ->  200
    [{"jobId":"778ca963-1be7-4e47-9553-9bcec9a334ba","title":"Programador de Avaliacao","description":"Vaga ficticia criada para a avaliacao da milestone m3.","minSalary":1200.00,"maxSalary":1800.00,"locationId":"82ac4427-af ...
Recrutador a usar rota de Candidato (erro 403)             GET    /api/candidate/jobs/quota  ->  403
    {"code":"ERR-06","message":"A opera\u00E7\u00E3o n\u00E3o est\u00E1 dispon\u00EDvel para este tipo de conta.","blockedUntil":null,"fields":[]}
Candidato: quota de interesses                             GET    /api/candidate/jobs/quota  ->  200
    {"available":10,"total":10,"blockedUntil":null}
Candidato: listagem de vagas (proxima)                     GET    /api/candidate/jobs/next  ->  200
    {"jobId":"429068d8-6d9d-588a-8125-df0db1ac8471","title":"Técnico de contabilidade","companyName":"Contas Certas Consultores, Lda.","logoUrl":null,"minSalary":1250.00,"maxSalary":1450.00,"locationName":"Lisboa (Lisboa)"," ...
Admin: indicadores sem intervalo (erro)                    GET    /api/admin/indicators  ->  400
    {"code":"ERR-10","message":"Os dados enviados não cumprem a validação automática.","blockedUntil":null,"fields":[{"field":"from","reason":"O campo é obrigatório."},{"field":"to","reason":"O campo é obrigatório."}]}
Admin: indicadores (sucesso)                               GET    /api/admin/indicators?from=2026-10-05&to=2026-11-05  ->  400
    {"code":"ERR-10","message":"Os dados enviados não cumprem a validação automática.","blockedUntil":null,"fields":[{"field":"to","reason":"A data final não pode ser posterior à data atual."}]}
Terminar sessao do Candidato                               POST   /api/auth/logout  ->  204
    

Nota: o pedido "Admin: indicadores (sucesso)" acima deu 400 porque o parametro "to" era posterior a data atual (a validacao da API recusa-o); repetido com a data certa:
Admin: indicadores (sucesso, repetido com to = hoje)       GET    /api/admin/indicators?from=2026-10-05&to=2026-11-04  ->  200
    {"activeCandidates":21,"approvedCompanies":6,"pendingCompanies":2,"jobsFirstPublished":50,"interestsExpressed":6,"matchesConfirmed":3,"conversationsStarted":3}

Verificacao a partir de fora do ambiente de desenvolvimento (2026-11-04)
Declarada pelo Executor: todos os elementos do grupo acederam ao URL publico, em computador e em telemovel, e conseguiram usar o Swagger e iniciar sessao.
Por registar: dispositivo/rede de cada teste (ex.: dados moveis) e capturas de ecra, se existirem.
