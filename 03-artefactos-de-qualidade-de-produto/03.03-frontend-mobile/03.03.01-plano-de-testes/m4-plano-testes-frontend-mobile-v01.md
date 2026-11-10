# Plano de Testes do Frontend Mobile — Katch

**Unidade curricular:** Laboratório de Desenvolvimento de Software — LDS  
**Milestone:** `m4`  
**Ficheiro:** `m4-plano-testes-frontend-mobile-v01.md`  
**Pasta de arquivo:** `03-artefactos-de-qualidade-de-produto/03.03-frontend-mobile/03.03.01-plano-de-testes`

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
| Módulo | Frontend mobile (`katch-frontend-mobile`), aplicação móvel do Candidato, em Flutter (Dart 3) para Android |
| Issue | `I123` — Elaborar os planos de testes do frontend web e do frontend mobile |
| Executor / Revisor / Auditor | João Borguem / Miguel Santos / João Coelho |
| Documentos de origem | Regulamento de Funcionamento da UC (secção 19, pasta `03.03.01`); Regulamento Interno do Grupo v01 (secções 6.2, 10.4, 11.3 a 11.6 e 14.3); `m3-declaracao-ambito-v04.pdf`; `m3-especificacao-requisitos-v02.md`; `m3-modelo-classes-frontend-mobile-v02.md` |
| Documentos relacionados | `m3-plano-testes-backend-v01.md` (`03.01.01`); `m4-plano-testes-frontend-web-v01.md` (`03.02.01`); `m4-relatorio-evidencias-pipeline-ci-cd-frontends-v01.md` (`04.16-ci-cd`, Issue `I120`); `m4-registo-nao-conformidades-v02.xlsx` (NC-033) |

O documento segue a secção 19 do Regulamento de Funcionamento da Unidade Curricular para `03.03.01-plano-de-testes`: define o âmbito, a estratégia, o ambiente, os dados, as responsabilidades e os critérios de entrada e de saída dos testes do frontend mobile — testes unitários e de widget (`flutter_test`), testes de sistema automáticos (`integration_test`, `QG-07`) e simulação dos testes de aceitação — e a rastreabilidade de cada requisito ao teste que o verifica.

### 1.1. Histórico de versões

| Versão | Data | Descrição das alterações | Issue |
| --- | --- | --- | --- |
| v01 | 2026-11-06 | Criação do documento: objetivos, âmbito, níveis e técnicas de teste, ambiente, dados, cenários de sistema automáticos, simulação dos testes de aceitação, responsabilidades, âmbito da cobertura, critérios de entrada, de saída, de suspensão e de retoma, quality gates, rastreabilidade aos requisitos, entregáveis, riscos e referências. | `I123` |
| v01 (revisão) | 2026-11-09 | Correção da revisão da `I123`: responsável da pipeline segundo a secção 11.6 do Regulamento Interno (D01); tempo máximo de apresentação das mensagens novas nos cenários de conversas igual ao `P17` (M01). | `I123` |

Cada alteração posterior acrescenta uma linha. As versões anteriores são conservadas.

---

## 2. Objetivos

1. Verificar a lógica da aplicação móvel — cliente da API, sessão, consulta periódica, serviços, controllers de estado, validação local (`InputRules`) e widgets — independentemente da API e da rede (testes unitários e de widget).
2. Atingir pelo menos 40 % de cobertura de linhas no âmbito definido na secção 10 (Regulamento Interno, secção 11.5) e falhar a pipeline abaixo desse limiar.
3. Verificar, com o sistema completo (aplicação num emulador Android, API e PostgreSQL), os fluxos do Candidato da Declaração de Âmbito v04, de forma automática em cada Merge Request (testes de sistema `integration_test`, `QG-07`; Regulamento Interno, secção 11.6).
4. Demonstrar, por simulação dos testes de aceitação, que a aplicação móvel satisfaz o âmbito aprovado e os critérios de aceitação dos requisitos da especificação v02.
5. Bloquear a integração na `main` sempre que um teste falha, a cobertura fica abaixo do limiar ou um quality gate não é cumprido.

## 3. Âmbito

### 3.1. Incluído

O âmbito é a coluna «Âmbito no frontend-mobile» da Declaração de Âmbito v04 e os requisitos da especificação v02 cujo campo «Componentes abrangidos» inclui o Frontend mobile (secção 13).

