# Especificação dos testes de sistema do frontend web — Katch

## 1. Identificação do documento

| Campo | Valor |
| --- | --- |
| Projeto | Katch |
| Grupo | G11 |
| Milestone | m4 |
| Issue | `I079` — Escrever os testes de sistema automáticos do frontend web (Playwright), Sprint 09 |
| Executor / Revisor / Auditor | João Borguem / Miguel Santos / Roberto Baptista |
| Documentos de origem | `m4-plano-testes-frontend-web-v01.md` (secções 5, 6, 7 e 12); `m3-especificacao-requisitos-v02.md` |
| Código dos testes | `katch-frontend-web`: `playwright.config.ts`, `e2e/sw-nn-*.spec.ts`, `e2e/support/katch.ts`, `e2e/README.md` |

### 1.1. Histórico de versões

| Versão | Data | Descrição das alterações | Issue |
| --- | --- | --- | --- |
| v01 | 2026-11-23 | Criação do documento: ambiente, dados e especificação dos cenários SW-01 a SW-21. Correções da revisão e da auditoria (2026-11-24): resultados com o código de erro, SW-06 com as fotografias da vaga e SW-09 com a fotografia, o destaque das competências e o CV enviados pelo teste. | `I079` |

## 2. Ambiente e execução

- Frontend construído com `VITE_API_BASE_URL` igual a `E2E_API_URL` (por omissão `http://localhost:5180`) e servido com `vite preview` no porto 4173, sem o proxy de desenvolvimento (CORS da API, `I130`).
- API do `katch-backend` (main) sobre PostgreSQL 18 com os scripts `01-create-schema.sql`, `02-seed-reference-data.sql` e `03-seed-demo-data.sql`.
- Playwright com Chromium (1280×800, `pt-PT`, `Europe/Lisbon`), um só worker, cenários por ordem; relatório HTML com vídeo, capturas e trace das falhas (`playwright-report`).
- Na pipeline, os passos `QG-06` do job `build-and-test` do `ci.yml`; uma falha faz falhar o job, que a regra de proteção da `main` exige com sucesso.
- As palavras-passe vêm de variáveis de ambiente (`E2E_DEMO_PASSWORD`, `E2E_ADMIN_PASSWORD`), nunca do repositório (RNF003).

## 3. Dados de teste

Cada cenário cria pela API os Recrutadores, as Empresas e as vagas que altera, com um sufixo único, para que a execução se repita sem repor a base. Usa as contas de demonstração `recrutador1` (Nortecode, para leitura), `admin@katch.pt` e `candidato09` a `candidato19` (interesses, matches, bloqueio e suspensão, repostos no fim quando aplicável). O SW-09 envia pela API da aplicação móvel a fotografia e o CV de `candidato10` e acrescenta-lhe a competência pretendida pela vaga, se ainda não a tiver.

## 4. Cenários

Cada cenário tem um ficheiro `e2e/sw-nn-*.spec.ts`, um `test.describe('SW-nn …')` e testes identificados por `SW-nnx`.

### SW-01 — Início e fim de sessão

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF038, RF083, RF106, RF107 |
| Precondições | Contas de demonstração ativas (recrutador1, admin); Recrutador novo bloqueado pela API. |
| Dados | recrutador1@demo.katch.pt; admin@katch.pt; palavra-passe errada; correio inexistente; Recrutador `sw01-…` bloqueado. |
| Passos | SW-01a: entrar como Recrutador em /entrar; terminar sessão; entrar como Administrador em /admin/entrar.<br>SW-01b: entrar com palavra-passe errada e com correio inexistente.<br>SW-01c: entrar com a conta bloqueada.<br>SW-01d: entrar, terminar sessão e abrir /vagas e /admin/indicadores por URL. |
| Resultados esperados | Recrutador em /vagas; Administrador em /admin/indicadores («Painel de indicadores»).<br>Aviso (`role=alert`) com o motivo uniforme; continua em /entrar.<br>Aviso com o motivo; não entra.<br>Volta a /entrar; as páginas reservadas levam ao início de sessão da área. |
| Testes | `e2e/sw-01-*.spec.ts` |

