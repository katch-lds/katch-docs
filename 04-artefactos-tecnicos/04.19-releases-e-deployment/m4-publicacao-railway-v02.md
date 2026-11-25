# Publicação do backend no Railway — m4-v02

| Campo | Valor |
| --- | --- |
| Projeto | Katch — LDS, LEI, 2026/2027, turma LEI3T2, grupo 11 |
| Documento | `m4-publicacao-railway-v02.md` |
| Versão publicada na m3 | `v0.3.0`, commit `3f4a070946035440e8ad9c746b83a7b904478881` |
| Versão com as correções da I136 | Merge Commit `796f81e99ef9d955eab4d27095443accf821f171` na `main` (PR #47, 2026-11-23). Ainda não tem tag Git. A publicação no Railway com esta versão ainda não foi feita |
| URL público | `https://katch-backend-production.up.railway.app` |
| Issue | I119 (v01); I137 (v02) |

## Histórico de versões

| Versão | Data | Alteração | Issue |
| --- | --- | --- | --- |
| v01 | 2026-11-04 | Primeira versão: procedimento de publicação no Railway. | I119 |
| v01 (revisão) | 2026-11-05 | Imagens do Dockerfile fixadas por digest (M02), documentação da criação das contas de avaliação (M03) e nota sobre os avisos do log (M06). | I119 |
| v02 | 2026-11-23 | Publicação do backend com as correções da I136: variáveis `ASPNETCORE_ENVIRONMENT=Production`, `Swagger__Enabled`, `ForwardedHeaders__Enabled` e `AllowedHosts`, e variáveis `Limits__*` opcionais; *Healthcheck Path* `/health/ready`; migration `AddLoginSecurity` aplicada antes da API (11 migrations); dados de demonstração (`03-seed-demo-data.sql`) fora do Railway; verificações novas (`/health`, `/health/ready`, `X-Correlation-Id`, colunas novas); aviso novo do log; limitação L-1 do Swagger. | I137 |

Este documento descreve como o backend foi publicado e como repetir o procedimento. Não contém segredos: os valores sensíveis só existem nas variáveis do serviço no Railway. O procedimento é retomado em m4 no guia de deployment (I085). A versão v02 aplica-se tanto a uma publicação nova como à atualização de uma publicação da m3 (`v0.3.0`): no segundo caso, os passos 4 e 5 (variáveis e migration) fazem-se **antes** de publicar a versão nova.

## 1. Estrutura no Railway

| Conceito | Neste projeto |
| --- | --- |
| Projeto | `easygoing-freedom` (ambiente `production`, região US West) |
| Serviço da API | `katch-backend`, construído a partir de um `Dockerfile` |
| Serviço da base de dados | `Postgres` (imagem `postgres-ssl:18`, PostgreSQL 18.6) com volume |
| Rede | A API liga-se à base por `postgres.railway.internal` (rede privada); a base não tem acesso público |
| Domínio | `katch-backend-production.up.railway.app`, gerado pelo Railway, com HTTPS |

## 2. Pré-requisitos

- Conta no Railway com acesso ao projeto e o CLI instalado (`npm install -g @railway/cli`), com `railway login` feito.
- SDK do .NET 10.0.401 ou superior e `psql` 18, para aplicar as migrations e os dados iniciais.
- Uma chave SSH registada no Railway (`railway ssh keys add`), para o túnel até à base.

## 3. Passos

1. **Obter a versão exata.** Clone limpo da tag da versão a publicar (na m3, `v0.3.0`; na m4, a tag que for criada sobre o commit `796f81e`, ou o próprio commit enquanto não houver tag), numa pasta com caminho curto:

   ```powershell
   git clone --branch <tag> --depth 1 https://github.com/katch-lds/katch-backend.git C:\k119
   git -C C:\k119 rev-parse HEAD     # tem de ser o commit registado para a tag (m3, v0.3.0: 3f4a070946035440e8ad9c746b83a7b904478881)
   ```

2. **Acrescentar os ficheiros de build** à cópia (não ao repositório): `Dockerfile` e `.dockerignore` da pasta `m3-configuracao-publicacao-v01`. As imagens base estão fixadas pelo digest que o build de 2026-11-04 usou (SDK `sha256:de4fe90a…`, runtime `sha256:de0ee358…`), para a publicação ser repetível com as mesmas imagens. O repositório não tem Dockerfile, e o Railpack do Railway não reconhece a solução, que tem os `.csproj` em `src/`.
3. **Criar o projeto** no Railway, com o serviço `katch-backend`, e **adicionar a base**: `railway add --database postgres`.
4. **Definir as variáveis** do serviço `katch-backend` (nunca no repositório):

   | Variável | Valor |
   | --- | --- |
   | `ConnectionStrings__Katch` | `Host=${{Postgres.PGHOST}};Port=${{Postgres.PGPORT}};Database=${{Postgres.PGDATABASE}};Username=${{Postgres.PGUSER}};Password=${{Postgres.PGPASSWORD}}` (referências ao serviço `Postgres`) |
   | `Jwt__Secret` | valor aleatório com pelo menos 32 bytes, gerado na altura e escrito direto no Railway |
   | `ASPNETCORE_ENVIRONMENT` | `Production` (sem página de exceções e com HSTS). Na m3 era `Development`, porque o Swagger só se ativava neste ambiente; com a I136 o Swagger depende de `Swagger__Enabled` |
   | `Swagger__Enabled` | `true` enquanto as contas de avaliação usarem o Swagger; sem esta variável, o Swagger não é publicado, em qualquer ambiente |
   | `ForwardedHeaders__Enabled` | `true`. O HTTPS termina no proxy do Railway: com a opção, a API usa o IP do cliente e o esquema que o proxy acrescenta no fim de `X-Forwarded-For` e `X-Forwarded-Proto`. Sem ela, o limite de pedidos por IP conta todos os pedidos como vindos do mesmo IP (o do proxy) |
   | `AllowedHosts` | `katch-backend-production.up.railway.app` (o domínio público do passo 8). **Sem esta variável a API responde `400` a todos os pedidos**, porque o `appsettings.json` só aceita `localhost`, `127.0.0.1` e `10.0.2.2` |

   O `Dockerfile` ata a API à porta `$PORT` do Railway, por isso não é preciso definir `ASPNETCORE_URLS`.

   Opcionais, com os valores por omissão do README do backend (secção 15), validados no arranque (um valor inválido faz a API não arrancar):

   | Variável | Por omissão | Efeito |
   | --- | --- | --- |
   | `Limits__MaxRequestBodyBytes` | `65536` | Corpo máximo dos pedidos fora dos carregamentos (`413`, ERR-12) |
   | `Limits__RateLimitWindowSeconds` | `60` | Duração da janela dos limites de pedidos |
   | `Limits__LoginPermitLimit` | `10` | Inícios de sessão por IP em cada janela |
   | `Limits__RegistrationPermitLimit` | `10` | Registos (Candidato e Recrutador, contagem comum) por IP em cada janela |
   | `Limits__LocationsPermitLimit` | `30` | Consultas anónimas das localidades por IP em cada janela |
   | `Limits__UserPermitLimit` | `300` | Pedidos de cada conta autenticada em cada janela |
   | `Limits__MaxFailedLogins` | `5` | Inícios de sessão falhados seguidos que bloqueiam a conta |
   | `Limits__LockoutSeconds` | `60` | Duração do primeiro bloqueio; cada novo bloqueio dobra-a |
   | `Limits__MaxLockoutSeconds` | `900` | Duração máxima de um bloqueio |
   | `Limits__IndicatorsMaxPeriodDays` | `366` | Período máximo dos indicadores |

   Em *Settings*, *Deploy*, *Healthcheck Path*, pode indicar-se `/health/ready`, para o Railway só trocar de versão quando a nova liga à base de dados.

5. **Aplicar as migrations e os dados iniciais** (antes de arrancar a API, ver o aviso do passo 7), por túnel. A migration `AddLoginSecurity` tem de ficar aplicada **antes** de a API nova arrancar: a API lê as colunas novas de `app_user` em cada pedido autenticado, e sem elas os pedidos falham. Num Railway já publicado com a m3 (10 migrations), o `dotnet ef database update` aplica só essa:

   ```powershell
   railway connect Postgres --tunnel-only -P 55432       # noutro terminal; mantém o túnel aberto
   $env:ConnectionStrings__Katch = "Host=127.0.0.1;Port=55432;Database=<db>;Username=<user>;Password=<palavra-passe>"
   $env:Katch__AdminPassword = "<palavra-passe do Administrador>"   # 8+ caracteres, com letra e algarismo
   dotnet tool restore
   dotnet ef database update --project src/Katch.Backend
   psql -v ON_ERROR_STOP=1 -f database/02-seed-reference-data.sql
   psql -f database/04-verify.sql
   ```

   O `03-seed-demo-data.sql` já não se aplica no Railway: as contas de demonstração partilham uma palavra-passe conhecida do grupo, e o script recusa-se a correr sem `-v demo=1`. Só serve em bases de desenvolvimento. As contas de avaliação criam-se pelos endpoints (secção 4). Uma base que já tinha os dados de demonstração da m3 mantém-nos.

   As credenciais emitidas antes da migration (sem a claim `ver`) deixam de ser aceites: todos os utilizadores iniciam sessão outra vez depois da publicação.

   `<db>`, `<user>` e `<palavra-passe>` lêem-se das variáveis `PGDATABASE`, `PGUSER` e `PGPASSWORD` do serviço `Postgres`. O `psql` usa as variáveis `PGHOST=127.0.0.1`, `PGPORT=55432`, `PGUSER`, `PGPASSWORD` e `PGDATABASE`.

6. **Publicar a API** a partir da pasta do passo 1: `railway up --service katch-backend --ci`. Desligar o repositório GitHub do serviço (*Settings*, *Source*, *Disconnect*), para que um push não substitua esta versão.
7. **Reiniciar a API depois das migrations** (`railway redeploy --service katch-backend -y`) se ela já estava a correr quando foram aplicadas. O Npgsql guarda em cache os tipos enumerados no arranque, e sem reinício os pedidos falham com `500` (`InvalidCastException`).
8. **Gerar o domínio público:** `railway domain --service katch-backend`.

## 4. Contas de avaliação

As contas de Candidato e de Recrutador criam-se pelos endpoints de registo (não dependem dos seeds, cuja palavra-passe de demonstração é a limitação L-09 da I106). A palavra-passe de cada conta tem de cumprir o P05 (8 ou mais caracteres, com uma letra e um algarismo) e é gerada na altura; só consta de `m4-credenciais-avaliacao-v02.md` (na m3, `m3-credenciais-avaliacao-v01.md`), fora dos repositórios. Todos os dados são fictícios.

| Passo | Pedido | Resultado esperado |
| --- | --- | --- |
| 1 | `POST /api/auth/candidates` (nome, correio, telefone de 9 algarismos, `locationId` de `GET /api/reference-lists/locations`, palavra-passe, `acceptTerms=true`) | `201` |
| 2 | `POST /api/auth/recruiters` (correio, palavra-passe, `acceptTerms=true`) | `201` |
| 3 | `POST /api/auth/login` do Recrutador, com `X-Client-App: web` | `200` com `token` |
| 4 | `POST /api/recruiter/company` com o `token` (designação, NIF de 9 algarismos, setor, morada, `locationId`, contacto, responsável) | `201`; a Empresa fica pendente |
| 5 | `POST /api/auth/login` do Administrador (`admin@katch.pt`, palavra-passe de `Katch__AdminPassword`), com `X-Client-App: web` | `200` com `token` |
| 6 | `GET /api/admin/companies/pending` e `POST /api/admin/companies/{companyId}/approve` com o `token` do Administrador | `204`; o Recrutador passa a poder gerir vagas |

O Administrador não se cria por endpoint: é a conta `admin@katch.pt` da migration `AddAdmin`, e a palavra-passe é a de `Katch__AdminPassword` no passo 5 da secção 3. A sequência completa, com os erros esperados, está no registo de verificação.

## 5. Verificação

| O quê | Como | Resultado esperado |
| --- | --- | --- |
| Migrations | `select count(*), max(migration_id) from "__EFMigrationsHistory";` | 11 e `20261121195748_AddLoginSecurity` |
| Colunas de `app_user` | `select column_name from information_schema.columns where table_name = 'app_user' and column_name in ('failed_login_count','locked_until','token_version');` | 3 linhas |
| Esquema e dados iniciais | `database/04-verify.sql` | 17 tabelas, `existe_notification` falso, 1 Administrador, 0 resumos fora do formato. Os volumes do P18 (20 Candidatos ativos, 5 Empresas aprovadas, 2 pendentes, 50 vagas publicadas) só se cumprem numa base com os dados de demonstração, que a publicação já não aplica |
| Vida e prontidão | `GET /health` e `GET /health/ready` | `200` com o corpo `Healthy`, sem credencial; `/health/ready` responde `503` (`Unhealthy`) sem ligação à base |
| Correlação | Qualquer resposta | Cabeçalho `X-Correlation-Id` presente |
| Anfitrião | Qualquer pedido ao domínio público | Resposta normal; `400` indica que `AllowedHosts` não tem o domínio |
| Swagger | `GET /swagger/v1/swagger.json` | `200`, com 62 caminhos (como na `v0.3.0`: a I136 não altera as rotas dos controllers); `404` sem `Swagger__Enabled=true` |
| Início de sessão | `POST /api/auth/login` com o cabeçalho `X-Client-App` | `200` com `token`; palavra-passe errada devolve `401` (`ERR-02`); mais de 10 inícios de sessão por IP num minuto devolvem `429` (`ERR-14`) com `Retry-After` |

O registo completo dos pedidos de sucesso e de erro está em `m3-registo-verificacao-url-publico-v01.md`. O log de build e de arranque está em `m3-registo-publicacao-railway-v01.md`.

## 6. Avisos do log

O log de build e de arranque (`m3-registo-publicacao-railway-v01.md`) tem avisos que não afetam o funcionamento da API:

| Aviso | Causa | Efeito |
| --- | --- | --- |
| `Cannot load library libgssapi_krb5.so.2` | O Npgsql tenta carregar a biblioteca de autenticação Kerberos (GSSAPI), que a imagem `aspnet` não inclui | Nenhum: a ligação à base usa palavra-passe, e a API liga-se e responde normalmente |
| `Failed to determine the https port for redirect` | O contentor escuta só em HTTP; o HTTPS termina no Railway | Nenhum: o acesso externo é sempre por HTTPS |
| `Storing keys in a directory ... DataProtection-Keys` | A API não configura persistência das chaves de proteção de dados | Nenhum: só usa JWT, que não depende dessas chaves |
| `Pedido recusado: {Method} {Path} -> {StatusCode} (conta {AccountId})` | Registo de segurança da I136: as respostas `401`, `403` e `429` ficam em `Warning`, só com o método, o caminho, o estado e o identificador da conta | Nenhum: não indica falha da API (um login falhado ou um limite excedido também aparecem) |
| `Compiling a query which loads related collections ... QuerySplittingBehavior` | Aviso do EF Core sobre consultas com várias coleções | Nenhum funcional; é do código da v0.3.0, que não se altera nesta publicação |

## 7. Limitações

As limitações da publicação estão em `m4-credenciais-avaliacao-v02.md`, secção 5. Com a I136, a limitação L-1 (Swagger só em `Development`) deixa de se aplicar, porque o Swagger depende de `Swagger__Enabled`. Mantêm-se os ficheiros carregados em disco local, o Administrador único, o crédito limitado do Railway e a cache dos tipos enumerados (L-2 a L-5). Acrescem os limites de pedidos e o bloqueio de início de sessão, as credenciais invalidadas pela migration `AddLoginSecurity` e o `AllowedHosts` restrito ao domínio do Railway (L-6 a L-8).
