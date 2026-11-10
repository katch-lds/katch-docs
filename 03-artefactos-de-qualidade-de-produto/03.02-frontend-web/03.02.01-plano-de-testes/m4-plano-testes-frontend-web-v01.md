# Plano de Testes do Frontend Web — Katch

**Unidade curricular:** Laboratório de Desenvolvimento de Software — LDS  
**Milestone:** `m4`  
**Ficheiro:** `m4-plano-testes-frontend-web-v01.md`  
**Pasta de arquivo:** `03-artefactos-de-qualidade-de-produto/03.02-frontend-web/03.02.01-plano-de-testes`

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
| Módulo | Frontend web (`katch-frontend-web`), área de gestão web do Recrutador e do Administrador, em React 19 com Vite 8 e TypeScript 6 |
| Issue | `I123` — Elaborar os planos de testes do frontend web e do frontend mobile |
| Executor / Revisor / Auditor | João Borguem / Miguel Santos / João Coelho |
| Documentos de origem | Regulamento de Funcionamento da UC (secção 19, pasta `03.02.01`); Regulamento Interno do Grupo v01 (secções 6.2, 10.4, 11.3 a 11.6 e 14.3); `m3-declaracao-ambito-v04.pdf`; `m3-especificacao-requisitos-v02.md`; `m3-modelo-classes-frontend-web-v02.md` |
| Documentos relacionados | `m3-plano-testes-backend-v01.md` (`03.01.01`); `m4-plano-testes-frontend-mobile-v01.md` (`03.03.01`); `m4-relatorio-evidencias-pipeline-ci-cd-frontends-v01.md` (`04.16-ci-cd`, Issue `I120`); `m4-registo-nao-conformidades-v02.xlsx` (NC-033) |

O documento segue a secção 19 do Regulamento de Funcionamento da Unidade Curricular para `03.02.01-plano-de-testes`: define o âmbito, a estratégia, o ambiente, os dados, as responsabilidades e os critérios de entrada e de saída dos testes do frontend web — testes unitários (Jest), testes de sistema automáticos (Playwright, `QG-06`) e simulação dos testes de aceitação — e a rastreabilidade de cada requisito ao teste que o verifica.

### 1.1. Histórico de versões

| Versão | Data | Descrição das alterações | Issue |
| --- | --- | --- | --- |
| v01 | 2026-11-06 | Criação do documento: objetivos, âmbito, níveis e técnicas de teste, ambiente, dados, cenários de sistema automáticos, simulação dos testes de aceitação, responsabilidades, âmbito da cobertura, critérios de entrada, de saída, de suspensão e de retoma, quality gates, rastreabilidade aos requisitos, entregáveis, riscos e referências. | `I123` |
| v01 (revisão) | 2026-11-09 | Correção da revisão da `I123`: responsável da pipeline segundo a secção 11.6 do Regulamento Interno (D01); tempo máximo de apresentação das mensagens novas nos cenários de conversas igual ao `P17` (M01). | `I123` |

Cada alteração posterior acrescenta uma linha. As versões anteriores são conservadas.

---

## 2. Objetivos

1. Verificar a lógica da área de gestão web — cliente da API, sessão, consulta periódica, hooks, validação dos formulários e componentes com lógica — independentemente da API e da rede (testes unitários Jest).
2. Atingir pelo menos 40 % de cobertura de linhas no âmbito definido na secção 10 (Regulamento Interno, secção 11.5) e falhar a pipeline abaixo desse limiar.
3. Verificar, com o sistema completo (frontend web, API e PostgreSQL), os fluxos do Recrutador e do Administrador da Declaração de Âmbito v04, de forma automática em cada Merge Request (testes de sistema Playwright, `QG-06`; Regulamento Interno, secção 11.6).
4. Demonstrar, por simulação dos testes de aceitação, que a área de gestão web satisfaz o âmbito aprovado e os critérios de aceitação dos requisitos da especificação v02.
5. Bloquear a integração na `main` sempre que um teste falha, a cobertura fica abaixo do limiar ou um quality gate não é cumprido.

## 3. Âmbito

### 3.1. Incluído

O âmbito é a coluna «Âmbito no frontend-web» da Declaração de Âmbito v04 e os requisitos da especificação v02 cujo campo «Componentes abrangidos» inclui o Frontend web (secção 13).

| Funcionalidade (DA v04) | Âmbito no frontend web | Requisitos | Níveis de teste |
| --- | --- | --- | --- |
| F001 — Autenticação e controlo de acesso | Início e fim de sessão do Recrutador e do Administrador, alteração da própria palavra-passe, consulta das contas e bloqueio e reativação pelo Administrador | RF037 a RF040, RF083 a RF087, RF106, RF107 | Unitário, sistema, aceitação |
| F002 — Registo e ativação de candidatos | Lista dos Candidatos registados pelo Administrador, com suspensão e reativação | RF088 a RF090 | Unitário, sistema, aceitação |
| F003 — Registo de empresas e aprovação administrativa | Registo da Empresa, estado do pedido e motivo de recusa, correção e nova submissão; página de apresentação, logótipo e galeria; aprovação, recusa, suspensão e reativação pelo Administrador | RF041 a RF049, RF091 a RF095, RF116 | Unitário, sistema, aceitação |
| F004 — Perfil do candidato e competências | Consulta do perfil profissional pelo Recrutador, com fotografia, hiperligações e curriculum vitae (no contexto da F007) | RF061, RF062 | Unitário, sistema, aceitação |
| F005 — Publicação e gestão de vagas | Criação, publicação, alteração, suspensão, nova publicação, encerramento e eliminação das vagas da Empresa; fotografias; consulta global pelo Administrador | RF050 a RF059, RF098, RF114, RF115 | Unitário, sistema, aceitação |
| F007 — Avaliação de candidatos interessados | Candidatos em espera por vaga, perfil completo, aceitação e recusa | RF060 a RF064, RF117 | Unitário, sistema, aceitação |
| F008 — Confirmação de match e disponibilização de contacto | Confirmação do match e consulta dos matches da Empresa com os dados de contacto | RF066 a RF068 | Unitário, sistema, aceitação |
| F009 — Supervisão da plataforma e indicadores | Painel de indicadores por período, listas globais, listas pré-definidas, área reservada ao Administrador | RF091, RF096 a RF104 | Unitário, sistema, aceitação |
| F010 — Comunicação entre as partes | Conversas do Recrutador: lista com mensagens por ler, histórico, envio, consulta periódica de 3 em 3 segundos, marcação como lidas, encerramento e modo apenas de consulta | RF068 a RF075, RF103, RF109 | Unitário, sistema, aceitação |
| Requisitos não funcionais do frontend web | Sem deslocamento horizontal entre 1280 e 1920 píxeis; indicação de falha de ligação em 10 segundos | RNF012, RNF018 | Unitário (RNF018), sistema, aceitação |