| Funcionalidade (DA v04) | Âmbito no frontend mobile | Requisitos | Níveis de teste |
| --- | --- | --- | --- |
| F001 — Autenticação e controlo de acesso | Início e fim de sessão do Candidato e alteração da própria palavra-passe | RF001, RF002, RF105 | Unitário, sistema, aceitação |
| F002 — Registo e ativação de candidatos | Formulário de registo, com a localidade da lista pré-definida, apresentação dos erros da validação automática e confirmação da ativação | RF003 a RF005 | Unitário, sistema, aceitação |
| F003 — Registo de empresas e aprovação administrativa | Consulta da página de apresentação da Empresa a partir do detalhe da vaga | RF017 | Unitário, sistema, aceitação |
| F004 — Perfil do candidato e competências | Criação e alteração do perfil: dados, preferências de procura, competências da lista e opção «Outro», fotografia, hiperligações e curriculum vitae | RF006 a RF012 | Unitário, sistema, aceitação |
| F006 — Exploração de vagas e manifestação de interesse | Cartões de vaga segundo as preferências, distância, detalhe, recusa, manifestação de interesse, quota e bloqueio, ajuste das preferências na área de exploração | RF013 a RF023 | Unitário, sistema, aceitação |
| F008 — Confirmação de match e disponibilização de contacto | Lista dos matches e dados de contacto depois do match | RF024, RF025, RF068 | Unitário, sistema, aceitação |
| F010 — Comunicação entre as partes | Conversas do Candidato: lista com mensagens por ler, histórico, envio, consulta periódica de 3 em 3 segundos, marcação como lidas, encerramento e modo apenas de consulta | RF026 a RF030, RF068, RF070, RF075, RF108 | Unitário, sistema, aceitação |
| Requisitos não funcionais do frontend mobile | Cartão seguinte em 2 segundos; uma interação por ação sobre o cartão; áreas a 3 interações; falha de ligação em 10 segundos | RNF002, RNF010, RNF011, RNF014 | Unitário (RNF014), sistema, aceitação |

### 3.2. Excluído

| Elemento | Motivo | Verificação alternativa |
| --- | --- | --- |
| F005, F007 e F009 | Sem interface na aplicação móvel (DA v04) | Plano de testes do frontend web |
| Administração de contas, registo e aprovação de Empresas | Exclusivos da área de gestão web (DA v04, F001 e F003) | Plano de testes do frontend web |
| Requisitos só do Backend (RF065, RF112, RF113, RF118, RNF001, RNF003 a RNF009, RNF013, RNF015 a RNF017) | A regra é aplicada no servidor | Plano de testes do backend (`03.01.01`); a aplicação só apresenta o resultado (por exemplo, a rejeição de um ficheiro pelo RNF016), verificado nos cenários de sistema da secção 7 |
| iOS | O ambiente de desenvolvimento e de demonstração do grupo usa Android; a DA v04 não garante o suporte a equipamentos fora desse ambiente | — |
| Funcionamento sem ligação à rede | Fora da DA v04 (limites transversais) | Só a indicação da falha de ligação (RNF014) |
| Testes de carga, de segurança e de acessibilidade formal | Fora da DA v04 | — |

## 4. Estratégia

### 4.1. Níveis de teste

| Nível | Ferramenta | Objeto | Duplos de teste | Execução |
| --- | --- | --- | --- | --- |
| Unitário e de widget | `flutter_test` (SDK do Flutter) | Unitário: modelos, cliente da API, `ApiException`, `InputRules`, sessão, consulta periódica, serviços e controllers de estado (`ChangeNotifier`). Widget: cada ecrã da secção 9.3 do modelo de classes, com os estados de carregamento, de erro, de lista vazia e de sucesso | `ApiClient` (interface do Núcleo), `http.Client` (`MockClient` do pacote `http`), `Clock`, seletor de ficheiros e abertura de aplicações externas (interfaces de `core/device`), serviços (nos controllers) | Automática em cada Merge Request e localmente com `flutter test --coverage` |
| Sistema automático (`QG-07`) | `integration_test` (SDK do Flutter) | Fluxos completos da secção 7 na aplicação num emulador Android, sobre a API do `katch-backend` e PostgreSQL 18 | Só o seletor de ficheiros (interface `core/device`), substituído por um que devolve os ficheiros de teste, porque o seletor nativo não é controlável pelo teste | Automática em cada Merge Request (`reactivecircus/android-emulator-runner`) e localmente com `flutter test integration_test` num emulador ou num dispositivo |
| Simulação dos testes de aceitação | Guião de cenários de aceitação (secção 8) | A aplicação instalada num dispositivo ou emulador Android, ligada ao backend do ambiente de demonstração | Nenhum | Uma vez no fecho de `m4`, depois de integradas as Issues de implementação (`I081`) |

### 4.2. Técnicas

- **Partições de equivalência e valores-limite** na validação local (`InputRules`) e nos formulários, com os parâmetros da especificação v02: P04 (telefone com 8, 9 e 10 algarismos; correio eletrónico sem `@`), P05 (7 e 8 caracteres; sem algarismo), P06 (0, 1, 500 e 501 km), P07 (1, 2, 40 e 41 caracteres; o carácter `@`), P08 (1000 e 1001 caracteres), P10 (3 e 4 hiperligações; sem `http://` ou `https://`), P01 e P02 (5 MB e 5 MB + 1 byte; formato não admitido), P03 (0, 1, 1000 e 1001 caracteres).
- **Transições de estado** apresentadas na aplicação: quota de interesses (disponível, esgotada com indicação da reposição, reposta), interesse (em espera, match), conversa (ativa e modo apenas de consulta) e sessão (iniciada, expirada, terminada).
- **Tabelas de decisão** na navegação (`AuthGate`): sessão iniciada ou não, credencial expirada e perfil por completar determinam o ecrã apresentado.
- **Casos de erro da API**: para cada formulário, pelo menos um erro de validação devolvido pela API (`400`, catálogo ERR da documentação da API v02) apresentado no campo correspondente, e os erros `401`, `403`, `404` e `409` tratados pelo cliente da API.
- **Tempo**: relógio fixo (`Clock`) para a reposição da quota (24 horas, P11) e `fakeAsync` para a consulta periódica de 3 segundos (P17), para a ausência de pedidos sobrepostos e para o limite de tempo de 9 segundos do cliente (RNF014).
- **Contagem de interações**: nos testes de sistema, o número de toques ou gestos (`tester.tap`, `tester.fling`) entre o ecrã principal e cada área, e por ação sobre o cartão (RNF010, RNF011, P21).