### SW-02 — Criação da conta de Recrutador e registo da Empresa

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF037, RF041 a RF044 |
| Precondições | Nenhuma. |
| Dados | Correio e nome únicos; NIF válido gerado (módulo 11); recrutador1 (correio repetido); NIF `51234567`. |
| Passos | SW-02a: criar conta em /criar-conta, preencher o registo da Empresa e «Pedir aprovação».<br>SW-02b: criar conta com o correio de recrutador1.<br>SW-02c: registo com NIF de 8 algarismos; depois com a morada vazia. |
| Resultados esperados | /empresa/estado com «A Empresa aguarda aprovação.» e estado «Pendente».<br>Erro do correio repetido; continua em /criar-conta.<br>Campo NIF e depois Morada marcados como inválidos; o pedido não é enviado. |
| Testes | `e2e/sw-02-*.spec.ts` |

### SW-03 — Empresa não aprovada sem operações reservadas

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF039 |
| Precondições | Recrutador com Empresa pendente (criado pela API). |
| Dados | Recrutador `sw03-…`. |
| Passos | Entrar e abrir por URL /vagas, /vagas/nova, /candidatos, /matches e /conversas. |
| Resultados esperados | Estado do pedido com «A Empresa não pode publicar vagas nem aceder a candidatos.»; nenhuma das páginas reservadas é apresentada. |
| Testes | `e2e/sw-03-*.spec.ts` |

### SW-04 — Aprovação e recusa da Empresa

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF044, RF045, RF091 a RF094 |
| Precondições | Duas Empresas pendentes criadas pela API. |
| Dados | Motivo com 9 caracteres; motivo válido. |
| Passos | SW-04a: Administrador abre os dados submetidos e aprova.<br>SW-04b: recusa com motivo de 9 caracteres e depois com motivo válido; o Recrutador vê o motivo, corrige a morada e submete de novo. |
| Resultados esperados | A Empresa sai da lista de pendentes.<br>Motivo curto rejeitado no campo; recusa registada; o Recrutador vê «Recusada» e o motivo; depois de submeter, «Pendente». |
| Testes | `e2e/sw-04-*.spec.ts` |

### SW-05 — Página e dados da Empresa

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF046 a RF049 |
| Precondições | Recrutador com Empresa aprovada (API). |
| Dados | PNG 1×1; ficheiro de 5 MB + 1 byte; texto com extensão .png. |
| Passos | SW-05a: gravar descrição e sítio; carregar logótipo e 6 fotografias; tentar a 7.ª; recarregar.<br>SW-05b: carregar a imagem de 5 MB + 1 byte e o ficheiro com conteúdo de outro formato.<br>SW-05c: apagar o nome do responsável e «Guardar alterações». |
| Resultados esperados | «Página guardada»; 6/6 fotografias; a 7.ª não entra; depois de recarregar, os dados mantêm-se.<br>Aviso de erro; a galeria continua 0/6.<br>Campo marcado como inválido. |
| Testes | `e2e/sw-05-*.spec.ts` |

### SW-06 — Criação e publicação de vaga

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF050 a RF053 |
| Precondições | Recrutador com Empresa aprovada. |
| Dados | Mínimo 1500 e máximo 1200; depois máximo 1900; competência, benefício, urgente, data-limite a 30 dias; fotografia PNG `vaga.png`. |
| Passos | Criar a vaga com mínimo superior ao máximo; corrigir; escolher a fotografia em «Adicionar fotografia»; guardar; publicar na lista; abrir a alteração da vaga. |
| Resultados esperados | «O valor mínimo não pode ser superior ao máximo.»; a fotografia aparece por enviar (1/5); ao guardar, o envio da fotografia é aceite e a vaga fica «Não publicada» na lista e «Publicada» depois de publicar; na alteração, a fotografia guardada é apresentada (1/5). |
| Testes | `e2e/sw-06-*.spec.ts` |

### SW-07 — Ciclo de vida da vaga

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF054 a RF056, RF058, RF114, RF115 |
| Precondições | Vagas publicadas criadas pela API; interesse de candidato09 numa delas. |
| Dados | Novo título. |
| Passos | SW-07a: editar o título; suspender; voltar a publicar; encerrar; eliminar outra vaga sem interesses.<br>SW-07b: eliminar a vaga com interesses. |
| Resultados esperados | Estados «Suspensa», «Publicada», «Encerrada»; a vaga eliminada desaparece.<br>«Não é possível eliminar a vaga»; a vaga mantém-se. |
| Testes | `e2e/sw-07-*.spec.ts` |

### SW-08 — Vagas só da própria Empresa

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF059 |
| Precondições | Duas Empresas aprovadas, cada uma com uma vaga. |
| Dados | — |
| Passos | Listar /vagas; abrir por URL a edição da vaga da outra Empresa. |
| Resultados esperados | Só a vaga da própria Empresa; a outra apresenta erro ou acesso negado. |
| Testes | `e2e/sw-08-*.spec.ts` |

