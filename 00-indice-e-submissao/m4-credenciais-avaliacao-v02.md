# Credenciais e acesso para avaliação do backend — m4

| Campo | Valor |
| --- | --- |
| Projeto | Katch — LDS, LEI, 2026/2027, turma LEI3T2, grupo 11 |
| Documento | `m4-credenciais-avaliacao-v02.md` |
| Componente | Backend (API ASP.NET Core e base de dados PostgreSQL), publicado no Railway |
| Versão publicada | **m4** — Merge Commit `796f81e99ef9d955eab4d27095443accf821f171` na main (PR #47, Issue I136, 2026-11-23); tag Git a indicar quando for criada |
| Issue | I137 (versão anterior: I119) |

> **Documento com credenciais temporárias de avaliação.** Não o publique em repositórios públicos nem o reencaminhe. As contas existem só para avaliar a milestone m4 e serão removidas ou terão a palavra-passe alterada depois da avaliação.

## Histórico de versões

| Versão | Data | Alteração | Issue |
| --- | --- | --- | --- |
| v01 | 2026-11-04 | Primeira versão: URL público, contas de avaliação e instruções de acesso (backend m3-v01, tag `v0.3.0`). Conservada em `m3-credenciais-avaliacao-v01.md`. | I119 |
| v02 | 2026-11-23 | Atualização ao backend com as correções da I136: palavras-passe das contas a preencher (secção 2); limites de pedidos e bloqueio de início de sessão (secções 3.1 e 5); verificações de saúde (secção 1); período máximo dos indicadores (secção 3.3); base sem os dados de demonstração (secção 4); limitação L-1 retirada e novas L-6 a L-8 (secção 5). | I137 |

## 1. URL público

| O quê | Endereço |
| --- | --- |
| API (base) | `https://katch-backend-production.up.railway.app` |
| Swagger (documentação interativa) | `https://katch-backend-production.up.railway.app/swagger` |
| Especificação OpenAPI (JSON) | `https://katch-backend-production.up.railway.app/swagger/v1/swagger.json` |
| Estado da API | `https://katch-backend-production.up.railway.app/health` (`200` com `Healthy`) |
| Estado da API e da base de dados | `https://katch-backend-production.up.railway.app/health/ready` (`200` com `Healthy`; `503` com a base indisponível) |

O acesso é por HTTPS e não é preciso instalar nada. Não há página inicial em `/`: a API só tem os caminhos listados no Swagger e os dois de estado.

## 2. Contas de avaliação

Todas as contas e todos os dados são fictícios. Há uma conta por perfil. As palavras-passe são as das contas criadas na publicação m4 (procedimento em `m4-publicacao-railway-v02.md`, secção 4) e são preenchidas aqui no momento da entrega.

| Perfil | Correio eletrónico | Palavra-passe | Ponto de acesso (`X-Client-App`) |
| --- | --- | --- | --- |
| Candidato | `candidato.avaliacao@demo.katch.pt` | `<a preencher>` | `mobile` |
| Recrutador (Empresa «Empresa de Avaliacao Lda», já aprovada) | `recrutador.avaliacao@demo.katch.pt` | `<a preencher>` | `web` |
| Administrador | `admin@katch.pt` | `<a preencher: valor de Katch__AdminPassword>` | `web` |

Cada palavra-passe tem 8 ou mais caracteres, com uma letra e um algarismo, e não pode ser uma palavra-passe comum (por exemplo `password1`), que a API recusa.

Limites de cada conta:

- **Candidato** e **Recrutador:** só têm as permissões do respetivo perfil. O Recrutador só gere a sua Empresa e as suas vagas; o Candidato só vê as vagas e a sua quota de interesses.
- **Administrador:** a API só tem um papel de Administrador, sem forma de limitar os seus privilégios. A conta `admin@katch.pt` é a conta de Administrador do sistema, com uma palavra-passe exclusiva desta avaliação e que será alterada depois dela. Veja a limitação L-3.

As credenciais emitidas antes da publicação m4 deixaram de ser aceites: é preciso iniciar sessão outra vez, mesmo com uma conta que já existisse.

## 3. Como aceder

### 3.1 No Swagger

1. Abra `https://katch-backend-production.up.railway.app/swagger`.
2. Em `POST /api/auth/login`, escolha **Try it out**, preencha o cabeçalho `X-Client-App` com o ponto de acesso da conta (tabela da secção 2) e o corpo com o correio e a palavra-passe:

   ```json
   { "email": "candidato.avaliacao@demo.katch.pt", "password": "<palavra-passe da secção 2>" }
   ```

3. Execute e copie o valor de `token` da resposta (`200`).
4. Clique em **Authorize** (cadeado, no topo), cole o `token` **sem** o prefixo `Bearer` e confirme.
5. Experimente os endpoints do perfil. As credenciais expiram ao fim de 8 horas. Terminar a sessão (`POST /api/auth/logout`) ou alterar a palavra-passe termina também todas as credenciais da conta, e é preciso repetir o início de sessão.

Atenção aos limites: cinco inícios de sessão falhados seguidos bloqueiam a conta durante 1 minuto (o prazo dobra a cada novo bloqueio, até 15 minutos), e a resposta é a mesma de uma palavra-passe errada, mesmo com a palavra-passe certa. Mais de 10 inícios de sessão por minuto, do mesmo IP, devolvem `429`. Se acontecer, espere o tempo do cabeçalho `Retry-After`.

### 3.2 Com curl

```text
curl -s -X POST https://katch-backend-production.up.railway.app/api/auth/login \
  -H "Content-Type: application/json" -H "X-Client-App: mobile" \
  -d '{"email":"candidato.avaliacao@demo.katch.pt","password":"<palavra-passe>"}'

curl -s https://katch-backend-production.up.railway.app/api/candidate/jobs/quota \
  -H "Authorization: Bearer <token>"
```

### 3.3 Sugestão de percurso de avaliação

| Perfil | Pedidos |
| --- | --- |
| Candidato | `GET /api/candidate/jobs/next` (listagem de vagas), `GET /api/candidate/jobs/quota`, `POST /api/candidate/jobs/{jobId}/interest` |
| Recrutador | `GET /api/recruiter/company/status`, `GET /api/recruiter/jobs`, `POST /api/recruiter/jobs` |
| Administrador | `GET /api/admin/companies/pending`, `GET /api/admin/indicators?from=aaaa-mm-dd&to=aaaa-mm-dd` (o `to` não pode ser posterior à data atual e o período não pode exceder 366 dias) |
| Sem conta | `POST /api/auth/candidates` e `POST /api/auth/recruiters` (registo), `GET /api/reference-lists/locations`, `GET /health` e `GET /health/ready` |

Erros esperados, para verificar a validação: palavra-passe errada (`401`, `ERR-02`), cabeçalho `X-Client-App` em falta (`400`), conta Candidato no ponto de acesso `web` (`403`, `ERR-05`), pedido sem credencial (`401`, `ERR-01`), rota de outro perfil (`403`, `ERR-06`), credencial de antes de terminar a sessão (`401`, `ERR-01`), palavra-passe comum no registo (`400`, `ERR-10`), rota inexistente com sessão (`404`, `ERR-20`) e mais de 10 inícios de sessão por minuto do mesmo IP (`429`, `ERR-14`).

## 4. Dados publicados

A base de dados é uma PostgreSQL 18 gerida no Railway, criada com as 11 migrations do backend (a última é `20261121195748_AddLoginSecurity`) e preenchida com o script `database/02-seed-reference-data.sql`. Tem 17 tabelas e as contas da secção 2. O script `03-seed-demo-data.sql` já não se aplica no Railway (só corre com `-v demo=1`, porque as contas de demonstração partilham uma palavra-passe conhecida do grupo), por isso a publicação não traz os volumes do P18 (20 Candidatos, 5 Empresas aprovadas, 2 pendentes e 50 vagas). Uma base que já tinha os dados de demonstração da m3 mantém-nos. Todos os dados são fictícios.

## 5. Limitações

| Id | Limitação | Efeito |
| --- | --- | --- |
| L-1 | Retirada na m4. Na m3 o backend corria com `ASPNETCORE_ENVIRONMENT=Development`, porque o Swagger só se ativava nesse ambiente. | Com a I136, o backend corre com `ASPNETCORE_ENVIRONMENT=Production` e o Swagger publica-se com `Swagger__Enabled=true`. As respostas `500` já não incluem detalhe técnico. |
| L-2 | Os ficheiros carregados (logótipos e fotografias) ficam no disco do contentor e perdem-se quando o serviço é publicado de novo. | Os dados iniciais não referenciam ficheiros, por isso não se perde nada que a avaliação precise. Basta carregar o ficheiro outra vez. |
| L-3 | Só existe um papel de Administrador, sem privilégios limitáveis. | A avaliação usa `admin@katch.pt` com uma palavra-passe temporária, a alterar depois. |
| L-4 | O serviço corre num plano de crédito limitado do Railway. | A disponibilidade depende do crédito restante; o grupo acompanha o consumo. |
| L-5 | Os tipos enumerados da base de dados são lidos pela API no arranque e ficam em cache (Npgsql). Se as migrations forem aplicadas ou alteradas com a API já em execução, os pedidos que usam esses tipos falham com `500` (`InvalidCastException`). | Depois de qualquer migration, reiniciar o serviço (`railway redeploy`). |
| L-6 | Os limites de pedidos e o bloqueio de início de sessão (I136) contam por IP e por conta. | Quem errar a palavra-passe 5 vezes seguidas fica 1 minuto sem poder iniciar sessão, e mais de 10 inícios de sessão por minuto do mesmo IP recebem `429`. Esperar o tempo de `Retry-After`. |
| L-7 | A publicação m4 aplica a migration `AddLoginSecurity`, que invalida as credenciais emitidas antes. | Todos os utilizadores iniciam sessão outra vez uma vez. |
| L-8 | O `AllowedHosts` aceita só o domínio do Railway. | Um pedido com outro cabeçalho `Host` recebe `400`. Acesso apenas pelo URL da secção 1. |

## 6. Disponibilidade

A API e a base de dados estão publicadas e verificadas antes do fecho da submissão de m4 e mantêm-se acessíveis durante o período de avaliação. Se o URL não responder, contacte o grupo 11. A data limite da submissão de m4 não consta deste documento e deve ser indicada na entrega.

## 7. Verificação

O registo dos pedidos de sucesso e de erro feitos através do URL público é arquivado em `projeto/04-artefactos-tecnicos/04.19-releases-e-deployment/`. O da m3 é `m3-registo-verificacao-url-publico-v01.md` (Issue I119); o da m4 ainda não existe.