### 3.2. Excluído

| Elemento | Motivo | Verificação alternativa |
| --- | --- | --- |
| F006 — Exploração de vagas e manifestação de interesse | Sem interface na área web (DA v04, F006) | Plano de testes do frontend mobile |
| Registo do Candidato e criação ou alteração do perfil profissional (F002, F004) | Operações exclusivas da aplicação móvel (DA v04) | Plano de testes do frontend mobile |
| Requisitos só do Backend (RF065, RF112, RF113, RF118, RNF001, RNF003 a RNF009, RNF013, RNF015 a RNF017) | A regra é aplicada no servidor | Plano de testes do backend (`03.01.01`); o frontend web só apresenta o resultado (por exemplo, o motivo uniforme do RNF009 e a rejeição do RNF016), verificado nos cenários de sistema da secção 7 |
| Tempos de resposta da API e de carregamento de ficheiros (RNF001, RNF015) | Dependem do ambiente de execução e do servidor | Collection Postman do backend (P19) e simulação dos testes de aceitação no ambiente de demonstração (secção 8) |
| RF057 — Encerramento automático na data-limite | Executado pela tarefa periódica do backend | Testes do backend; o frontend web só apresenta o estado `encerrada`, verificado por teste unitário da listagem e na simulação dos testes de aceitação |
| Navegadores diferentes do Chromium nos testes automáticos | O ambiente de desenvolvimento e de demonstração do grupo usa navegadores Chromium; a DA v04 não garante o suporte a equipamentos fora desse ambiente | Simulação dos testes de aceitação no navegador usado na demonstração |
| Testes de carga, de segurança e de acessibilidade formal | Fora da DA v04 (limites transversais) | — |

## 4. Estratégia

### 4.1. Níveis de teste

| Nível | Ferramenta | Objeto | Duplos de teste | Execução |
| --- | --- | --- | --- | --- |
| Unitário | Jest 30 com `jest-environment-jsdom` e Testing Library (`@testing-library/react`, `@testing-library/jest-dom`) | Um módulo de cada vez: cliente da API, erros, monitor de ligação, sessão, consulta periódica, serviços, hooks, validação dos formulários, componentes e páginas com lógica | `IApiClient`, `fetch`, relógio e temporizadores (`jest.useFakeTimers`), serviços (nos hooks) e hooks (nas páginas) | Automática em cada Merge Request e localmente com `npm test` / `npm run test:coverage` |
| Sistema automático (`QG-06`) | Playwright (`@playwright/test`), Chromium | Fluxos completos da secção 7 no navegador, sobre o frontend construído (`vite build`), a API do `katch-backend` e PostgreSQL 18 | Nenhum no sistema; só nos cenários de falha de ligação (RNF018) o pedido é interrompido com `page.route` | Automática em cada Merge Request e localmente com `npx playwright test` |
| Simulação dos testes de aceitação | Guião de cenários de aceitação (secção 8) | A área de gestão web instalada no ambiente de demonstração | Nenhum | Uma vez no fecho de `m4`, depois de integradas as Issues de implementação (`I081`) |

### 4.2. Técnicas

- **Partições de equivalência e valores-limite** na validação local dos formulários, com os parâmetros da especificação v02: formatos P04 e P05, textos P08 (1000 e 1001 caracteres), hiperligações P10, motivo de recusa P12 (9, 10, 500 e 501 caracteres), galeria e fotografias P13 (6 e 7; 5 e 6), imagens P01 (5 MB e 5 MB + 1 byte; logótipo 2 MB), intervalo salarial da vaga (mínimo igual, inferior e superior ao máximo).
- **Transições de estado** apresentadas na interface: estado da Empresa (`pendente`, `aprovada`, `recusada`, `suspensa`), da vaga (`não publicada`, `publicada`, `suspensa`, `encerrada`), da conta (`ativa`, `bloqueada`, `suspensa`) e da conversa (ativa e modo apenas de consulta) — cada ação só está disponível nos estados em que a API a admite.
- **Tabelas de decisão** nas rotas protegidas: sessão iniciada ou não, tipo de conta (Recrutador ou Administrador) e estado da Empresa determinam a página apresentada (RF039, RF104, RF103).
- **Casos de erro da API**: para cada formulário, pelo menos um erro de validação devolvido pela API (`400`, catálogo ERR da documentação da API v02) apresentado no campo correspondente, e os erros `401`, `403`, `404` e `409` tratados pelo cliente da API.
- **Tempo e consulta periódica**: temporizadores falsos do Jest para o intervalo de 3 segundos (P17), para a ausência de pedidos sobrepostos e para o limite de tempo inferior a 10 segundos (RNF018).
- **Verificação de leiaute**: em cada página, nas larguras de 1280 e 1920 píxeis, `document.documentElement.scrollWidth` não excede `clientWidth` (RNF012, P21).