### SW-09 — Candidatos em espera e perfil completo

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF060 a RF062 |
| Precondições | Vaga com interesse de candidato10, a quem o teste envia a fotografia e o CV e dá a competência pretendida pela vaga; vaga sem interesses. |
| Dados | Fotografia PNG e CV em PDF enviados pelo teste. |
| Passos | Abrir /candidatos com a vaga; abrir o perfil; abrir o CV; escolher a vaga sem interessados. |
| Resultados esperados | Um candidato, sem «Aceitar»/«Recusar» na lista; perfil com a fotografia, a competência que coincide com a vaga destacada (✓ e legenda «Competência que coincide com as pretendidas na vaga.»), hiperligações e «Abrir CV (PDF)»; o CV é devolvido com 200 em PDF; vaga vazia sem candidatos. |
| Testes | `e2e/sw-09-*.spec.ts` |

### SW-10 — Decisão sobre o Candidato e match

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF063, RF064, RF066, RF117 |
| Precondições | Vaga com interesses de candidato11 e candidato12. |
| Dados | — |
| Passos | Aceitar o primeiro; recusar o segundo; voltar ao perfil aceite. |
| Resultados esperados | «Match confirmado» com o contacto; recusado sai da lista («Candidato recusado…»); o perfil decidido não tem ações. |
| Testes | `e2e/sw-10-*.spec.ts` |

### SW-11 — Matches da Empresa

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF067, RF068 |
| Precondições | Match com candidato13 (API). |
| Dados | — |
| Passos | Abrir /matches; «Abrir conversa». |
| Resultados esperados | Só o match da Empresa, com a vaga e o contacto; abre /conversas?match=… com a caixa de mensagem ativa. |
| Testes | `e2e/sw-11-*.spec.ts` |

### SW-12 — Conversas do Recrutador

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF069 a RF073, RF109, P17 |
| Precondições | Match com candidato14 e mensagem do Candidato (API). |
| Dados | Mensagem de 1001 caracteres. |
| Passos | Abrir /conversas; abrir a conversa; enviar; o Candidato responde por pedido direto; escrever 1001 caracteres. |
| Resultados esperados | «1 por ler»; ao abrir, deixa de haver por ler e o histórico aparece por ordem; mensagem enviada no fim; resposta visível em ≤ 5 s sem recarregar; o campo limita a 1000 e a API recusa 1001 (400). |
| Testes | `e2e/sw-12-*.spec.ts` |

### SW-13 — Encerramento e modo apenas de consulta

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF070, RF074, RF075 |
| Precondições | Matches com candidato15, 16 e 19; interesse em espera de candidato18. |
| Dados | — |
| Passos | SW-13a: encerrar a conversa.<br>SW-13b: Administrador bloqueia candidato16; abrir a conversa; tentar a conversa de um interesse sem match.<br>SW-13c: Administrador suspende a Empresa; o Candidato consulta e tenta enviar. |
| Resultados esperados | «Conversa encerrada. Apenas consulta.», histórico visível, envio desativado.<br>Conversa só de consulta; sem match, a API recusa o envio e o ecrã não permite enviar.<br>Conversa «CLOSED» com «COMPANY_SUSPENDED»; envio recusado. |
| Testes | `e2e/sw-13-*.spec.ts` |

### SW-14 — Contas e Candidatos (Administrador)

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF085 a RF090 |
| Precondições | Recrutador criado pela API; candidato17. |
| Dados | — |
| Passos | SW-14a: pesquisar a conta, bloquear e reativar.<br>SW-14b: suspender e reativar candidato17; início de sessão por pedido direto. |
| Resultados esperados | «Bloqueada» e início de sessão recusado; «Ativa» e início de sessão aceite.<br>«Suspensa» e início de sessão recusado; «Ativa» e aceite. |
| Testes | `e2e/sw-14-*.spec.ts` |

### SW-15 — Empresas e vagas (Administrador)

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF095, RF097, RF098, RF116 |
| Precondições | Empresa aprovada com vaga publicada (API). |
| Dados | — |
| Passos | Consultar /admin/vagas; em /admin/empresas suspender e reativar a Empresa. |
| Resultados esperados | A vaga aparece com a Empresa; estados «Suspensa» e «Aprovada». |
| Testes | `e2e/sw-15-*.spec.ts` |

