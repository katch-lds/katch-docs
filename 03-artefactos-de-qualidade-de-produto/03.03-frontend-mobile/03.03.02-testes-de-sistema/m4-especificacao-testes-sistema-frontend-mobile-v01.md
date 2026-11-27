# Especificação dos testes de sistema do frontend mobile — Katch

## 1. Identificação do documento

| Campo | Valor |
| --- | --- |
| Projeto | Katch |
| Grupo | G11 |
| Milestone | m4 |
| Issue | `I080` — Escrever os testes de sistema automáticos do frontend mobile (integration_test), Sprint 10 |
| Executor / Revisor / Auditor | João Borguem / João Coelho / Roberto Baptista |
| Documentos de origem | `m4-plano-testes-frontend-mobile-v01.md` (secções 4, 5, 6, 7 e 12); `m4-especificacao-requisitos-v03.md` |
| Código dos testes | `katch-frontend-mobile`: `integration_test/katch_system_test.dart`, `integration_test/scenarios/sm_*.dart`, `integration_test/support/*.dart`, `integration_test/README.md`; passo `QG-07` do `.github/workflows/ci.yml` |

### 1.1. Histórico de versões

| Versão | Data | Descrição das alterações | Issue |
| --- | --- | --- | --- |
| v01 | 2026-11-27 | Criação do documento: ambiente, dados e especificação dos cenários SM-01 a SM-17. | `I080` |

## 2. Ambiente e execução

- Aplicação completa (`KatchApp` com as dependências reais de `AppDependencies.create`) num emulador Android (API 34, `x86_64`), construída pelo `flutter test integration_test` (Flutter 3.47.x, Java 17).
- Substituições: o seletor de ficheiros, por um que devolve os ficheiros do teste (`QueuedFilePicker`), como prevê o plano (secção 4.3); e o armazenamento seguro da sessão, por um em memória, para cada teste começar sem sessão (desvio D1, secção 2.1). O cliente HTTP é o real, envolvido por um que só regista os cartões recebidos (`GET /api/candidate/jobs/next`) para o SM-07 e o SM-11 (D1).
- API do `katch-backend` (versão fixa `796f81e`, a mesma do QG-06 do frontend web; desvio D3, secção 2.1) arrancada no próprio job, sobre PostgreSQL 18 com os scripts `01-create-schema.sql`, `02-seed-reference-data.sql` e `03-seed-demo-data.sql`; o emulador chega-lhe por `http://10.0.2.2:5180` (`--dart-define=API_BASE_URL`).
- Um só ficheiro `*_test.dart` (`katch_system_test.dart`), para a aplicação ser construída e instalada uma vez; cada cenário é um `group('SM-nn …')` com testes `SM-nnx`, executados por ordem.
- Na pipeline, os passos `QG-07` do job `build-and-test` do `ci.yml`, com o relatório `integration-test-report.json` (`--file-reporter json`) e o registo da API no artefacto `integration-test-report`; uma falha faz falhar o job, que a regra de proteção da `main` exige com sucesso.
- A palavra-passe do Administrador é gerada em cada execução, mascarada nos registos e passada por `--dart-define=E2E_ADMIN_PASSWORD`; o `Jwt__Secret` também é gerado. Nenhuma palavra-passe fica no repositório (RNF003). Os limites de pedidos por IP da API (`Limits__*`) são alargados só nesta instância.
- As esperas usam `pump` com intervalos de 100 ms até ao elemento aparecer (máximo de 20 s por espera), e não `pumpAndSettle`, porque os indicadores de carregamento animam enquanto a API responde.
- Os tempos do SM-16 e do SM-17 contam a partir do toque (o cronómetro arranca depois de o elemento estar pronto a receber o toque e imediatamente antes dele) e são escritos na saída e no relatório JSON em todas as execuções, passe ou falhe o teste, para se ver a folga em relação ao limite.

### 2.1. Desvios ao plano de testes