### 4.3. Duplos de teste

Nos testes unitários, a API nunca é contactada: o cliente da API é testado com um `fetch` simulado; os serviços com um duplo de `IApiClient`; os hooks com duplos dos serviços e da consulta periódica; as páginas com duplos dos hooks ou dos serviços. Os duplos são escritos à mão ou com `jest.fn()`, sem bibliotecas adicionais de _mocking_. A configuração é substituída pelo `test/envMock.ts` (mapeado no `jest.config.cjs`), porque `import.meta.env` só existe no Vite.

Nos testes de sistema não há duplos: o navegador fala com a API real. A única exceção é a interrupção controlada dos pedidos (`page.route(..., route => route.abort())` ou atraso superior ao limite) nos cenários de falha de ligação.

### 4.4. Regressão

Todos os testes unitários e de sistema correm em cada Merge Request para a `main` e em cada integração na `main` (pipeline `.github/workflows/ci.yml`, Issue `I120`). Cada Issue de implementação acrescenta os testes unitários da sua funcionalidade (Definition of Done e coluna 14 do Backlog de Projeto) e não pode integrar com testes a falhar nem com a cobertura abaixo de 40 %. Os cenários de sistema da secção 7 são automatizados na `I079` e passam a fazer parte da regressão a partir dessa Issue.

## 5. Ambiente

| Elemento | Testes unitários | Testes de sistema (`QG-06`) | Simulação dos testes de aceitação |
| --- | --- | --- | --- |
| Sistema operativo | `ubuntu-latest` (GitHub Actions); Windows 11 nas execuções locais | `ubuntu-latest` na pipeline; Windows 11 nas execuções locais | Equipamentos do grupo (ambiente de demonstração) |
| Node.js | 24 (`engines` do `package.json`; `NODE_VERSION` da pipeline) | 24 | 24, para construir o frontend |
| Frontend | Código-fonte (`src`) | Frontend construído com `npm run build` e servido com `vite preview` | Frontend construído, servido no ambiente de demonstração |
| Ferramentas de teste | Jest 30.5, `babel-jest` 30.5 com Babel 8, `jest-environment-jsdom` 30.5, `@testing-library/react` 16.3, `@testing-library/jest-dom` 7.0, `@testing-library/dom` 10.4 | `@playwright/test` com o navegador Chromium; versão fixada no `package.json` e no `package-lock.json` pela `I079` | Navegador Chromium (Google Chrome ou Microsoft Edge) nas larguras de 1280 e 1920 píxeis |
| Backend | Nenhum | `katch-backend` (ramo `main`, .NET 10, porto 5180) arrancado no próprio job | `katch-backend` no ambiente de demonstração ou no URL público da `I119` |
| Base de dados | Nenhuma | PostgreSQL 18 (serviço do job), com as migrations e os scripts `02` e `03` da pasta `database` | PostgreSQL 18 com os dados de demonstração (P18) |
| Configuração da API no frontend | `test/envMock.ts` | `VITE_API_BASE_URL` apontado para a API do job | `VITE_API_BASE_URL` do ambiente de demonstração |
| Segredos | Nenhum | `Jwt__Secret` e `Katch__AdminPassword` gerados aleatoriamente em cada execução e passados aos testes por variáveis de ambiente; nunca no repositório nem nos relatórios | Variáveis de ambiente da máquina; credenciais das contas de teste fora do repositório |

**Pipeline.** O job `build-and-test` corre `npm ci`, `npm run build` (`QG-01`), `npm run lint` e `npm run format:check` (`QG-05`), `npm run test:coverage -- --ci` (testes unitários com o limiar de 40 %) e publica o artefacto `coverage-report`. O passo `QG-06 Testes de sistema (Playwright)` já existe e só corre quando existe `playwright.config.ts`; a `I079` cria esse ficheiro e completa o passo com: o PostgreSQL 18 como serviço do job, o checkout do `katch-backend`, a aplicação das migrations e dos dados, o arranque da API e do `vite preview`, a instalação só do Chromium (`npx playwright install --with-deps chromium`) e a publicação do artefacto `playwright-report`. O job `static-analysis` corre o SonarQube com o Quality Gate «Katch QG-05».

**Configuração do Playwright** (a criar na `I079`): `testDir: 'e2e'`; projeto `chromium` com `viewport` de 1280 × 800; `reporter: [['list'], ['html', { open: 'never' }]]`; `use: { screenshot: 'only-on-failure', video: 'retain-on-failure', trace: 'retain-on-failure' }`; `retries: 0` na pipeline, para que um teste instável não passe por repetição; `workers: 1`, porque os cenários partilham a mesma base de dados.

## 6. Dados de teste