### 4.3. Duplos de teste

Nos testes unitários e de widget, a API nunca é contactada. Cada dependência externa está atrás de uma interface do Núcleo, da Sessão ou de `core/device` (modelo de classes, secções 4 e 6) e é substituída por um duplo escrito à mão; o cliente HTTP real é testado com o `MockClient` do pacote `http`. O armazenamento seguro da sessão é substituído por uma implementação em memória.

Nos testes de sistema, a aplicação arranca com as dependências reais (`AppDependencies`) e a API real; a única substituição é o seletor de ficheiros, que devolve os ficheiros de teste incluídos no próprio teste.

### 4.4. Regressão

Todos os testes unitários, de widget e de sistema correm em cada Merge Request para a `main` e em cada integração na `main` (pipeline `.github/workflows/ci.yml`, Issue `I120`). Cada Issue de implementação acrescenta os testes unitários e de widget da sua funcionalidade (Definition of Done) e não pode integrar com testes a falhar nem com a cobertura abaixo de 40 %. Os cenários de sistema da secção 7 são automatizados na `I080` e passam a fazer parte da regressão a partir dessa Issue.

## 5. Ambiente

| Elemento | Testes unitários e de widget | Testes de sistema (`QG-07`) | Simulação dos testes de aceitação |
| --- | --- | --- | --- |
| Sistema operativo | `ubuntu-latest` (GitHub Actions); Windows 11 nas execuções locais | `ubuntu-latest` com emulador Android (API 34, `x86_64`) | Equipamentos do grupo (ambiente de demonstração) |
| Flutter e Dart | Flutter do canal `stable` (`subosito/flutter-action@v2`; versão registada no passo «Versões» de cada execução); Dart SDK `^3.8.0` (`pubspec.yaml`) | O mesmo; Java 17 (Temurin) para o Gradle | A versão da `main` à data da simulação |
| Pacotes | `flutter_test` (SDK), `http` 1.4, `flutter_lints` 6; restantes fixados no `pubspec.lock` | `integration_test` (SDK), acrescentado às `dev_dependencies` pela `I080` | — |
| Aplicação | Código-fonte (`lib`) | APK de depuração construído pelo `flutter test integration_test` | APK instalado num dispositivo ou emulador Android |
| Backend | Nenhum | `katch-backend` (ramo `main`, .NET 10, porto 5180) arrancado no próprio job; o emulador chega ao anfitrião por `http://10.0.2.2:5180` | `katch-backend` no ambiente de demonstração ou no URL público da `I119` |
| Base de dados | Nenhuma | PostgreSQL 18 (serviço do job), com as migrations e os scripts `02` e `03` da pasta `database` | PostgreSQL 18 com os dados de demonstração (P18) |
| Configuração da API na aplicação | Duplos | `--dart-define=API_BASE_URL=http://10.0.2.2:5180` | `--dart-define=API_BASE_URL` do ambiente de demonstração |
| Segredos | Nenhum | `Jwt__Secret` e `Katch__AdminPassword` gerados aleatoriamente em cada execução; nunca no repositório nem nos relatórios | Variáveis de ambiente da máquina; credenciais das contas de teste fora do repositório |

**Pipeline.** O job `build-and-test` corre `flutter pub get`, `flutter test --coverage` (testes unitários e de widget), a verificação da cobertura de linhas ≥ 40 % sobre o `coverage/lcov.info`, `flutter build apk --debug` (`QG-01`) e publica o artefacto `coverage-report`. Antes da `I080`, a `I075` (primeiro ecrã que chama a API) acrescenta a configuração de rede só da variante de depuração — `android/app/src/debug/res/xml/network_security_config.xml`, com tráfego sem TLS permitido apenas para `10.0.2.2`, referida no `android/app/src/debug/AndroidManifest.xml` — porque o Android bloqueia por omissão o HTTP sem TLS e a API do job responde em `http://10.0.2.2:5180`; a variante de produção continua a exigir HTTPS. O passo `QG-07 Testes de sistema (integration_test)` já existe e só corre quando existe um teste em `integration_test/`; a `I080` completa-o com: o PostgreSQL 18 como serviço do job, o checkout do `katch-backend`, a aplicação das migrations e dos dados, o arranque da API, a passagem de `--dart-define=API_BASE_URL=http://10.0.2.2:5180` e o relatório da execução (`--file-reporter json:integration-test-report.json`), publicado no artefacto `integration-test-report`. O job `static-analysis` corre o `QG-05` do mobile: `dart format --output=none --set-exit-if-changed lib test` e `flutter analyze --fatal-infos --fatal-warnings` (secção 12).