| N.º | Plano de testes | Na automação | Motivo | Efeito na cobertura |
| --- | --- | --- | --- | --- |
| D1 | Secção 4.3: nos testes de sistema, a única substituição é o seletor de ficheiros; o armazenamento em memória da sessão é previsto só nos testes unitários e de widget. | O armazenamento seguro da sessão é substituído por um em memória (`MemorySessionStorage`) e o cliente HTTP real é envolvido por um que só regista os cartões recebidos (`RecordingClient`). | Os 21 testes correm na mesma instalação da aplicação (um só ponto de entrada); com o armazenamento real, a sessão de um teste passava ao seguinte. O registo dos cartões permite ao SM-07 e ao SM-11 confirmar a ordem e os filtros sem depender da posição no ecrã. | A persistência da sessão no armazenamento seguro do Android não é verificada neste nível (fica nos testes unitários do `SessionManager` com o duplo do armazenamento). Os pedidos e as respostas da API não são alterados. |
| D2 | Secção 6: cada cenário regista o seu Candidato pela aplicação; as ações do Recrutador usam as contas dos dados de demonstração. | Só o SM-01 regista o Candidato pela aplicação; os restantes criam-no pela API e iniciam sessão pela aplicação. Os Recrutadores e as Empresas são criados pela API em cada cenário, com um sufixo único. | O registo pela aplicação é o objeto do SM-01 e do SM-02; repeti-lo nos outros cenários alongava a execução sem verificar nada de novo. Contas próprias por cenário evitam que a quota, as vagas e os matches de uma execução afetem a seguinte e permitem repetir a execução sem repor a base. | Nenhum: o registo pela aplicação é verificado no SM-01 e no SM-02, e cada cenário continua a usar a aplicação em tudo o que verifica. |
| D3 | Secção 5: `katch-backend` do ramo `main`. | Versão fixa `796f81e` (a `main` à data da execução), a mesma do QG-06 do frontend web. | Uma alteração no backend não deve fazer falhar o merge de um Merge Request do mobile que não lhe mexe; a versão é atualizada de propósito quando a API muda. | Uma alteração da API posterior a `796f81e` só é apanhada quando a versão fixa for atualizada no `ci.yml`. |
| D4 | Secção 6: imagens JPEG e PNG, imagem de 5 MB e de 5 MB + 1 byte, PDF mínimo, PDF de 5 MB + 1 byte e ficheiro `.pdf` com outro conteúdo, incluídos nos recursos dos testes. | PNG de 1×1 píxel, PDF mínimo, PNG de 5 MB + 1 byte e `falso.pdf` com conteúdo PNG, gerados no próprio teste. | Gerar os ficheiros no teste evita ficheiros binários grandes no repositório. | O JPEG, a imagem de exatamente 5 MB e o PDF de 5 MB + 1 byte não são usados no SM-06; o limite e o tipo são verificados com a imagem de 5 MB + 1 byte e com o `falso.pdf`. |

## 3. Dados de teste

- Cada cenário cria pela API os Candidatos, os Recrutadores (com a Empresa registada, aprovada pelo Administrador, com descrição, sítio e logótipo) e as vagas de que precisa, com um sufixo único no correio eletrónico e nos nomes, para a execução se repetir sem repor a base. O SM-01 regista o Candidato pela aplicação; os restantes registam-no pela API e iniciam sessão pela aplicação (desvio D2, secção 2.1).
- As ações do Recrutador e do Administrador (aceitar o interesse, enviar mensagens, bloquear a conta, suspender a Empresa) são pedidos diretos à API, como prevê o plano (secção 6).
- **Localidades:** cada cenário de exploração usa uma localidade sem vagas nos dados de demonstração (Bragança, Guarda, Viseu, Castelo Branco, Évora, Beja, Faro, Portalegre, Leiria, Aveiro) para o Candidato e para as vagas que cria, que são urgentes. Como os cartões aparecem com as urgentes primeiro e, entre elas, as mais próximas, a vaga do cenário é o primeiro cartão (distância 0). Se aparecer antes outra vaga (de execuções anteriores), o teste recusa-a até chegar à sua.
- **Ficheiros:** PNG de 1×1 píxel, PDF mínimo, PNG com 5 MB + 1 byte e ficheiro `falso.pdf` com conteúdo PNG (RNF016), gerados no próprio teste (desvio D4, secção 2.1).