- Todos os dados são fictícios (Regulamento Interno, secção 14.3): é proibida a importação ou cópia de dados reais de utilizadores para os ambientes de desenvolvimento ou de teste.
- **Unitários:** cada teste constrói os DTOs de que precisa, com valores fixos e legíveis (por exemplo «Lumen Software, Lda.», NIF `509123456`, «Ana Ferreira»), e o instante fixado pelos temporizadores falsos. Não há dados partilhados entre testes.
- **Sistema:** a base de dados é recriada em cada execução da pipeline (migrations e scripts `02-seed-reference-data.sql` e `03-seed-demo-data.sql`). A conta de Administrador é a dos dados iniciais (`admin@katch.pt`), com a palavra-passe gerada para a execução. Cada cenário cria as contas, Empresas e vagas de que precisa através da própria interface ou por pedidos diretos à API (`request` do Playwright), com um sufixo aleatório no correio eletrónico e no NIF para não colidir com outros cenários. As ações do Candidato necessárias aos cenários do Recrutador (manifestar interesse, enviar mensagens) são feitas por pedido direto à API com uma conta de Candidato criada no cenário.
- **Ficheiros:** imagens JPEG e PNG pequenas, uma imagem de 5 MB, uma de 5 MB + 1 byte, um PDF mínimo e um ficheiro com a extensão `.png` e conteúdo de outro formato (RNF016), guardados em `e2e/fixtures`.
- **Aceitação:** dados de demonstração com os volumes mínimos do P18 (20 Candidatos ativos, 5 Empresas aprovadas, 2 Empresas pendentes, 50 vagas publicadas) e as contas temporárias de cada perfil; os dados que dependem do tempo (vaga com data-limite ultrapassada) são preparados por script antes da sessão.

## 7. Cenários de sistema automáticos (`QG-06`)

Cada cenário é um ficheiro ou um grupo `test.describe` do Playwright, identificado pelo código `SW-nn` no nome. A especificação detalhada (passos, dados e resultado esperado de cada caso) é produzida na `I079` em `03.02.02-testes-de-sistema`, a partir desta lista. Cada cenário inclui o caso de sucesso e os casos de erro indicados.

| Código | Cenário | Casos de sucesso e de erro | Requisitos |
| --- | --- | --- | --- |
| `SW-01` | Início e fim de sessão | Recrutador e Administrador entram e são encaminhados para a área do seu tipo de conta; palavra-passe errada e conta bloqueada mostram o motivo uniforme devolvido pela API; terminar sessão volta ao início de sessão e impede o regresso às páginas reservadas | RF038, RF083, RF106, RF107 |
| `SW-02` | Criação da conta de Recrutador e registo da Empresa | Registo válido deixa a Empresa no estado `pendente` e mostra o estado do pedido; correio eletrónico repetido, NIF com 8 algarismos e campo obrigatório vazio são rejeitados com o campo e o motivo | RF037, RF041 a RF044 |
| `SW-03` | Empresa não aprovada sem operações reservadas | Recrutador de Empresa pendente vê o aviso e não acede à gestão de vagas, aos candidatos nem às conversas, nem por URL direto | RF039 |
| `SW-04` | Aprovação e recusa da Empresa | Administrador consulta a lista de pendentes e os dados submetidos, aprova uma Empresa e recusa outra com motivo; motivo com 9 caracteres é rejeitado; o Recrutador recusado vê o motivo, corrige e submete de novo, voltando a `pendente` | RF044, RF045, RF091 a RF094 |
| `SW-05` | Página e dados da Empresa | Grava a página de apresentação, o logótipo e as fotografias da galeria e vê-os após recarregar; 7.ª fotografia da galeria, imagem de 5 MB + 1 byte e ficheiro com conteúdo de outro formato são rejeitados; alteração dos dados de registo com erro de validação mostra o erro | RF046 a RF049 |
| `SW-06` | Criação e publicação de vaga | Cria uma vaga com todos os campos, competências e benefícios das listas, fotografias e data-limite, e publica-a; intervalo salarial com mínimo superior ao máximo é rejeitado | RF050 a RF053 |
| `SW-07` | Ciclo de vida da vaga | Altera, suspende, volta a publicar e encerra uma vaga; elimina uma vaga sem interesses; a eliminação de uma vaga com interesses é impedida | RF054 a RF056, RF058, RF114, RF115 |
| `SW-08` | Vagas só da própria Empresa | A listagem mostra só as vagas da Empresa; o URL direto de uma vaga de outra Empresa apresenta acesso negado | RF059 |
| `SW-09` | Candidatos em espera e perfil completo | Lista por vaga os candidatos em espera, sem ações de decisão na lista; abre o perfil com fotografia, hiperligações e competências coincidentes destacadas; descarrega o curriculum vitae; vaga sem interessados mostra o estado vazio | RF060 a RF062 |
| `SW-10` | Decisão sobre o Candidato e match | Aceitar confirma o match e mostra os dados de contacto; recusar retira o Candidato da lista; a decisão registada não pode ser alterada | RF063, RF064, RF066, RF117 |
| `SW-11` | Matches da Empresa | A lista mostra só os matches da Empresa, com a vaga, o Candidato e o contacto; abrir um match leva à conversa criada automaticamente | RF067, RF068 |
| `SW-12` | Conversas do Recrutador | A lista indica as mensagens por ler; abrir a conversa marca-as como lidas e mostra o histórico ordenado; envia uma mensagem; uma mensagem enviada pelo Candidato por pedido direto aparece sem recarregar a página (consulta periódica de 3 em 3 segundos; a mensagem aparece no máximo 5 segundos depois do envio, P17); mensagem com 1001 caracteres é rejeitada | RF069 a RF073, RF109 |
| `SW-13` | Encerramento e modo apenas de consulta | Encerrar a conversa deixa o histórico visível e impede o envio; bloquear a conta do Candidato ou suspender a Empresa passa a conversa a modo apenas de consulta; sem match não há conversa | RF070, RF074, RF075 |
| `SW-14` | Contas e Candidatos (Administrador) | Consulta as contas, bloqueia e reativa uma conta; consulta a lista de Candidatos, suspende e reativa um Candidato; o Candidato suspenso não consegue iniciar sessão por pedido direto | RF085 a RF090 |
| `SW-15` | Empresas e vagas (Administrador) | Suspende e reativa uma Empresa aprovada; consulta a lista global de Empresas e a lista global de vagas publicadas | RF095, RF097, RF098, RF116 |
| `SW-16` | Listas pré-definidas | Acrescenta e altera a designação de uma competência e de um benefício; a designação repetida é rejeitada; a nova competência aparece na criação de vagas | RF099 a RF102 |
| `SW-17` | Painel de indicadores | Apresenta os indicadores do período escolhido; um período com a data inicial posterior à final é rejeitado | RF096 |
| `SW-18` | Área de supervisão e conversas | O Recrutador não acede às páginas do Administrador; o Administrador não tem acesso às conversas, nem por URL direto | RF103, RF104 |
| `SW-19` | Alteração da própria palavra-passe | Recrutador e Administrador alteram a palavra-passe e entram com a nova; palavra-passe atual errada e nova palavra-passe fora do P05 são rejeitadas | RF040, RF084 |
| `SW-20` | Sem deslocamento horizontal | Em cada página das secções 8.2 e 8.3 do modelo de classes, nas larguras de 1280 e 1920 píxeis, não há deslocamento horizontal | RNF012 |
| `SW-21` | Falha de ligação | Com os pedidos à API interrompidos ou sem resposta, a página indica a falha de ligação em 10 segundos, no máximo, a contar da ação | RNF018 |