## 6. Dados de teste

- Todos os dados são fictícios (Regulamento Interno, secção 14.3): é proibida a importação ou cópia de dados reais de utilizadores para os ambientes de desenvolvimento ou de teste.
- **Unitários e de widget:** cada teste constrói os modelos de que precisa, com valores fixos e legíveis (por exemplo «Ana Ferreira», `ana.ferreira@exemplo.pt`, `912345678`, localidade «Felgueiras»), e o instante fixado pelo `Clock`. Não há dados partilhados entre testes.
- **Sistema:** a base de dados é recriada em cada execução da pipeline (migrations e scripts `02-seed-reference-data.sql` e `03-seed-demo-data.sql`), o que garante as Empresas aprovadas e as vagas publicadas do P18. Cada cenário regista o seu próprio Candidato pela aplicação, com um sufixo aleatório no correio eletrónico. As ações do Recrutador e do Administrador necessárias aos cenários do Candidato (aceitar o interesse, enviar mensagens, bloquear a conta, suspender a Empresa) são feitas por pedido direto à API a partir do próprio teste, com as contas dos dados de demonstração e do Administrador.
- **Ficheiros:** imagens JPEG e PNG pequenas, uma imagem de 5 MB, uma de 5 MB + 1 byte, um PDF mínimo, um PDF de 5 MB + 1 byte e um ficheiro com a extensão `.pdf` e conteúdo de outro formato (RNF016), incluídos nos recursos dos testes.
- **Aceitação:** dados de demonstração com os volumes mínimos do P18 e as contas temporárias de cada perfil; os dados que dependem do tempo (Candidato com a quota esgotada há mais de 24 horas, para a reposição do RF021) são preparados por script antes da sessão.

## 7. Cenários de sistema automáticos (`QG-07`)

Cada cenário é um ficheiro ou um grupo `group` em `integration_test/`, identificado pelo código `SM-nn` no nome. A especificação detalhada (passos, dados e resultado esperado de cada caso) é produzida na `I080` em `03.03.02-testes-de-sistema`, a partir desta lista. Cada cenário inclui o caso de sucesso e os casos de erro indicados.

| Código | Cenário | Casos de sucesso e de erro | Requisitos |
| --- | --- | --- | --- |
| `SM-01` | Registo e ativação imediata | Registo válido, com a localidade da lista, mostra a confirmação e permite iniciar sessão de imediato | RF003, RF005 |
| `SM-02` | Registo rejeitado | Correio eletrónico já registado, telefone com 8 algarismos e localidade por preencher são rejeitados com o campo e o motivo | RF004 |
| `SM-03` | Início e fim de sessão | Credenciais corretas iniciam sessão; palavra-passe errada e conta bloqueada (bloqueio feito por pedido direto do Administrador) mostram o motivo; terminar sessão volta ao início de sessão | RF001, RF105 |
| `SM-04` | Alteração da própria palavra-passe | Altera a palavra-passe e inicia sessão com a nova; palavra-passe atual errada é rejeitada | RF002 |
| `SM-05` | Perfil profissional e preferências | Grava função, localidade, disponibilidade e resumo; seleciona competências da lista; acrescenta «C#» pela opção «Outro»; define as preferências; resumo com 1001 caracteres, competência com 41 caracteres e distância de 501 km são rejeitados | RF006 a RF009 |
| `SM-06` | Fotografia, curriculum vitae e hiperligações | Carrega a fotografia e o curriculum vitae e grava três hiperligações; imagem de 5 MB + 1 byte, PDF com conteúdo de outro formato e 4.ª hiperligação são rejeitados | RF010 a RF012 |
| `SM-07` | Cartões de vaga segundo as preferências | Os cartões mostram logótipo, função, intervalo salarial, localização e distância, contrato, regime e urgência; não aparecem vagas fora da distância máxima ou abaixo da pretensão salarial definidas no cenário | RF013 a RF015 |
| `SM-08` | Detalhe da vaga e página da Empresa | Abre o detalhe com a descrição, as competências e os benefícios e, a partir dele, a página de apresentação da Empresa | RF016, RF017 |
| `SM-09` | Recusar e manifestar interesse | Cada ação sobre o cartão exige uma interação; a vaga recusada não volta a aparecer; o interesse diminui a quota apresentada | RF018, RF019, RF022, RNF010 |
| `SM-10` | Quota esgotada | Ao 10.º interesse, novos interesses são bloqueados e é indicado o instante da reposição; a recusa continua disponível | RF020, RF022 |
| `SM-11` | Ajuste das preferências na exploração | Alterar as preferências na área de exploração altera os cartões seguintes | RF023 |
| `SM-12` | Matches e dados de contacto | Depois da aceitação pelo Recrutador (pedido direto), o match aparece na lista com a vaga e os dados de contacto | RF024, RF025, RF068 |
| `SM-13` | Conversas do Candidato | A lista indica as mensagens por ler; abrir a conversa marca-as como lidas e mostra o histórico ordenado; envia uma mensagem; uma mensagem do Recrutador enviada por pedido direto aparece sem atualização manual (consulta periódica de 3 em 3 segundos; a mensagem aparece no máximo 5 segundos depois do envio, P17); mensagem com 1001 caracteres é rejeitada | RF026 a RF029, RF108 |
| `SM-14` | Encerramento e modo apenas de consulta | Encerrar a conversa deixa o histórico visível e impede o envio; a suspensão da Empresa (pedido direto do Administrador) passa a conversa a modo apenas de consulta; sem match não há conversa | RF030, RF070, RF075 |
| `SM-15` | Navegação em 3 interações | A partir do ecrã principal, o perfil, a exploração, os matches e as conversas são alcançados em 3 interações, no máximo | RNF011 |
| `SM-16` | Cartão seguinte em 2 segundos | Depois de recusar ou de manifestar interesse, o cartão seguinte é apresentado em 2 segundos, no máximo (medição com `Stopwatch`) | RNF002 |
| `SM-17` | Falha de ligação | Com a aplicação apontada para um endereço sem resposta, a falha de ligação é indicada em 10 segundos, no máximo, a contar da ação | RNF014 |