## 4. Cenários

Cada cenário tem um `group('SM-nn …')` num ficheiro `integration_test/scenarios/sm_*.dart` e testes identificados por `SM-nnx`. Os passos usam as chaves dos widgets (`Key`) e os textos da aplicação.

### SM-01 — Registo e ativação imediata

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF003, RF005 |
| Precondições | Sem sessão. |
| Dados | Nome, correio `sm01-…@teste.katch.pt` e telefone únicos; localidade «Bragança (Bragança)» da lista; palavra-passe válida (P05). |
| Passos | SM-01a: «Criar conta»; preencher os campos, escolher a localidade e aceitar as condições; «Criar conta»; «Começar a explorar»; terminar sessão na Conta; iniciar sessão com os dados do registo. |
| Resultados esperados | «A sua conta está ativa»; ecrã principal com os separadores; depois de terminar, o início de sessão com os mesmos dados volta ao ecrã principal. |
| Testes | `sm_01_02_registo.dart` |

### SM-02 — Registo rejeitado

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF004 |
| Precondições | Candidato já registado pela API. |
| Dados | Correio do Candidato existente; telefone `91234567` (8 algarismos); localidade por escolher. |
| Passos | SM-02a: preencher com o telefone de 8 algarismos e sem localidade e criar conta; corrigir o telefone e a localidade e criar conta. |
| Resultados esperados | Telefone com «O contacto telefónico tem de ter 9 algarismos.» e localidade com «O campo é obrigatório.», sem pedido à API; depois, o correio com «O endereço de correio eletrónico já está registado.» devolvido pela API; não há confirmação de conta ativa. |
| Testes | `sm_01_02_registo.dart` |

### SM-03 — Início e fim de sessão

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF001, RF105 |
| Precondições | Candidatos registados pela API; um deles bloqueado pelo Administrador durante o teste. |
| Dados | Credenciais corretas; palavra-passe errada. |
| Passos | SM-03a: iniciar sessão; terminar sessão na Conta.<br>SM-03b: iniciar sessão com a palavra-passe errada; bloquear a conta (pedido do Administrador); iniciar sessão com a palavra-passe correta. |
| Resultados esperados | Ecrã principal; depois de terminar, o início de sessão sem os separadores.<br>«O endereço de correio eletrónico ou a palavra-passe estão incorretos.»; depois, «A conta está bloqueada.»; em ambos não entra. |
| Testes | `sm_03_04_sessao.dart` |

### SM-04 — Alteração da própria palavra-passe

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF002 |
| Precondições | Candidato registado pela API, com sessão iniciada. |
| Dados | Palavra-passe atual errada; palavra-passe nova válida. |
| Passos | SM-04a: Conta → «Alterar palavra-passe» com a atual errada; com a atual correta; terminar sessão; entrar com a antiga e com a nova. |
| Resultados esperados | «A palavra-passe atual está errada.» no campo; depois, «A palavra-passe foi alterada.»; a antiga é recusada e a nova inicia sessão. |
| Testes | `sm_03_04_sessao.dart` |