## 8. Simulação dos testes de aceitação

A simulação dos testes de aceitação é feita na Issue `I081` (Sprint previsional S10) e é declarada como **simulação**: os participantes são elementos do grupo que representam o cliente, não utilizadores reais.

| Aspeto | Regra deste plano |
| --- | --- |
| Objeto | A área de gestão web integrada na `main`, com o backend da mesma data, no ambiente de demonstração |
| Cenários | Um cenário de aceitação por funcionalidade da secção 3.1 (`AW-F001`, `AW-F002`, `AW-F003`, `AW-F004`, `AW-F005`, `AW-F007`, `AW-F008`, `AW-F009`, `AW-F010`) e um para os requisitos não funcionais (`AW-RNF`), escritos como guião de utilização do Recrutador ou do Administrador e verificados contra o **critério de aceitação** de cada requisito da especificação v02 |
| Participantes | Um elemento no papel de Recrutador e outro no de Administrador, que não tenham sido Executores das Issues de implementação da funcionalidade em causa; um observador que regista os resultados; todos identificados no relatório |
| Medições | Tempos dos RNF medidos com cronómetro ou com as ferramentas do navegador (RNF015, RNF018); larguras de 1280 e 1920 píxeis (RNF012) |
| Registo | Para cada cenário: data e hora, versão (commit) testada, participantes, passos, resultado (aceite, aceite com desvio, não aceite), desvios observados e evidência (captura) |
| Desvios | Cada desvio é registado como defeito em `03.02.07-defeitos` e na Issue correspondente; um requisito obrigatório não aceite impede o critério de saída da secção 11.2 |
| Entregáveis | Especificação dos testes de aceitação e relatório da simulação em `03.02.03-testes-de-aceitacao` |

## 9. Responsabilidades

| Atividade | Responsável | Verificação |
| --- | --- | --- |
| Testes unitários de cada funcionalidade | Executor da Issue de implementação (Definition of Done e coluna 14 do Backlog): `I069` Miguel Santos; `I070` Roberto Baptista; `I071` Miguel Santos; `I072` Miguel Santos; `I121` Miguel Santos; `I122` João Coelho; `I073` e `I074` a atribuir no Sprint Planning do S08 | Revisor de cada Issue: `I069` João Coelho; `I070` Miguel Santos; `I071` João Borguem; `I072` Roberto Baptista; `I121` João Borguem; `I122` Roberto Baptista |
| Este plano de testes | João Borguem (`I123`) | Revisor Miguel Santos; Auditor João Coelho |
| Testes de sistema automáticos (Playwright), especificação e resultados | Executor da `I079` (a atribuir no Sprint Planning do S09) | Revisor e Auditor da `I079` |
| Simulação dos testes de aceitação, especificação e relatório | Executor da `I081` (a atribuir no Sprint Planning do S10) | Revisor e Auditor da `I081` |
| Análise estática (relatório SonarQube e ESLint) | Executor da `I124` | Revisor e Auditor da `I124` |
| Configuração e manutenção da pipeline e dos quality gates | João Borguem, responsável pela configuração e manutenção técnica da pipeline (Regulamento Interno, secção 11.6; configuração dos frontends na `I120`) | Revisor e Auditor de cada Issue que altera a pipeline |
| Verificação independente da configuração e dos resultados da pipeline | Líder em funções em `m4`, João Borguem (Regulamento Interno, secções 6.2 e 11.6) | — |
| Registo de defeitos encontrados pelos testes | Quem encontra o defeito, na Issue correspondente e em `03.02.07-defeitos` | Revisor da Issue |

O Executor, o Revisor e o Auditor de cada Issue são sempre alunos diferentes; o Executor nunca aprova nem integra o próprio Merge Request (Regulamento Interno, secção 10.4).

## 10. Âmbito da cobertura

| Medida | Código incluído | Código excluído e justificação | Limiar |
| --- | --- | --- | --- |
| Cobertura de linhas dos testes unitários do frontend web (Regulamento Interno, secção 11.5) | `src/**/*.{ts,tsx}`: páginas, componentes e hooks com lógica, serviços, cliente da API, sessão, rotas, consulta periódica e utilitários | `src/main.tsx` (arranque da aplicação), `src/config/env.ts` (leitura de `import.meta.env`, substituída nos testes), `src/types/**` e `*.d.ts` (tipos puros, sem código executável) | ≥ 40 % |