### SW-16 — Listas pré-definidas

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF099 a RF102 |
| Precondições | Nenhuma. |
| Dados | Competência e benefício com nome único. |
| Passos | Acrescentar e renomear uma competência e um benefício; repetir a competência; abrir a criação de vaga como Recrutador. |
| Resultados esperados | Itens na lista com o novo nome; a repetida é rejeitada; a competência renomeada aparece na criação de vagas. |
| Testes | `e2e/sw-16-*.spec.ts` |

### SW-17 — Painel de indicadores

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF096 |
| Precondições | Nenhuma. |
| Dados | Período dos últimos 60 dias; período invertido. |
| Passos | Aplicar o período; aplicar o período com a data inicial posterior à final. |
| Resultados esperados | Pedido 200 e indicadores apresentados; período invertido rejeitado sem pedido à API. |
| Testes | `e2e/sw-17-*.spec.ts` |

### SW-18 — Área de supervisão e conversas

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF103, RF104 |
| Precondições | Nenhuma. |
| Dados | — |
| Passos | SW-18a: Recrutador abre /admin/… por URL.<br>SW-18b: Administrador abre /conversas por URL. |
| Resultados esperados | Nenhuma página do Administrador é apresentada.<br>Sem ligação «Conversas» e sem a lista de conversas. |
| Testes | `e2e/sw-18-*.spec.ts` |

### SW-19 — Alteração da própria palavra-passe

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF040, RF084 |
| Precondições | Recrutador criado pela API; Administrador. |
| Dados | Palavra-passe atual errada; nova «curta» (fora do P05); nova válida. |
| Passos | SW-19a: alterar com a atual errada, com a nova inválida e com dados válidos.<br>SW-19b: o Administrador altera e a palavra-passe original é reposta no fim. |
| Resultados esperados | Erros indicados; «Palavra-passe alterada.»; a nova entra e a antiga não.<br>«Palavra-passe alterada.»; a nova entra. |
| Testes | `e2e/sw-19-*.spec.ts` |

### SW-20 — Sem deslocamento horizontal

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RNF012 |
| Precondições | Recrutador aprovado com vaga; recrutador1; Administrador. |
| Dados | Larguras de 1280 e 1920 px. |
| Passos | Abrir cada página sem sessão, do Recrutador e da supervisão (secções 8.2 e 8.3 do modelo de classes). |
| Resultados esperados | `scrollWidth` igual a `clientWidth` em todas as páginas e larguras. |
| Testes | `e2e/sw-20-*.spec.ts` |

### SW-21 — Falha de ligação

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RNF018 |
| Precondições | Sessão iniciada (Recrutador e Administrador). |
| Dados | Pedidos à API interrompidos ou sem resposta. |
| Passos | SW-21a: interromper os pedidos e abrir /matches.<br>SW-21b: deixar os pedidos sem resposta e abrir /admin/contas.<br>SW-21c: sem ligação, submeter o início de sessão. |
| Resultados esperados | «Não foi possível contactar o servidor…» em ≤ 10 s, nos três casos. |
| Testes | `e2e/sw-21-*.spec.ts` |

## 5. Rastreabilidade

| Cenário | Requisitos |
| --- | --- |
| `SW-01` | RF038, RF083, RF106, RF107 |
| `SW-02` | RF037, RF041 a RF044 |
| `SW-03` | RF039 |
| `SW-04` | RF044, RF045, RF091 a RF094 |
| `SW-05` | RF046 a RF049 |
| `SW-06` | RF050 a RF053 |
| `SW-07` | RF054 a RF056, RF058, RF114, RF115 |
| `SW-08` | RF059 |
| `SW-09` | RF060 a RF062 |
| `SW-10` | RF063, RF064, RF066, RF117 |
| `SW-11` | RF067, RF068 |
| `SW-12` | RF069 a RF073, RF109, P17 |
| `SW-13` | RF070, RF074, RF075 |
| `SW-14` | RF085 a RF090 |
| `SW-15` | RF095, RF097, RF098, RF116 |
| `SW-16` | RF099 a RF102 |
| `SW-17` | RF096 |
| `SW-18` | RF103, RF104 |
| `SW-19` | RF040, RF084 |
| `SW-20` | RNF012 |
| `SW-21` | RNF018 |

## 6. Limitações

- O SW-13 verifica a suspensão da Empresa pelo lado do Candidato (pedido direto), porque o Recrutador da Empresa suspensa deixa de ter a área da Empresa.
- O limite de 1001 caracteres do SW-12 é verificado no campo (máximo de 1000) e na API (400), porque o campo não deixa escrever mais.
- Os interesses gastam a quota diária das contas `candidato09` a `candidato19`; em execuções repetidas no mesmo dia pode ser preciso repor a base.