### SM-05 — Perfil profissional e preferências

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF006 a RF009 (e preferências de RF014 e RF015) |
| Precondições | Candidato registado pela API, com sessão iniciada. |
| Dados | Função «Programador Flutter», localidade «Bragança (Bragança)», disponibilidade «15 dias», resumo; uma competência da lista e «C#» por «Outro»; distância de 120 km, pretensão de 1300 €, regime «Híbrido», contrato «Sem termo». Casos de erro: resumo com 1001 caracteres, competência com 41 caracteres, distância de 501 km. |
| Passos | SM-05a: gravar o perfil; nas Competências, escolher da lista e acrescentar «C#» por «Outro» e gravar; nas Preferências, definir e gravar; ler o perfil pela API.<br>SM-05b: gravar o resumo de 1001 caracteres; acrescentar a competência de 41 caracteres; gravar a distância de 501 km; enviar os mesmos valores diretamente à API. |
| Resultados esperados | «O perfil foi gravado.», «As competências foram gravadas.», «As preferências foram gravadas.»; o perfil da API tem os valores gravados, a competência da lista e «C#».<br>«O texto não pode exceder 1000 caracteres.», «A competência tem de ter entre 2 e 40 caracteres…» e «A distância máxima tem de ser um número inteiro entre 1 e 500.» nos campos, sem gravação; a API responde 400 (ERR-10) aos três. |
| Testes | `sm_05_06_perfil.dart` |

### SM-06 — Fotografia, curriculum vitae e hiperligações

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF010 a RF012, RNF016 |
| Precondições | Candidato registado pela API, com sessão iniciada. |
| Dados | `foto.png` (1×1), `cv.pdf` (mínimo), três hiperligações `https://`; `grande.png` (5 MB + 1 byte), `falso.pdf` (conteúdo PNG), 4.ª hiperligação. |
| Passos | SM-06a: «Fotografia, CV e hiperligações»: escolher a fotografia e o CV, acrescentar três hiperligações e gravar; ler o perfil pela API.<br>SM-06b: escolher `grande.png` e `falso.pdf`; acrescentar quatro hiperligações; gravar; enviar os mesmos ficheiros e quatro hiperligações diretamente à API. |
| Resultados esperados | «Os ficheiros e as hiperligações foram gravados.» e «Curriculum vitae enviado.»; o perfil tem fotografia, CV e 3 hiperligações.<br>«O ficheiro excede 5 MB.», «O ficheiro tem de estar no formato PDF.» e «São permitidas no máximo 3 hiperligações.», sem 4.º campo nem gravação; a API recusa os ficheiros (400 ou 413) e as hiperligações (400); o perfil fica sem fotografia nem CV. |
| Testes | `sm_05_06_perfil.dart` |

### SM-07 — Cartões de vaga segundo as preferências

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF013 a RF015 |
| Precondições | Empresa aprovada com logótipo; Candidato em Bragança com distância máxima de 50 km e pretensão de 1500 € (pela API). |
| Dados | Vaga elegível em Bragança (1400 € – 2000 €, «Sem termo», «Presencial», urgente); vaga em Vila Real (cerca de 80 km); vaga em Bragança com máximo de 1200 €. |
| Passos | SM-07a: chegar ao cartão da vaga elegível; recusar todos os cartões até «Não há vagas compatíveis com as suas preferências.». |
| Resultados esperados | O cartão mostra o logótipo, a função, a Empresa, «1400 € – 2000 €», «Bragança (Bragança) · 0 km», «Sem termo», «Presencial» e «Urgente»; nenhum cartão é da vaga de Vila Real nem da de 1200 €; todos os cartões recebidos estão a 50 km ou menos (ou são remotos) e têm salário máximo de 1500 € ou mais. |
| Testes | `sm_07_11_16_exploracao.dart` |

### SM-08 — Detalhe da vaga e página da Empresa

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF016, RF017 |
| Precondições | Empresa aprovada com descrição e sítio; Candidato na Guarda. |
| Dados | Vaga na Guarda com 2 competências e 2 benefícios. |
| Passos | SM-08a: no cartão, «Ver detalhe»; «Ver página da Empresa». |
| Resultados esperados | Detalhe com a descrição, as competências e os benefícios da vaga; página com o nome e a descrição da Empresa. |
| Testes | `sm_07_11_16_exploracao.dart` |