A reposição da quota ao fim de 24 horas (RF021) não é automatizada no nível de sistema, porque exigiria esperar 24 horas ou alterar o relógio do servidor: é verificada no nível unitário (controller da exploração com relógio fixo), nos testes do backend e na simulação dos testes de aceitação com dados preparados.

## 8. Simulação dos testes de aceitação

A simulação dos testes de aceitação é feita na Issue `I081` (Sprint previsional S10) e é declarada como **simulação**: os participantes são elementos do grupo que representam o cliente, não utilizadores reais.

| Aspeto | Regra deste plano |
| --- | --- |
| Objeto | A aplicação móvel construída a partir da `main`, com o backend da mesma data, no ambiente de demonstração |
| Cenários | Um cenário de aceitação por funcionalidade da secção 3.1 (`AM-F001`, `AM-F002`, `AM-F003`, `AM-F004`, `AM-F006`, `AM-F008`, `AM-F010`) e um para os requisitos não funcionais (`AM-RNF`), escritos como guião de utilização do Candidato e verificados contra o **critério de aceitação** de cada requisito da especificação v02 |
| Participantes | Um elemento no papel de Candidato, que não tenha sido Executor das Issues de implementação da funcionalidade em causa; um elemento que executa na área de gestão web as ações do Recrutador e do Administrador necessárias; um observador que regista os resultados; todos identificados no relatório |
| Medições | Tempos dos RNF medidos com cronómetro (RNF002, RNF014); contagem das interações (RNF010, RNF011); a falha de ligação é provocada desligando a rede do dispositivo |
| Registo | Para cada cenário: data e hora, versão (commit) testada, dispositivo e versão do Android, participantes, passos, resultado (aceite, aceite com desvio, não aceite), desvios observados e evidência (captura de ecrã) |
| Desvios | Cada desvio é registado como defeito em `03.03.07-defeitos` e na Issue correspondente; um requisito obrigatório não aceite impede o critério de saída da secção 11.2 |
| Entregáveis | Especificação dos testes de aceitação e relatório da simulação em `03.03.03-testes-de-aceitacao` |

## 9. Responsabilidades

| Atividade | Responsável | Verificação |
| --- | --- | --- |
| Testes unitários e de widget de cada funcionalidade | Executor da Issue de implementação (Definition of Done): `I075`, `I076`, `I077` (S08) e `I078` (S09), a atribuir no Sprint Planning de cada Sprint | Revisor de cada Issue |
| Este plano de testes | João Borguem (`I123`) | Revisor Miguel Santos; Auditor João Coelho |
| Testes de sistema automáticos (`integration_test`), especificação e resultados | Executor da `I080` (a atribuir no Sprint Planning do S09) | Revisor e Auditor da `I080` |
| Simulação dos testes de aceitação, especificação e relatório | Executor da `I081` (a atribuir no Sprint Planning do S10) | Revisor e Auditor da `I081` |
| Análise estática (relatório do Dart analyzer e do `dart format`) | Executor da `I124` | Revisor e Auditor da `I124` |
| Configuração e manutenção da pipeline e dos quality gates | João Borguem, responsável pela configuração e manutenção técnica da pipeline (Regulamento Interno, secção 11.6; configuração dos frontends na `I120`) | Revisor e Auditor de cada Issue que altera a pipeline |
| Verificação independente da configuração e dos resultados da pipeline | Líder em funções em `m4`, João Borguem (Regulamento Interno, secções 6.2 e 11.6) | — |
| Registo de defeitos encontrados pelos testes | Quem encontra o defeito, na Issue correspondente e em `03.03.07-defeitos` | Revisor da Issue |

O Executor, o Revisor e o Auditor de cada Issue são sempre alunos diferentes; o Executor nunca aprova nem integra o próprio Merge Request (Regulamento Interno, secção 10.4).

## 10. Âmbito da cobertura