A cobertura é medida pelo Jest (`npm run test:coverage`, relatórios `text`, `lcov` e `json-summary`) e o limiar está em `coverageThreshold.global.lines` do `jest.config.cjs`: abaixo de 40 % o comando falha e a pipeline termina sem sucesso. Como o `collectCoverageFrom` lista todo o `src`, os ficheiros sem testes contam como não cobertos. O relatório é publicado no artefacto `coverage-report` e importado pelo SonarQube. A cobertura dos testes Playwright não é medida nem somada.

## 11. Critérios de entrada e de saída

### 11.1. Critérios de entrada

| Nível | Critérios |
| --- | --- |
| Unitário | Issue em execução no Sprint Backlog; página, hook ou serviço previsto no modelo de classes do frontend web v02 e operação documentada na documentação da API v02; o projeto compila (`npm run build`) |
| Sistema | Funcionalidade integrada na `main` do frontend web; endpoints usados integrados na `main` do backend; `playwright.config.ts` e o passo `QG-06` configurados (`I079`); base de dados recriada com as migrations e os dados |
| Aceitação | Todas as Issues de implementação do frontend web integradas; `QG-06` aprovado na `main`; ambiente de demonstração instalado com os dados do P18; participantes e observador designados |
| Pipeline | Merge Request aberto para a `main`; ramo atualizado com a `main` |

### 11.2. Critérios de saída

| Nível | Critérios |
| --- | --- |
| Unitário | 100 % dos testes executados sem falhas; cobertura de linhas ≥ 40 % no âmbito da secção 10; cada formulário com casos válidos, inválidos e de fronteira dos parâmetros aplicáveis |
| Sistema (`QG-06`) | 100 % dos testes executados sem falhas; os cenários `SW-01` a `SW-21` automatizados; relatório HTML do Playwright, com vídeo e capturas das falhas, publicado no artefacto `playwright-report` |
| Aceitação | Todos os cenários executados; todos os requisitos obrigatórios da secção 13 aceites; desvios registados como defeitos e tratados ou justificados no relatório |
| Pipeline | Build, ESLint, Prettier, testes unitários com a cobertura, testes de sistema e Quality Gate do SonarQube sem falhas na execução do Merge Request |

### 11.3. Suspensão e retoma

Os testes são suspensos quando o build falha, quando a pipeline não arranca, quando o backend do job não arranca ou quando um defeito bloqueia a maior parte dos casos de um nível (por exemplo, o início de sessão falha em todos os cenários de sistema). Retomam-se depois da correção, repetindo o nível inteiro sobre o novo commit; os resultados anteriores ficam como histórico e não são reaproveitados. Um teste de sistema instável é tratado como falha e corrigido; não se acrescentam repetições automáticas para o fazer passar.

## 12. Quality gates aplicáveis

| Código | Verificação | Critério | Evidência |
| --- | --- | --- | --- |
| `QG-01` | Construção | `npm run build` (`tsc -b` e `vite build`) sem erros | Registo do job `build-and-test` |
| `QG-05` | Linter e análise estática | ESLint sem ocorrências (`--max-warnings 0`), `prettier --check` sem diferenças e Quality Gate «Katch QG-05» do SonarQube aprovado | Registo dos jobs e artefacto `sonarqube-report` |
| `QG-06` | Testes de sistema automáticos | 100 % dos testes Playwright executados sem falhas | Relatório HTML do Playwright com vídeo e capturas das falhas (artefacto `playwright-report`) |
| Cobertura (secção 11.5) | Testes unitários e cobertura | 100 % dos testes Jest sem falhas e ≥ 40 % de linhas | Artefacto `coverage-report` e resumo da execução |

Os gates `QG-02`, `QG-03` e `QG-04` do Regulamento Interno (secção 11.6) aplicam-se só ao backend. A verificação da cobertura dos frontends não tem código próprio na secção 11.6, mas é bloqueante na pipeline, como os gates codificados. Todos os gates são efetivos: o incumprimento termina o job sem sucesso, e a regra de proteção da `main` exige os jobs `build-and-test` e `static-analysis` com sucesso antes do merge (evidências da `I120`).

## 13. Rastreabilidade aos requisitos

Legenda: **U** — teste unitário na Issue de implementação; **S** — cenário de sistema automático da secção 7; **A** — cenário de aceitação da secção 8.

