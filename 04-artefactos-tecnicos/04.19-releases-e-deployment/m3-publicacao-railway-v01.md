# Publicação do backend no Railway — m3-v01

| Campo | Valor |
| --- | --- |
| Projeto | Katch — LDS, LEI, 2026/2027, turma LEI3T2, grupo 11 |
| Documento | `m3-publicacao-railway-v01.md` |
| Versão publicada | `v0.3.0`, commit `3f4a070946035440e8ad9c746b83a7b904478881` |
| URL público | `https://katch-backend-production.up.railway.app` |
| Issue | I119 |

## Histórico de versões

| Versão | Data | Alteração | Issue |
| --- | --- | --- | --- |
| v01 | 2026-11-04 | Primeira versão: procedimento de publicação no Railway. | I119 |

Este documento descreve como o backend foi publicado e como repetir o procedimento. Não contém segredos: os valores sensíveis só existem nas variáveis do serviço no Railway. O procedimento é retomado em m4 no guia de deployment (I085).

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

1. **Obter a versão exata.** Clone limpo da tag, numa pasta com caminho curto:

   ```powershell
   git clone --branch v0.3.0 --depth 1 https://github.com/katch-lds/katch-backend.git C:\k119
   git -C C:\k119 rev-parse HEAD     # tem de ser 3f4a070946035440e8ad9c746b83a7b904478881
   ```

2. **Acrescentar os ficheiros de build** à cópia (não ao repositório): `Dockerfile` e `.dockerignore` da pasta `m3-configuracao-publicacao-v01`. O repositório não tem Dockerfile, e o Railpack do Railway não reconhece a solução, que tem os `.csproj` em `src/`.
3. **Criar o projeto** no Railway, com o serviço `katch-backend`, e **adicionar a base**: `railway add --database postgres`.
4. **Definir as variáveis** do serviço `katch-backend` (nunca no repositório):

   | Variável | Valor |
   | --- | --- |
   | `ConnectionStrings__Katch` | `Host=${{Postgres.PGHOST}};Port=${{Postgres.PGPORT}};Database=${{Postgres.PGDATABASE}};Username=${{Postgres.PGUSER}};Password=${{Postgres.PGPASSWORD}}` (referências ao serviço `Postgres`) |
   | `Jwt__Secret` | valor aleatório com pelo menos 32 bytes, gerado na altura e escrito direto no Railway |
   | `ASPNETCORE_ENVIRONMENT` | `Development` (na v0.3.0 o Swagger só é ativado neste ambiente; ver a limitação L-1 de `m3-credenciais-avaliacao-v01.md`) |

   O `Dockerfile` ata a API à porta `$PORT` do Railway, por isso não é preciso definir `ASPNETCORE_URLS`.

5. **Aplicar as migrations e os dados iniciais** (antes de arrancar a API, ver o aviso do passo 7), por túnel:

   ```powershell
   railway connect Postgres --tunnel-only -P 55432       # noutro terminal; mantém o túnel aberto
   $env:ConnectionStrings__Katch = "Host=127.0.0.1;Port=55432;Database=<db>;Username=<user>;Password=<palavra-passe>"
   $env:Katch__AdminPassword = "<palavra-passe do Administrador>"   # 8+ caracteres, com letra e algarismo
   dotnet tool restore
   dotnet ef database update --project src/Katch.Backend
   psql -v ON_ERROR_STOP=1 -f database/02-seed-reference-data.sql
   psql -v ON_ERROR_STOP=1 -f database/03-seed-demo-data.sql
   psql -f database/04-verify.sql
   ```

   `<db>`, `<user>` e `<palavra-passe>` lêem-se das variáveis `PGDATABASE`, `PGUSER` e `PGPASSWORD` do serviço `Postgres`. O `psql` usa as variáveis `PGHOST=127.0.0.1`, `PGPORT=55432`, `PGUSER`, `PGPASSWORD` e `PGDATABASE`.

6. **Publicar a API** a partir da pasta do passo 1: `railway up --service katch-backend --ci`. Desligar o repositório GitHub do serviço (*Settings*, *Source*, *Disconnect*), para que um push não substitua esta versão.
7. **Reiniciar a API depois das migrations** (`railway redeploy --service katch-backend -y`) se ela já estava a correr quando foram aplicadas. O Npgsql guarda em cache os tipos enumerados no arranque, e sem reinício os pedidos falham com `500` (`InvalidCastException`).
8. **Gerar o domínio público:** `railway domain --service katch-backend`.

## 4. Verificação

| O quê | Como | Resultado esperado |
| --- | --- | --- |
| Migrations | `select count(*), max(migration_id) from "__EFMigrationsHistory";` | 10 e `20261024180929_AddMessageAndOperationLog` |
| Dados iniciais | `database/04-verify.sql` | 17 tabelas, 20 Candidatos ativos, 5 Empresas aprovadas, 2 pendentes, 50 vagas publicadas, 1 Administrador, 0 resumos fora do formato |
| Swagger | `GET /swagger/v1/swagger.json` | `200`, com 62 caminhos |
| Início de sessão | `POST /api/auth/login` com o cabeçalho `X-Client-App` | `200` com `token`; palavra-passe errada devolve `401` (`ERR-02`) |

O registo completo dos pedidos de sucesso e de erro está em `m3-registo-verificacao-url-publico-v01.txt`. O log de build e de arranque está em `m3-registo-publicacao-railway-v01.txt`.

## 5. Limitações

As limitações da publicação (Swagger em `Development`, ficheiros carregados em disco local, Administrador único, crédito limitado do Railway e a cache dos tipos enumerados) estão em `m3-credenciais-avaliacao-v01.md`, secção 5.