| Medida | Código incluído | Código excluído e justificação | Limiar |
| --- | --- | --- | --- |
| Cobertura de linhas dos testes unitários e de widget do frontend mobile (Regulamento Interno, secção 11.5) | `lib/**/*.dart`: widgets e ecrãs, controllers e stores de estado, serviços, núcleo, sessão, consulta periódica e modelos com lógica | Ficheiros gerados automaticamente (`*.g.dart`, `*.freezed.dart`, `lib/generated/**`), se vierem a existir; `lib/main.dart` (arranque da aplicação, verificado pelos testes de sistema) | ≥ 40 % |

A cobertura é medida pelo `flutter test --coverage` (formato `lcov`, `coverage/lcov.info`) e verificada no passo «Cobertura de linhas ≥ 40 %» da pipeline, que soma as linhas encontradas (`LF`) e cobertas (`LH`) e termina sem sucesso abaixo do limiar. O relatório é publicado no artefacto `coverage-report`. A cobertura dos testes `integration_test` não é medida nem somada.

**Ficheiros sem testes.** O `flutter test --coverage` só regista os ficheiros carregados por algum teste: um ficheiro de `lib/` que nenhum teste importa não aparece no `lcov.info` e não conta como não coberto, o que faria a medida parecer mais alta do que é. Para a medida respeitar o âmbito acima, a primeira Issue de implementação do mobile (`I075`) acrescenta um teste que importa todos os ficheiros de `lib/` (por exemplo, `test/coverage_all_files_test.dart`, gerado por um script e verificado na pipeline) e retira do `lcov.info`, antes da verificação, os ficheiros excluídos acima.

## 11. Critérios de entrada e de saída

### 11.1. Critérios de entrada

| Nível | Critérios |
| --- | --- |
| Unitário e de widget | Issue em execução no Sprint Backlog; ecrã, controller ou serviço previsto no modelo de classes do frontend mobile v02 e operação documentada na documentação da API v02; `flutter analyze` sem ocorrências |
| Sistema | Funcionalidade integrada na `main` do frontend mobile; endpoints usados integrados na `main` do backend; configuração de rede da variante de depuração com tráfego sem TLS para `10.0.2.2` (`I075`); pacote `integration_test` e passo `QG-07` configurados (`I080`); base de dados recriada com as migrations e os dados |
| Aceitação | Todas as Issues de implementação do frontend mobile integradas; `QG-07` aprovado na `main`; ambiente de demonstração instalado com os dados do P18; APK instalado; participantes e observador designados |
| Pipeline | Merge Request aberto para a `main`; ramo atualizado com a `main` |

### 11.2. Critérios de saída

| Nível | Critérios |
| --- | --- |
| Unitário e de widget | 100 % dos testes executados sem falhas; cobertura de linhas ≥ 40 % no âmbito da secção 10; cada regra de `InputRules` com casos válidos, inválidos e de fronteira dos parâmetros aplicáveis; cada ecrã com os estados de carregamento, erro e sucesso |
| Sistema (`QG-07`) | 100 % dos testes executados sem falhas; os cenários `SM-01` a `SM-17` automatizados; relatório da execução publicado no artefacto `integration-test-report` |
| Aceitação | Todos os cenários executados; todos os requisitos obrigatórios da secção 13 aceites; desvios registados como defeitos e tratados ou justificados no relatório |
| Pipeline | Build, `dart format`, Dart analyzer, testes unitários e de widget com a cobertura e testes de sistema sem falhas na execução do Merge Request |

### 11.3. Suspensão e retoma

Os testes são suspensos quando o build falha, quando a pipeline não arranca, quando o emulador ou o backend do job não arrancam ou quando um defeito bloqueia a maior parte dos casos de um nível (por exemplo, o registo falha em todos os cenários de sistema). Retomam-se depois da correção, repetindo o nível inteiro sobre o novo commit; os resultados anteriores ficam como histórico e não são reaproveitados. Um teste de sistema instável é tratado como falha e corrigido; não se acrescentam repetições automáticas para o fazer passar.

## 12. Quality gates aplicáveis

| Código | Verificação | Critério | Evidência |
| --- | --- | --- | --- |
| `QG-01` | Construção | `flutter build apk --debug` sem erros | Registo do job `build-and-test` |
| `QG-05` | Linter e análise estática | `dart format --set-exit-if-changed` sem diferenças e `flutter analyze --fatal-infos --fatal-warnings` sem ocorrências | Registo do job `static-analysis` |
| `QG-07` | Testes de sistema automáticos | 100 % dos testes `integration_test` executados sem falhas | Relatório da execução (artefacto `integration-test-report`) e registo do job |
| Cobertura (secção 11.5) | Testes unitários e de widget e cobertura | 100 % dos testes `flutter test` sem falhas e ≥ 40 % de linhas | Artefacto `coverage-report` e resumo da execução |

**QG-05 do mobile sem SonarQube.** O Regulamento Interno (secções 11.4 e 11.6) prevê o SonarQube também no frontend mobile, mas o SonarQube Community Build não analisa Dart. O grupo decidiu na `I120` (2026-11-05) usar como `QG-05` do mobile o Dart analyzer com `flutter_lints`, com infos e avisos tratados como erro, e o `dart format`. A divergência está registada como não conformidade NC-033 (`m4-registo-nao-conformidades-v02.xlsx`), cuja ação corretiva é a revisão do Regulamento Interno (secção 19.2). Este plano aplica o `QG-05` tal como está configurado e passa a seguir a nova redação quando a revisão for aprovada.