| Requisito | Título | Issue de implementação | U | S | A |
| --- | --- | --- | --- | --- | --- |
| RF037 | Criar conta de Recrutador | `I069` | Sim | `SW-02` | `AW-F001` |
| RF038 | Iniciar sessão na área de gestão web (Recrutador) | `I069` | Sim | `SW-01` | `AW-F001` |
| RF039 | Impedir operações reservadas sem Empresa aprovada | `I069` | Sim | `SW-03` | `AW-F001` |
| RF040 | Alterar a própria palavra-passe (Recrutador) | `I069` | Sim | `SW-19` | `AW-F001` |
| RF041 | Registar a Empresa | `I069` | Sim | `SW-02` | `AW-F003` |
| RF042 | Impedir registo de Empresa sem validação automática | `I069` | Sim | `SW-02` | `AW-F003` |
| RF043 | Colocar a Empresa registada em estado pendente | `I069` | Sim | `SW-02` | `AW-F003` |
| RF044 | Consultar o estado do pedido de registo | `I069` | Sim | `SW-02`, `SW-04` | `AW-F003` |
| RF045 | Corrigir e submeter novamente o pedido | `I069` | Sim | `SW-04` | `AW-F003` |
| RF046 | Gravar a página de apresentação da Empresa | `I121` | Sim | `SW-05` | `AW-F003` |
| RF047 | Carregar o logótipo da Empresa | `I121` | Sim | `SW-05` | `AW-F003` |
| RF048 | Carregar fotografias da galeria da Empresa | `I121` | Sim | `SW-05` | `AW-F003` |
| RF049 | Alterar os dados de registo da Empresa aprovada | `I121` | Sim | `SW-05` | `AW-F003` |
| RF050 | Criar uma vaga | `I070` | Sim | `SW-06` | `AW-F005` |
| RF051 | Validar o intervalo salarial da vaga | `I070` | Sim | `SW-06` | `AW-F005` |
| RF052 | Carregar fotografias da vaga | `I070` | Sim | `SW-06` | `AW-F005` |
| RF053 | Publicar uma vaga | `I070` | Sim | `SW-06` | `AW-F005` |
| RF054 | Alterar uma vaga | `I070` | Sim | `SW-07` | `AW-F005` |
| RF055 | Suspender uma vaga | `I070` | Sim | `SW-07` | `AW-F005` |
| RF056 | Encerrar uma vaga | `I070` | Sim | `SW-07` | `AW-F005` |
| RF057 | Encerrar automaticamente a vaga na data-limite | `I070` (apresentação do estado) | Sim | — (secção 3.2) | `AW-F005` |
| RF058 | Impedir eliminação de vaga com interesses | `I070` | Sim | `SW-07` | `AW-F005` |
| RF059 | Restringir a gestão às vagas da própria Empresa | `I070` | Sim | `SW-08` | `AW-F005` |
| RF060 | Consultar candidatos em espera por vaga | `I071` | Sim | `SW-09` | `AW-F007` |
| RF061 | Abrir o perfil completo do Candidato | `I072` | Sim | `SW-09` | `AW-F004`, `AW-F007` |
| RF062 | Destacar competências coincidentes | `I072` | Sim | `SW-09` | `AW-F004`, `AW-F007` |
| RF063 | Aceitar um Candidato após abrir o perfil | `I072` | Sim | `SW-10` | `AW-F007` |
| RF064 | Recusar um Candidato após abrir o perfil | `I072` | Sim | `SW-10` | `AW-F007` |
| RF066 | Confirmar o match após aceitação | `I072` | Sim | `SW-10` | `AW-F008` |
| RF067 | Consultar os matches da Empresa | `I122` | Sim | `SW-11` | `AW-F008` |
| RF068 | Criar automaticamente a conversa do match | `I122` (apresentação) | Sim | `SW-11` | `AW-F008`, `AW-F010` |
| RF069 | Enviar mensagem ao Candidato | `I122` | Sim | `SW-12` | `AW-F010` |
| RF070 | Impedir comunicação sem match | `I122` | Sim | `SW-13` | `AW-F010` |
| RF071 | Consultar a lista de conversas (Recrutador) | `I122` | Sim | `SW-12` | `AW-F010` |
| RF072 | Consultar o histórico de uma conversa (Recrutador) | `I122` | Sim | `SW-12` | `AW-F010` |
| RF073 | Receber mensagens sem atualização manual (Recrutador) | `I122` | Sim | `SW-12` | `AW-F010` |
| RF074 | Encerrar uma conversa (Recrutador) | `I122` | Sim | `SW-13` | `AW-F010` |
| RF075 | Passar conversa a consulta por bloqueio ou suspensão | `I122` | Sim | `SW-13` | `AW-F010` |
| RF083 | Iniciar sessão na área de gestão web (Administrador) | `I069` | Sim | `SW-01` | `AW-F001` |
| RF084 | Alterar a própria palavra-passe (Administrador) | `I069` | Sim | `SW-19` | `AW-F001` |
| RF085 | Consultar as contas de utilizador | `I073` | Sim | `SW-14` | `AW-F001` |
| RF086 | Bloquear uma conta de utilizador | `I073` | Sim | `SW-14` | `AW-F001` |
| RF087 | Reativar uma conta bloqueada | `I073` | Sim | `SW-14` | `AW-F001` |
| RF088 | Consultar a lista de Candidatos | `I073` | Sim | `SW-14` | `AW-F002` |
| RF089 | Suspender a conta de um Candidato | `I073` | Sim | `SW-14` | `AW-F002` |
| RF090 | Reativar a conta de um Candidato suspensa | `I073` | Sim | `SW-14` | `AW-F002` |
| RF091 | Consultar as Empresas pendentes de aprovação | `I073` | Sim | `SW-04` | `AW-F003`, `AW-F009` |
| RF092 | Consultar os dados submetidos pela Empresa | `I073` | Sim | `SW-04` | `AW-F003` |
| RF093 | Aprovar o registo de uma Empresa | `I073` | Sim | `SW-04` | `AW-F003` |
| RF094 | Recusar o registo de uma Empresa com motivo | `I073` | Sim | `SW-04` | `AW-F003` |
| RF095 | Suspender uma Empresa aprovada | `I073` | Sim | `SW-15` | `AW-F003` |
| RF096 | Consultar o painel de indicadores por período | `I074` | Sim | `SW-17` | `AW-F009` |
| RF097 | Consultar a lista global de Empresas | `I073` | Sim | `SW-15` | `AW-F009` |
| RF098 | Consultar a lista global de vagas publicadas | `I073` | Sim | `SW-15` | `AW-F005`, `AW-F009` |
| RF099 | Acrescentar competência à lista pré-definida | `I073` | Sim | `SW-16` | `AW-F009` |
| RF100 | Alterar a designação de uma competência | `I073` | Sim | `SW-16` | `AW-F009` |
| RF101 | Acrescentar benefício à lista pré-definida | `I073` | Sim | `SW-16` | `AW-F009` |
| RF102 | Alterar a designação de um benefício | `I073` | Sim | `SW-16` | `AW-F009` |
| RF103 | Impedir o acesso do Administrador às conversas | `I122`, `I073` | Sim | `SW-18` | `AW-F009`, `AW-F010` |
| RF104 | Reservar a área de supervisão ao Administrador | `I073` | Sim | `SW-18` | `AW-F009` |
| RF106 | Terminar sessão na área de gestão web (Recrutador) | `I069` | Sim | `SW-01` | `AW-F001` |
| RF107 | Terminar sessão na área de gestão web (Administrador) | `I069`, `I073` | Sim | `SW-01` | `AW-F001` |
| RF109 | Marcar como lidas as mensagens ao abrir a conversa (Recrutador) | `I122` | Sim | `SW-12` | `AW-F010` |
| RF114 | Eliminar uma vaga sem interesses registados | `I070` | Sim | `SW-07` | `AW-F005` |
| RF115 | Voltar a publicar uma vaga suspensa | `I070` | Sim | `SW-07` | `AW-F005` |
| RF116 | Reativar uma Empresa suspensa | `I073` | Sim | `SW-15` | `AW-F003` |
| RF117 | Impedir a alteração de uma decisão registada | `I072` | Sim | `SW-10` | `AW-F007` |
| RNF012 | Apresentar a área de gestão web sem deslocamento horizontal | Todas as Issues do frontend web | — | `SW-20` | `AW-RNF` |
| RNF018 | Indicar falha de ligação na área web em 10 segundos | `I120` (cliente da API e monitor de ligação) e todas as páginas | Sim | `SW-21` | `AW-RNF` |