### SM-09 — Recusar e manifestar interesse

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF018, RF019, RF022, RNF010 |
| Precondições | Candidato em Viseu; duas vagas em Viseu. |
| Dados | Quota lida pela API antes e depois. |
| Passos | SM-09a: «Recusar» no cartão; repetir a recusa da mesma vaga pela API e pedir o cartão seguinte pela API; «Tenho interesse» no cartão seguinte. |
| Resultados esperados | Cada ação conta uma interação e mostra o cartão seguinte; a recusa repetida é recusada (409, ERR-32) e o cartão seguinte é outra vaga; depois do interesse, «Interesses disponíveis» baixa uma unidade, igual à quota da API. |
| Testes | `sm_07_11_16_exploracao.dart` |

### SM-10 — Quota esgotada

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF020, RF022 |
| Precondições | Candidato novo em Castelo Branco (quota completa). |
| Dados | Total da quota lido pela API (P11: 10 interesses). |
| Passos | SM-10a: «Tenho interesse» até esgotar a quota; tentar o interesse pela API; «Recusar». |
| Resultados esperados | «Esgotou os 10 interesses. Poderá voltar a manifestar interesse dentro de … (às …)» e «Interesses disponíveis: 0 de 10»; «Tenho interesse» desativado; a quota da API tem `blockedUntil`; o interesse pela API é recusado com `409` e o código `ERR-31`; a recusa mostra o cartão seguinte. |
| Testes | `sm_07_11_16_exploracao.dart` |

### SM-11 — Ajuste das preferências na exploração

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF023 |
| Precondições | Candidato em Évora; vaga presencial em Évora. |
| Dados | Regime «Remoto». |
| Passos | SM-11a: no cartão da vaga presencial, «Preferências»; escolher «Remoto» e gravar; voltar à exploração. |
| Resultados esperados | «As preferências foram gravadas.»; o cartão seguinte é outra vaga, com regime remoto («Remoto»). |
| Testes | `sm_07_11_16_exploracao.dart` |

### SM-12 — Matches e dados de contacto

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF024, RF025, RF068 |
| Precondições | Empresa aprovada; Candidato em Beja; vaga em Beja. |
| Dados | Correio de contacto da Empresa (o do Recrutador) e telefone `910000000`. |
| Passos | SM-12a: «Tenho interesse» no cartão da vaga; o Recrutador abre o perfil e aceita (pedido direto); separador «Matches»; «Ver contacto». |
| Resultados esperados | O match aparece com a função e a Empresa; o contacto mostra o correio eletrónico e o telefone da Empresa. |
| Testes | `sm_12_15_matches_conversas.dart` |

### SM-13 — Conversas do Candidato

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF026 a RF029, RF108 |
| Precondições | Match criado pela API (interesse e aceitação); duas mensagens do Recrutador. |
| Dados | Resposta do Candidato; mensagem nova do Recrutador; texto de 1001 caracteres. |
| Passos | SM-13a: separador «Conversas»; abrir a conversa; enviar a resposta; o Recrutador envia uma mensagem (pedido direto) com a conversa aberta; escrever 1001 caracteres; enviar 1001 caracteres pela API; voltar à lista. |
| Resultados esperados | A conversa e o separador indicam 2 mensagens por ler; o histórico aparece por ordem; a resposta aparece; a mensagem do Recrutador aparece sem atualização manual em 5 s, no máximo (P17); o campo fica nos 1000 caracteres e a API responde 400 (ERR-10); de volta à lista, já não há mensagens por ler. |
| Testes | `sm_12_15_matches_conversas.dart` |

### SM-14 — Encerramento e modo apenas de consulta

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RF030, RF070, RF075 |
| Precondições | Dois matches do mesmo Candidato com Empresas diferentes, cada conversa com uma mensagem; outro Candidato sem matches. |
| Dados | Suspensão da segunda Empresa pelo Administrador. |
| Passos | SM-14a: abrir a primeira conversa e encerrá-la («Encerrar»); enviar uma mensagem pela API; suspender a segunda Empresa (pedido direto); abrir a segunda conversa.<br>SM-14b: o Candidato sem matches abre «Conversas» e envia uma mensagem pela API para a conversa de outro match. |
| Resultados esperados | «Encerrou esta conversa. Pode consultar o histórico…», com a mensagem anterior visível e o campo desativado; a API responde 409 (ERR-36); a segunda conversa mostra «A conversa foi encerrada porque a conta da Empresa foi suspensa.», com o histórico e o campo desativado.<br>«Ainda não tem conversas.»; a API recusa a mensagem. |
| Testes | `sm_12_15_matches_conversas.dart` |