Os gates `QG-02`, `QG-03` e `QG-04` do Regulamento Interno (secção 11.6) aplicam-se só ao backend. A verificação da cobertura dos frontends não tem código próprio na secção 11.6, mas é bloqueante na pipeline, como os gates codificados. Todos os gates são efetivos: o incumprimento termina o job sem sucesso, e a regra de proteção da `main` exige os jobs `build-and-test` e `static-analysis` com sucesso antes do merge (evidências da `I120`).

## 13. Rastreabilidade aos requisitos

Legenda: **U** — teste unitário ou de widget na Issue de implementação; **S** — cenário de sistema automático da secção 7; **A** — cenário de aceitação da secção 8.

| Requisito | Título | Issue de implementação | U | S | A |
| --- | --- | --- | --- | --- | --- |
| RF001 | Iniciar sessão na aplicação móvel | `I075` | Sim | `SM-03` | `AM-F001` |
| RF002 | Alterar a própria palavra-passe (Candidato) | `I075` | Sim | `SM-04` | `AM-F001` |
| RF003 | Registar-se como Candidato | `I075` | Sim | `SM-01` | `AM-F002` |
| RF004 | Impedir registo de Candidato sem validação automática | `I075` | Sim | `SM-02` | `AM-F002` |
| RF005 | Ativar de imediato a conta do Candidato | `I075` | Sim | `SM-01` | `AM-F002` |
| RF006 | Gravar os dados do perfil profissional | `I077` | Sim | `SM-05` | `AM-F004` |
| RF007 | Definir as preferências de procura | `I077` | Sim | `SM-05` | `AM-F004` |
| RF008 | Selecionar competências da lista pré-definida | `I077` | Sim | `SM-05` | `AM-F004` |
| RF009 | Introduzir competência através da opção Outro | `I077` | Sim | `SM-05` | `AM-F004` |
| RF010 | Carregar fotografia de perfil | `I077` | Sim | `SM-06` | `AM-F004` |
| RF011 | Enviar curriculum vitae | `I077` | Sim | `SM-06` | `AM-F004` |
| RF012 | Indicar hiperligações profissionais externas | `I077` | Sim | `SM-06` | `AM-F004` |
| RF013 | Consultar vagas sob a forma de cartões | `I076` | Sim | `SM-07` | `AM-F006` |
| RF014 | Selecionar vagas segundo as preferências de procura | `I076` | Sim | `SM-07` | `AM-F006` |
| RF015 | Calcular a distância em linha reta | `I076` (apresentação) | Sim | `SM-07` | `AM-F006` |
| RF016 | Consultar o detalhe de uma vaga | `I076` | Sim | `SM-08` | `AM-F006` |
| RF017 | Consultar a página de apresentação da Empresa | `I076` | Sim | `SM-08` | `AM-F003` |
| RF018 | Recusar uma vaga | `I076` | Sim | `SM-09` | `AM-F006` |
| RF019 | Manifestar interesse numa vaga | `I076` | Sim | `SM-09` | `AM-F006` |
| RF020 | Bloquear novos interesses após esgotar a quota | `I076` | Sim | `SM-10` | `AM-F006` |
| RF021 | Repor integralmente a quota de interesses | `I076` | Sim | — (secção 7) | `AM-F006` |
| RF022 | Consultar os interesses disponíveis na quota | `I076` | Sim | `SM-09`, `SM-10` | `AM-F006` |
| RF023 | Ajustar preferências na área de exploração | `I076` | Sim | `SM-11` | `AM-F006` |
| RF024 | Consultar a lista de matches | `I078` | Sim | `SM-12` | `AM-F008` |
| RF025 | Consultar os dados de contacto após match | `I078` | Sim | `SM-12` | `AM-F008` |
| RF026 | Enviar mensagem ao Recrutador | `I078` | Sim | `SM-13` | `AM-F010` |
| RF027 | Consultar a lista de conversas (Candidato) | `I078` | Sim | `SM-13` | `AM-F010` |
| RF028 | Consultar o histórico de uma conversa (Candidato) | `I078` | Sim | `SM-13` | `AM-F010` |
| RF029 | Receber mensagens sem atualização manual (Candidato) | `I078` | Sim | `SM-13` | `AM-F010` |
| RF030 | Encerrar uma conversa (Candidato) | `I078` | Sim | `SM-14` | `AM-F010` |
| RF068 | Criar automaticamente a conversa do match | `I078` (apresentação) | Sim | `SM-12` | `AM-F008`, `AM-F010` |
| RF070 | Impedir comunicação sem match | `I078` | Sim | `SM-14` | `AM-F010` |
| RF075 | Passar conversa a consulta por bloqueio ou suspensão | `I078` | Sim | `SM-14` | `AM-F010` |
| RF105 | Terminar sessão na aplicação móvel | `I075` | Sim | `SM-03` | `AM-F001` |
| RF108 | Marcar como lidas as mensagens ao abrir a conversa (Candidato) | `I078` | Sim | `SM-13` | `AM-F010` |
| RNF002 | Apresentar o cartão de vaga seguinte em 2 segundos | `I076` | — | `SM-16` | `AM-RNF` |
| RNF010 | Exigir uma interação por ação sobre o cartão de vaga | `I076` | Sim | `SM-09` | `AM-RNF` |
| RNF011 | Aceder às áreas da aplicação móvel em 3 interações | Todas as Issues do frontend mobile | — | `SM-15` | `AM-RNF` |
| RNF014 | Indicar falha de ligação na aplicação móvel em 10 segundos | `I120` (cliente da API e monitor de ligação) e todos os ecrãs | Sim | `SM-17` | `AM-RNF` |