São 67 requisitos funcionais e 2 não funcionais. A Issue de cada requisito resulta da lista de requisitos da coluna 3 do Backlog de Projeto v07 ou, quando o requisito não consta dessa lista, da descrição e dos critérios de aceitação (coluna 14) da Issue: RF039 e RF083 (`I069`), RF060 (`I071`), RF061 a RF064, RF066 e RF117 (`I072`) e RF103 (`I122`). A `I126` (RTM de `m4`) regista nesta matriz os testes efetivamente executados.

## 14. Entregáveis de teste

| Entregável | Pasta | Issue |
| --- | --- | --- |
| `m4-plano-testes-frontend-web-v01.md` (este documento) | `03.02.01-plano-de-testes` | `I123` |
| Especificação e resultados dos testes de sistema automáticos (Playwright) | `03.02.02-testes-de-sistema` | `I079` |
| Especificação dos testes de aceitação e relatório da simulação | `03.02.03-testes-de-aceitacao` | `I081` |
| Resultados dos testes unitários (Jest) | `03.02.04-testes-unitarios` | Issue a criar num Backlog Refinement futuro (secção 15) |
| Relatório de cobertura dos testes unitários | `03.02.05-cobertura-de-testes` | Issue a criar num Backlog Refinement futuro (secção 15) |
| Relatório de análise estática | `03.02.06-analise-estatica` | `I124` |
| Registo de defeitos | `03.02.07-defeitos` | Quem encontra o defeito |
| Código dos testes (`src/**/*.test.ts(x)`, `e2e/`) | Repositório `katch-frontend-web` | Issues de implementação e `I079` |

Os nomes dos ficheiros seguem a nomenclatura oficial (`m4-<descrição>-v0N.<extensão>`) e são fixados nas Issues que os produzem.

## 15. Riscos e limitações

| Risco ou limitação | Efeito | Tratamento |
| --- | --- | --- |
| Os resultados dos testes unitários e o relatório de cobertura (`03.02.04` e `03.02.05`) ainda não têm Issue no Backlog de Projeto v07 | O artefacto pode ficar por produzir | Issues a criar num Backlog Refinement futuro; até lá, as evidências são o artefacto `coverage-report` e o resumo de cada execução da pipeline |

## 16. Referências

1. Regulamento de Funcionamento da Unidade Curricular de LDS 2026/2027, secção 19 (pastas `03.02.01` e `03.03.01`).
2. Regulamento Interno do Grupo G11, v01 (`m1-regulamento-grupo-v01.pdf`), secções 6.2, 10.4, 11.3, 11.4, 11.5, 11.6 e 14.3.
3. Declaração de Âmbito, v04 (`m3-declaracao-ambito-v04.pdf`), matriz do âmbito funcional (coluna «Âmbito no frontend-web») e limites transversais.
4. Especificação de Requisitos, v02 (`m3-especificacao-requisitos-v02.md`), secções 2.2, 3 (parâmetros P01 a P21), 4.2, 4.3 e 5.
5. Modelo de Classes do Frontend Web, v02 (`m3-modelo-classes-frontend-web-v02.md`), secções 2.1, 4, 6, 7 e 8.
6. Plano de Testes do Backend, v01 (`m3-plano-testes-backend-v01.md`).
7. Backlog de Projeto, v07 (`m4-backlog-projeto-v07.xlsx`) e Sprint Backlog do S07 (`m4-s07-sprint-backlog-v01.xlsx`): Issues `I069` a `I074`, `I079`, `I081`, `I120` a `I124` e `I126`.
8. Relatório de evidências da pipeline CI/CD dos frontends, v01 (`m4-relatorio-evidencias-pipeline-ci-cd-frontends-v01.md`, Issue `I120`), `.github/workflows/ci.yml` e `jest.config.cjs` do `katch-frontend-web`.
9. Registo de não conformidades, v02 (`m4-registo-nao-conformidades-v02.xlsx`), NC-033.
10. README do `katch-backend` (arranque da API, porto 5180, variáveis `ConnectionStrings__Katch`, `Jwt__Secret` e `Katch__AdminPassword`).