### SM-15 — Navegação em 3 interações

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RNF011 |
| Precondições | Candidato registado pela API, com sessão iniciada no ecrã principal. |
| Dados | Contagem de toques e gestos do teste. |
| Passos | SM-15a: a partir do ecrã principal, abrir «Perfil», «Explorar», «Matches» e «Conversas». |
| Resultados esperados | Cada área aparece com 3 interações, no máximo. |
| Testes | `sm_12_15_matches_conversas.dart` |

### SM-16 — Cartão seguinte em 2 segundos

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RNF002 |
| Precondições | Candidato em Aveiro; duas vagas em Aveiro. |
| Dados | `Stopwatch` iniciado no toque e parado quando aparece o cartão seguinte; o tempo de cada ação é escrito na saída. |
| Passos | SM-16a: «Recusar»; «Tenho interesse». |
| Resultados esperados | O cartão seguinte aparece em 2 segundos, no máximo, depois de cada ação. |
| Testes | `sm_07_11_16_exploracao.dart` |

### SM-17 — Falha de ligação

| Campo | Conteúdo |
| --- | --- |
| Requisitos | RNF014 |
| Precondições | Aplicação apontada para `http://10.255.255.1:5180` (rede que não responde). |
| Dados | Credenciais quaisquer; `Stopwatch` iniciado no toque em «Entrar», depois de preenchidos os campos, e parado quando aparece a mensagem; o tempo é escrito na saída. |
| Passos | SM-17a: preencher o correio eletrónico e a palavra-passe; «Entrar». |
| Resultados esperados | «Sem ligação ao servidor. Verifique a ligação e tente novamente.» em 10 segundos, no máximo, a contar do toque; não entra. |
| Testes | `sm_17_falha_ligacao.dart` |

## 5. Rastreabilidade

| Cenário | Requisitos |
| --- | --- |
| `SM-01` | RF003, RF005 |
| `SM-02` | RF004 |
| `SM-03` | RF001, RF105 |
| `SM-04` | RF002 |
| `SM-05` | RF006 a RF009 |
| `SM-06` | RF010 a RF012, RNF016 |
| `SM-07` | RF013 a RF015 |
| `SM-08` | RF016, RF017 |
| `SM-09` | RF018, RF019, RF022, RNF010 |
| `SM-10` | RF020, RF022 |
| `SM-11` | RF023 |
| `SM-12` | RF024, RF025, RF068 |
| `SM-13` | RF026 a RF029, RF108 |
| `SM-14` | RF030, RF070, RF075 |
| `SM-15` | RNF011 |
| `SM-16` | RNF002 |
| `SM-17` | RNF014 |

## 6. Limitações

- A reposição da quota ao fim de 24 horas (RF021) não é automatizada neste nível, como prevê o plano (secção 7): é verificada no nível unitário (controller da exploração com relógio fixo), nos testes do backend e na simulação dos testes de aceitação.
- O seletor de ficheiros nativo não é controlável pelo teste e é substituído pelo `QueuedFilePicker`; o armazenamento seguro da sessão é substituído por um em memória (D1). A abertura do correio, do telefone e do sítio da Empresa nas aplicações do dispositivo não é verificada (só os dados apresentados).
- Os tempos do SM-13, do SM-16 e do SM-17 são medidos no emulador da pipeline e incluem o tempo da API do job. No SM-17, o limite de 10 s (RNF014) tem pouca folga sobre o limite de tempo do cliente HTTP (9 s): na revisão, medido a partir do toque num emulador local, ficou entre 9 790 e 9 859 ms.