São 35 requisitos funcionais e 4 não funcionais. RF002 e RF105 são implementados na `I075`, com a autenticação do Candidato. A `I126` (RTM de `m4`) regista nesta matriz os testes efetivamente executados.

## 14. Entregáveis de teste

| Entregável | Pasta | Issue |
| --- | --- | --- |
| `m4-plano-testes-frontend-mobile-v01.md` (este documento) | `03.03.01-plano-de-testes` | `I123` |
| Especificação e resultados dos testes de sistema automáticos (`integration_test`) | `03.03.02-testes-de-sistema` | `I080` |
| Especificação dos testes de aceitação e relatório da simulação | `03.03.03-testes-de-aceitacao` | `I081` |
| Resultados dos testes unitários e de widget | `03.03.04-testes-unitarios` | Issue a criar num Backlog Refinement futuro (secção 15) |
| Relatório de cobertura dos testes unitários e de widget | `03.03.05-cobertura-de-testes` | Issue a criar num Backlog Refinement futuro (secção 15) |
| Relatório de análise estática | `03.03.06-analise-estatica` | `I124` |
| Registo de defeitos | `03.03.07-defeitos` | Quem encontra o defeito |
| Código dos testes (`test/`, `integration_test/`) | Repositório `katch-frontend-mobile` | Issues de implementação e `I080` |

Os nomes dos ficheiros seguem a nomenclatura oficial (`m4-<descrição>-v0N.<extensão>`) e são fixados nas Issues que os produzem.

## 15. Riscos e limitações

| Risco ou limitação | Efeito | Tratamento |
| --- | --- | --- |
| O Android bloqueia por omissão o tráfego HTTP sem TLS e a API do job responde em `http://10.0.2.2:5180` | Sem configuração, os pedidos da aplicação falham no emulador e o `QG-07` não corre | Configuração de rede só da variante de depuração (`network_security_config.xml` com tráfego sem TLS permitido apenas para `10.0.2.2`), feita na `I075`, primeiro ecrã que chama a API, para a `I080` e o `QG-07` a encontrarem feita; a variante de produção continua a exigir HTTPS (secção 5) |
| Os resultados dos testes unitários e o relatório de cobertura (`03.03.04` e `03.03.05`) ainda não têm Issue no Backlog de Projeto v07 | O artefacto pode ficar por produzir | Issues a criar num Backlog Refinement futuro; até lá, as evidências são o artefacto `coverage-report` e o resumo de cada execução da pipeline |

## 16. Referências

1. Regulamento de Funcionamento da Unidade Curricular de LDS 2026/2027, secção 19 (pastas `03.02.01` e `03.03.01`).
2. Regulamento Interno do Grupo G11, v01 (`m1-regulamento-grupo-v01.pdf`), secções 6.2, 10.4, 11.3, 11.4, 11.5, 11.6, 14.3 e 19.2.
3. Declaração de Âmbito, v04 (`m3-declaracao-ambito-v04.pdf`), matriz do âmbito funcional (coluna «Âmbito no frontend-mobile») e limites transversais.
4. Especificação de Requisitos, v02 (`m3-especificacao-requisitos-v02.md`), secções 2.2, 3 (parâmetros P01 a P21), 4.1, 4.2 e 5.
5. Modelo de Classes do Frontend Mobile, v02 (`m3-modelo-classes-frontend-mobile-v02.md`), secções 2.3, 4, 6, 7 e 9.
6. Plano de Testes do Backend, v01 (`m3-plano-testes-backend-v01.md`).
7. Backlog de Projeto, v07 (`m4-backlog-projeto-v07.xlsx`) e Sprint Backlog do S07 (`m4-s07-sprint-backlog-v01.xlsx`): Issues `I075` a `I081`, `I120`, `I123`, `I124` e `I126`.
8. Relatório de evidências da pipeline CI/CD dos frontends, v01 (`m4-relatorio-evidencias-pipeline-ci-cd-frontends-v01.md`, Issue `I120`) e `.github/workflows/ci.yml` do `katch-frontend-mobile`.
9. Registo de não conformidades, v02 (`m4-registo-nao-conformidades-v02.xlsx`), NC-033.
10. README do `katch-backend` (arranque da API, porto 5180, variáveis `ConnectionStrings__Katch`, `Jwt__Secret` e `Katch__AdminPassword`).
