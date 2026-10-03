# Especificação de Requisitos — Katch

**Unidade curricular:** Laboratório de Desenvolvimento de Software — LDS  
**Milestone:** `m2`  
**Ficheiro:** `m2-especificacao-requisitos-v01.md`  
**Pasta de arquivo:** `04-artefactos-tecnicos/04.03-requisitos-e-rtm`

---

## 1. Identificação do documento

| Campo | Informação |
| --- | --- |
| Grupo | G11 |
| Sistema | Katch |
| Ano letivo | 2026/2027 |
| Curso | LEI — Licenciatura em Engenharia Informática |
| Turma | LEI3T2 |
| Versão do documento | v02 |
| Data | 02 de outubro de 2026 |
| Critérios de qualidade aplicados | `m1-criterios-qualidade-requisitos-v01.md`, nos termos da secção 2.1 deste documento |
| Documentos de origem | `m1-proposta-sistema-v02.pdf` e `m1-declaracao-ambito-v02.pdf`, homologados com reservas pelo docente em 24-09-2026 (`m1-email-homologacao-ps-da-cca-v01.pdf`) |

### 1.1. Responsáveis por secção

| Secção | Issue | Executor | Revisor | Auditor |
| --- | --- | --- | --- | --- |
| 4.1. Requisitos funcionais do Candidato | `I020` | João Coelho | Roberto Baptista | João Borguem |
| 4.2. Requisitos funcionais do Recrutador | `I021` | Roberto Baptista | João Borguem | João Coelho |
| 4.3. Requisitos funcionais do Administrador | `I022` | Miguel Santos | João Borguem | João Coelho |
| 5. Requisitos não funcionais | `I023` | A preencher com a atribuição do Sprint Planning do Sprint S02 | A preencher | A preencher |
| 6. Restrições | A definir no Backlog Refinement | A preencher | A preencher | A preencher |

### 1.2. Histórico de versões

| Versão | Data | Descrição das alterações | Reunião e ata de aprovação |
| --- | --- | --- | --- |
| v01 | 29 de setembro de 2026 | Primeira versão: requisitos funcionais RF001 a RF118 das Issues I020, I021 e I022. RF105 a RF113 foram acrescentados na autoverificação; RF114 a RF118 foram acrescentados no tratamento das não conformidades detetadas na primeira aplicação da checklist, que também reformulou os requisitos não aprovados e uniformizou a redação. | Avaliação registada na `m2-checklist-aprovacao-requisitos-v01.xlsx`; aprovação formal a registar na ata do Sprint Review do Sprint S01 (30-09-2026) |
| v02 | 02 de outubro de 2026 | Acrescento dos requisitos não funcionais RNF001 a RNF018 da Issue I023 (secção 5), dos termos complementares necessários à sua verificação (secção 2.2) e dos parâmetros P18 a P21 (secção 3). | Avaliação registada na `[nome do ficheiro de aprovação]`; aprovação formal a registar na ata do Sprint Review do Sprint S02 (07-02-2026) |

---

## 2. Objeto e âmbito

Este documento especifica os requisitos do sistema Katch de forma verificável, consistente e rastreável, nos termos da secção 17 do Regulamento de Funcionamento da Unidade Curricular e do documento `m1-criterios-qualidade-requisitos-v01.md`. A versão v01 contém os requisitos funcionais dos três atores. Os requisitos não funcionais de desempenho, segurança, usabilidade, disponibilidade e manutenibilidade são acrescentados na secção 5 pela Issue `I023`, e as restrições tecnológicas e normativas na secção 6.

Todos os requisitos derivam das funcionalidades `F001` a `F011` da Proposta de Sistema v02 e respeitam o âmbito e os elementos excluídos da Declaração de Âmbito v02. A identificação, a estrutura de cada requisito, a redação do enunciado e a terminologia seguem as secções 3.1 a 3.5 dos critérios de qualidade, com as regras de aplicação fixadas na secção 2.1.

### 2.1. Convenções e regras de aplicação dos critérios

1. **Estrutura dos requisitos.** Cada requisito é apresentado numa tabela com os onze campos obrigatórios da secção 3.2 dos critérios de qualidade.
2. **Funcionalidades de origem.** A Proposta de Sistema v02 homologada contém as funcionalidades `F001` a `F011`. As referências dos critérios de qualidade ao intervalo `F001` a `F010` (secção 2.2, regra 1 de `CQ-01`, regra 1 de `CQ-05` e regra 2 de `CQ-08`) são aplicadas ao conjunto `F001` a `F011`. A Proposta de Sistema homologada é a referência do projeto nos termos da secção 7 do Regulamento de Funcionamento da Unidade Curricular, que prevalece sobre os critérios de qualidade (secção 2.1 dos critérios). A correção desse intervalo nos critérios fica pendente de uma nova versão desse documento.
3. **Redação do enunciado.** A estrutura funcional da secção 3.3 dos critérios é aplicada nas três formas seguintes:
   - requisito com ator: `O sistema deve permitir que <ator> <ação> <objeto>, <condições e regras aplicáveis>.`;
   - proibição dirigida a um ator: `O sistema deve impedir que <ator> <ação> <objeto>, <condições e regras aplicáveis>.`, forma utilizada no requisito conforme do Anexo B.1 dos critérios;
   - requisito sem ator direto (campo `Ator` com o valor `Não aplicável`, previsto na secção 3.2 dos critérios): `O sistema deve <ação> <objeto>, <condições e regras aplicáveis>.`
4. **Valores e parâmetros.** Os limites de formato, dimensão, número e comprimento constam da secção 3. Cada requisito que aplica um parâmetro indica o valor no próprio enunciado e o código do parâmetro no campo `Origem`.
5. **Terminologia.** Os termos do domínio têm o significado fixado no glossário controlado (secção 3.4 dos critérios de qualidade). Os termos do domínio que o glossário não contempla têm o significado fixado na secção 2.2 deste documento, com a mesma força vinculativa, e ficam propostos para inclusão no glossário na próxima versão dos critérios.
6. **Recusa e rejeição.** O termo «recusa» é utilizado apenas com o significado do glossário (ação do Candidato sobre uma vaga ou do Recrutador sobre um Candidato) e no termo composto «recusa do registo da Empresa» da secção 2.2. A não aceitação, pelo sistema, de dados ou operações que não cumprem uma regra designa-se «rejeição».

### 2.2. Termos complementares do domínio

| Termo | Significado fixado |
| --- | --- |
| Aplicação móvel | Ponto de acesso exclusivo do Candidato. |
| Área de gestão web | Ponto de acesso do Recrutador e do Administrador. |
| Área de exploração de vagas | Parte da aplicação móvel onde o Candidato consulta os cartões de vaga, recusa vagas, manifesta interesse e ajusta as preferências de procura. |
| Área de supervisão | Parte da área de gestão web reservada ao Administrador (F009). |
| Área de notificações | Parte da aplicação móvel e da área de gestão web onde o destinatário consulta as suas notificações e o número de notificações por ler. |
| Sessão iniciada | Estado de um utilizador autenticado no ponto de acesso do respetivo tipo de conta. |
| Tipo de conta | Candidato, Recrutador ou Administrador. Determina o ponto de acesso e as operações autorizadas (F001). |
| Estado da conta | `ativa`, `bloqueada` ou `suspensa`. O estado `bloqueada` resulta de decisão do Administrador sobre uma conta de Candidato ou de Recrutador (F001, RF086). O estado `suspensa` resulta de decisão do Administrador sobre uma conta de Candidato (F002, RF089). Os dois estados têm o mesmo efeito: impedem o início de sessão e passam as conversas da conta ao modo apenas de consulta. Distinguem-se pela funcionalidade de origem e pela operação de reativação (RF087 e RF090). |
| Estado da Empresa | `pendente`, `aprovada`, `recusada` ou `suspensa`. |
| Recusa do registo da Empresa | Decisão do Administrador que indefere o registo de uma Empresa pendente, com motivo, passando a Empresa ao estado `recusada`. |
| Relação entre Recrutador e Empresa | Cada Recrutador representa uma única Empresa e cada Empresa é representada por um único Recrutador, em consequência da unicidade do número de identificação fiscal (RF042). |
| Recrutador responsável pela vaga | Recrutador que representa a Empresa a que a vaga pertence. |
| Estado da vaga | `não publicada`, `publicada`, `suspensa` ou `encerrada`. |
| Candidato em espera | Candidato que manifestou interesse numa vaga e sobre o qual o Recrutador ainda não registou aceitação nem recusa. |
| Perfil profissional | Conjunto dos dados profissionais, competências, hiperligações, fotografia, curriculum vitae e preferências de procura mantido pelo Candidato (F004). |
| Perfil completo do Candidato | Apresentação do perfil profissional ao Recrutador, para uma vaga em que o Candidato está em espera (F007). |
| Preferências de procura | Distância máxima, pretensão salarial mínima, regimes de trabalho e tipos de contrato pretendidos pelo Candidato. |
| Localidade do Candidato | Localidade indicada no registo (RF003), alterável no perfil profissional (RF006), e utilizada no cálculo da distância (RF015). |
| Lista pré-definida | Lista fechada de valores de referência: localidades, carregada inicialmente no sistema e não editável através da plataforma (Declaração de Âmbito, F002); competências e benefícios, mantidas pelo Administrador (F009). |
| Conversa | Canal de comunicação único associado a um match e à vaga que lhe deu origem, com o histórico das mensagens (F010). |
| Mensagem | Texto enviado numa conversa pelo Candidato ou pelo Recrutador responsável pela vaga. |
| Modo apenas de consulta | Estado de uma conversa em que o histórico continua visível para as duas partes e o envio de mensagens é rejeitado. |
| Notificação | Aviso gerado pelo sistema para um destinatário, apresentado na área de notificações e com acesso direto ao elemento a que se refere (F011). |
| Rejeição | Não aceitação, pelo sistema, de dados ou de uma operação que não cumpre uma regra, com indicação do motivo quando o requisito o exija. |
| Ambiente de demonstração | Instalação do sistema nos equipamentos do grupo, em rede local, com os dados de demonstração carregados, onde são verificados os requisitos não funcionais. |
| Dados de demonstração | Dados fictícios carregados pelos scripts de dados iniciais, com os volumes mínimos fixados no parâmetro `P18`. |
| Interface do servidor | Conjunto das operações que o Backend disponibiliza à aplicação móvel e à área de gestão web. |
| Pedido direto | Pedido enviado à interface do servidor por um meio diferente da aplicação móvel e da área de gestão web, designadamente pela coleção de testes da interface do servidor. |
| Operação reservada | Operação da interface do servidor que exige sessão iniciada. São operações não reservadas apenas o registo de Candidato, a criação de conta de Recrutador e o início de sessão. |
| Credencial de sessão | Elemento emitido pelo sistema no início de sessão e apresentado em cada pedido seguinte para identificar o utilizador e o tipo de conta. |
| Tempo de resposta | Intervalo entre o envio de um pedido à interface do servidor e a receção da resposta completa. |
| Interação | Um toque, um clique ou um gesto do utilizador sobre a aplicação móvel ou a área de gestão web. |
| Registo de diagnóstico | Registo técnico do funcionamento produzido pelo servidor, distinto do registo de autoria das operações relevantes (RF112). |

---

## 3. Parâmetros fixados

A Proposta de Sistema e a Declaração de Âmbito referem limites de formato, dimensão, número e comprimento sem lhes atribuir valor, com exceção da quota de interesses e do período de bloqueio (P11), fixados na Proposta de Sistema v02. Para cumprir as regras 4 e 5 do critério `CQ-03`, a tabela seguinte fixa esses valores, aprovados com esta especificação. Os parâmetros `P18` a `P21` fixam as condições de medição e os valores-limite dos requisitos não funcionais (regra 3 de `CQ-07`). Os requisitos que os aplicam indicam o valor no próprio enunciado e o código do parâmetro no campo `Origem`. Uma alteração a um parâmetro obriga à revisão de todos os requisitos que o referem.

| Parâmetro | Matéria | Valor fixado | Funcionalidades |
| --- | --- | --- | --- |
| `P01` | Imagens (fotografia de perfil, fotografias da galeria e da vaga) | JPEG ou PNG; máximo 5 MB por ficheiro; logótipo: JPEG ou PNG, máximo 2 MB | F003, F004, F005 |
| `P02` | Curriculum vitae | PDF; máximo 5 MB | F004 |
| `P03` | Mensagem de texto | 1 a 1000 caracteres | F010 |
| `P04` | Formatos de dados de registo | Correio eletrónico no formato local@domínio; contacto telefónico com 9 algarismos; número de identificação fiscal com 9 algarismos | F001, F002, F003 |
| `P05` | Palavra-passe | Mínimo de 8 caracteres, com pelo menos uma letra e um algarismo | F001, F002 |
| `P06` | Distância máxima do Candidato | 1 a 500 km, em números inteiros | F004, F006 |
| `P07` | Competência introduzida pela opção "Outro" | 2 a 40 caracteres; apenas letras, algarismos, espaços e os símbolos + # . - | F004 |
| `P08` | Textos longos (resumo da experiência, descrição da Empresa) | Máximo 1000 caracteres | F003, F004 |
| `P09` | Tipos de contrato | Sem termo; A termo; Estágio; Prestação de serviços | F004, F005, F006 |
| `P10` | Hiperligações | Endereço iniciado por http:// ou https://; máximo 3 hiperligações profissionais por Candidato; 1 endereço do sítio na Internet por Empresa | F003, F004 |
| `P11` | Quota de interesses e período de bloqueio | 10 interesses; 24 horas contadas do instante do último interesse que esgota a quota; reposição integral (fixado na Proposta de Sistema v02) | F006, F011 |
| `P12` | Motivo de recusa do registo da Empresa | 10 a 500 caracteres | F003 |
| `P13` | Galeria e fotografias da vaga | Galeria da Empresa: máximo 6 fotografias; vaga: máximo 5 fotografias | F003, F005 |
| `P14` | Disponibilidade do Candidato | Imediata; 15 dias; 1 mês; A combinar | F004 |
| `P15` | Setor de atividade da Empresa | Tecnologia; Saúde; Comércio; Hotelaria e restauração; Construção; Indústria; Educação; Finanças; Logística; Serviços; Outro | F003 |
| `P16` | Regimes de trabalho | Presencial; Híbrido; Remoto | F004, F005, F006 |
| `P17` | Apresentação sem atualização manual | Mensagens e notificações apresentadas ao destinatário no máximo 5 segundos após o acontecimento que as origina, com a aplicação aberta e sessão iniciada, em ambiente de demonstração | F010, F011 |
| `P18` | Dados de demonstração | Mínimo de 20 Candidatos com conta no estado ativa, 5 Empresas no estado aprovada, 2 Empresas no estado pendente e 50 vagas no estado publicada. Todos os dados são fictícios | F001, F002, F003, F004, F005, F006, F007, F008, F009, F010, F011 |
| `P19` | Limites de tempo | Operação da interface do servidor, excluindo o carregamento de ficheiros: 2 segundos; carregamento de um ficheiro de 5 MB: 5 segundos; cartão de vaga seguinte: 2 segundos; indicação de falha de ligação: 10 segundos | F002, F003, F004, F005, F006, F007, F008, F009, F010, F011 |
| `P20` | Validade da credencial de sessão | 8 horas contadas desde o início de sessão | F001 |
| `P21` | Interações e apresentação | Ações sobre o cartão de vaga: 1 interação; acesso às áreas da aplicação móvel: 3 interações; larguras de janela da área de gestão web: 1280 a 1920 píxeis | F001, F003, F004, F005, F006, F007, F008, F009, F010, F011 |

---

## 4. Requisitos funcionais

### 4.1. Requisitos funcionais do Candidato (Issue `I020`)

Requisitos `RF001` a `RF036` e `RF105`, `RF108`, `RF110`. Os identificadores acrescentados depois da numeração inicial mantêm a ordem de criação (secção 3.1 dos critérios). Os requisitos sem ator direto incluídos nesta secção decorrem da mesma Issue.

#### RF001 — Iniciar sessão na aplicação móvel

| Campo | Conteúdo |
| --- | --- |
| ID | `RF001` |
| Título | Iniciar sessão na aplicação móvel |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato inicie sessão na aplicação móvel indicando o endereço de correio eletrónico e a palavra-passe, rejeitando o acesso, com indicação do motivo, quando as credenciais não coincidirem com as registadas ou a conta estiver no estado bloqueada ou suspensa. |
| Funcionalidade de origem | F001 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem conta registada. |
| Critério de aceitação | Com credenciais corretas e conta no estado ativa, a sessão é iniciada; com palavra-passe errada, ou com a conta no estado bloqueada ou suspensa, a sessão não é iniciada e é apresentado o motivo. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F001); Declaração de Âmbito v02 (F001) |

#### RF002 — Alterar a própria palavra-passe

| Campo | Conteúdo |
| --- | --- |
| ID | `RF002` |
| Título | Alterar a própria palavra-passe |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato altere a sua palavra-passe indicando a palavra-passe atual e a nova palavra-passe, rejeitando a alteração, com indicação do motivo, quando a palavra-passe atual estiver errada ou a nova palavra-passe não tiver pelo menos 8 caracteres, com pelo menos uma letra e um algarismo. |
| Funcionalidade de origem | F001 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada na aplicação móvel. |
| Critério de aceitação | Com a palavra-passe atual correta e a nova palavra-passe «katch2026», o início de sessão seguinte só é aceite com a nova palavra-passe; com a palavra-passe atual errada, ou com a nova palavra-passe «katch», a alteração é rejeitada. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F001); Declaração de Âmbito v02 (F001); parâmetros P05 da secção 3 |

#### RF003 — Registar-se como Candidato

| Campo | Conteúdo |
| --- | --- |
| ID | `RF003` |
| Título | Registar-se como Candidato |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato se registe livremente a partir da aplicação móvel, indicando nome, endereço de correio eletrónico, contacto telefónico, localidade selecionada da lista pré-definida de localidades, palavra-passe e aceitação das condições de utilização. |
| Funcionalidade de origem | F002 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | Não aplicável |
| Critério de aceitação | Um registo com todos os campos preenchidos e conformes com a validação automática é aceite e fica associado à localidade selecionada e às respetivas coordenadas. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F002); Declaração de Âmbito v02 (F002); parâmetros P04, P05 da secção 3 |

#### RF004 — Impedir registo de Candidato sem validação automática

| Campo | Conteúdo |
| --- | --- |
| ID | `RF004` |
| Título | Impedir registo de Candidato sem validação automática |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve impedir que o Candidato conclua o registo sem cumprir a validação automática, por campo obrigatório vazio, endereço de correio eletrónico fora do formato local@domínio ou já registado, contacto telefónico sem 9 algarismos ou palavra-passe sem pelo menos 8 caracteres, uma letra e um algarismo, indicando ao Candidato cada campo em causa e o motivo. |
| Funcionalidade de origem | F002 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | Não aplicável |
| Critério de aceitação | Um registo com o endereço de correio eletrónico já existente, com o contacto telefónico com 8 algarismos ou com a localidade por preencher é rejeitado e são apresentados o campo e o motivo. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F002); Declaração de Âmbito v02 (F002); parâmetros P04, P05 da secção 3 |

#### RF005 — Ativar de imediato a conta do Candidato

| Campo | Conteúdo |
| --- | --- |
| ID | `RF005` |
| Título | Ativar de imediato a conta do Candidato |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato inicie sessão imediatamente após um registo que cumpra a validação automática, ficando a conta no estado ativa sem aprovação manual do Administrador. |
| Funcionalidade de origem | F002 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O registo cumpriu a validação automática. |
| Critério de aceitação | Imediatamente após um registo válido, o Candidato inicia sessão com as credenciais indicadas e a conta tem o estado ativa, sem qualquer ação do Administrador. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F002); Declaração de Âmbito v02 (F002) |

#### RF006 — Gravar os dados do perfil profissional

| Campo | Conteúdo |
| --- | --- |
| ID | `RF006` |
| Título | Gravar os dados do perfil profissional |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato grave no seu perfil profissional os campos seguintes: função pretendida, localidade selecionada da lista pré-definida de localidades, disponibilidade entre imediata, 15 dias, 1 mês e a combinar, e resumo da experiência com um máximo de 1000 caracteres. |
| Funcionalidade de origem | F004 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada na aplicação móvel. |
| Critério de aceitação | Os quatro campos gravados são apresentados com o mesmo conteúdo na consulta seguinte do perfil profissional; um resumo com 1001 caracteres é rejeitado. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F004); Declaração de Âmbito v02 (F004); parâmetros P08, P14 da secção 3 |

#### RF007 — Definir as preferências de procura

| Campo | Conteúdo |
| --- | --- |
| ID | `RF007` |
| Título | Definir as preferências de procura |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato defina as preferências de procura seguintes: distância máxima em quilómetros, entre 1 e 500, pretensão salarial mínima em euros brutos mensais, regimes de trabalho pretendidos entre presencial, híbrido e remoto, e tipos de contrato pretendidos entre sem termo, a termo, estágio e prestação de serviços. |
| Funcionalidade de origem | F004 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada na aplicação móvel. |
| Critério de aceitação | As preferências gravadas são apresentadas na consulta seguinte; uma distância máxima de 0 ou de 501 quilómetros é rejeitada. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F004); Declaração de Âmbito v02 (F004); parâmetros P06, P09, P16 da secção 3 |

#### RF008 — Selecionar competências da lista pré-definida

| Campo | Conteúdo |
| --- | --- |
| ID | `RF008` |
| Título | Selecionar competências da lista pré-definida |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato associe ao seu perfil competências selecionadas da lista pré-definida de competências mantida pelo Administrador, apresentando-as como etiquetas. |
| Funcionalidade de origem | F004 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada na aplicação móvel. |
| Critério de aceitação | As competências selecionadas ficam associadas ao perfil e são apresentadas como etiquetas; não é possível selecionar uma competência que não conste da lista. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F004); Declaração de Âmbito v02 (F004) |

#### RF009 — Introduzir competência através da opção Outro

| Campo | Conteúdo |
| --- | --- |
| ID | `RF009` |
| Título | Introduzir competência através da opção Outro |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato introduza uma competência em texto livre através da opção "Outro", aceitando-a apenas quando tiver entre 2 e 40 caracteres e contiver exclusivamente letras, algarismos, espaços e os símbolos + # . -, e rejeitando-a, com indicação do motivo, nos restantes casos. |
| Funcionalidade de origem | F004 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada na aplicação móvel. |
| Critério de aceitação | A competência "C#" é aceite; uma competência com 1 carácter, com 41 caracteres ou com o carácter "@" é rejeitada com indicação do motivo. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F004); Declaração de Âmbito v02 (F004); parâmetros P07 da secção 3 |

#### RF010 — Carregar fotografia de perfil

| Campo | Conteúdo |
| --- | --- |
| ID | `RF010` |
| Título | Carregar fotografia de perfil |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato carregue uma fotografia de perfil opcional nos formatos JPEG ou PNG com dimensão máxima de 5 MB, rejeitando, com indicação do motivo, ficheiros noutro formato ou com dimensão superior. |
| Funcionalidade de origem | F004 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada na aplicação móvel. |
| Critério de aceitação | Uma fotografia PNG de 4 MB é aceite e apresentada no perfil profissional; um ficheiro GIF ou um JPEG de 6 MB é rejeitado com indicação do motivo. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F004); Declaração de Âmbito v02 (F004); parâmetros P01 da secção 3 |

#### RF011 — Enviar curriculum vitae

| Campo | Conteúdo |
| --- | --- |
| ID | `RF011` |
| Título | Enviar curriculum vitae |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato envie um curriculum vitae opcional no formato PDF com dimensão máxima de 5 MB, rejeitando, com indicação do motivo, ficheiros noutro formato ou com dimensão superior. |
| Funcionalidade de origem | F004 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada na aplicação móvel. |
| Critério de aceitação | Um PDF de 3 MB é aceite e fica associado ao perfil profissional; um ficheiro DOCX ou um PDF de 6 MB é rejeitado com indicação do motivo. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F004); Declaração de Âmbito v02 (F004); parâmetros P02 da secção 3 |

#### RF012 — Indicar hiperligações profissionais externas

| Campo | Conteúdo |
| --- | --- |
| ID | `RF012` |
| Título | Indicar hiperligações profissionais externas |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato indique até 3 hiperligações para páginas profissionais externas, aceitando apenas endereços iniciados por "http://" ou "https://" e conservando-os tal como indicados, sem importar conteúdo dessas páginas. |
| Funcionalidade de origem | F004 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada na aplicação móvel. |
| Critério de aceitação | O endereço "https://portefolio.pt" é aceite e apresentado sem alteração; o endereço "portefolio" e uma quarta hiperligação são rejeitados. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F004); Declaração de Âmbito v02 (F004); parâmetros P10 da secção 3 |

#### RF013 — Consultar vagas sob a forma de cartões

| Campo | Conteúdo |
| --- | --- |
| ID | `RF013` |
| Título | Consultar vagas sob a forma de cartões |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato consulte, um de cada vez, cartões de vaga com os dados seguintes: função, designação da Empresa, logótipo da Empresa, intervalo salarial, localidade, distância à localidade do Candidato em quilómetros, tipo de contrato, regime de trabalho, indicação de urgência e fotografias da vaga, sendo o logótipo e as fotografias apresentados apenas quando registados. |
| Funcionalidade de origem | F006 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada na aplicação móvel. |
| Critério de aceitação | Cada cartão apresentado contém os dez dados enumerados que estejam registados para a vaga correspondente; o cartão de uma vaga sem fotografias contém os restantes dados. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F006); Declaração de Âmbito v02 (F006) |

#### RF014 — Selecionar vagas segundo as preferências de procura

| Campo | Conteúdo |
| --- | --- |
| ID | `RF014` |
| Título | Selecionar vagas segundo as preferências de procura |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato consulte, na área de exploração de vagas, apenas as vagas que cumpram cumulativamente as condições seguintes: vaga no estado publicada; Empresa no estado aprovada; vaga não recusada pelo Candidato nem objeto de interesse do Candidato; distância não superior à distância máxima, condição não aplicada às vagas em regime remoto; valor máximo do intervalo salarial não inferior à pretensão salarial mínima; regime de trabalho e tipo de contrato incluídos nas preferências de procura. |
| Funcionalidade de origem | F006 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada na aplicação móvel. |
| Critério de aceitação | Com distância máxima de 20 km e pretensão salarial mínima de 1200 euros: uma vaga presencial a 30 km não é apresentada; uma vaga remota a 300 km é apresentada; uma vaga com intervalo salarial de 900 a 1100 euros não é apresentada; uma vaga com regime de trabalho ou tipo de contrato não incluído nas preferências não é apresentada; uma vaga suspensa, uma vaga de Empresa suspensa e uma vaga já recusada não são apresentadas. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F006); Declaração de Âmbito v02 (F006); parâmetros P06 da secção 3 |

#### RF015 — Calcular a distância em linha reta

| Campo | Conteúdo |
| --- | --- |
| ID | `RF015` |
| Título | Calcular a distância em linha reta |
| Tipo | Funcional |
| Ator | Não aplicável |
| Descrição | O sistema deve calcular a distância entre o Candidato e a vaga em linha reta, em quilómetros arredondados às unidades, a partir das coordenadas da localidade do Candidato e das coordenadas da localidade da vaga. |
| Funcionalidade de origem | F006 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | As localidades do Candidato e da vaga constam da lista pré-definida de localidades. |
| Critério de aceitação | Para duas localidades com coordenadas conhecidas, a distância apresentada coincide, com diferença máxima de 1 km, com a distância em linha reta calculada a partir dessas coordenadas. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F006); Declaração de Âmbito v02 (F006) |

#### RF016 — Consultar o detalhe de uma vaga

| Campo | Conteúdo |
| --- | --- |
| ID | `RF016` |
| Título | Consultar o detalhe de uma vaga |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato abra o detalhe de uma vaga apresentada, com a descrição completa, as competências pretendidas, os benefícios e o acesso à página de apresentação da Empresa. |
| Funcionalidade de origem | F006 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada na aplicação móvel. |
| Critério de aceitação | O detalhe de uma vaga apresenta a descrição, as competências e os benefícios registados pelo Recrutador e um acesso que abre a página da Empresa. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F006); Declaração de Âmbito v02 (F006) |

#### RF017 — Consultar a página de apresentação da Empresa

| Campo | Conteúdo |
| --- | --- |
| ID | `RF017` |
| Título | Consultar a página de apresentação da Empresa |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato consulte, a partir do detalhe de uma vaga, a página de apresentação da Empresa com o logótipo, a descrição da atividade, o endereço do sítio na Internet e a galeria de fotografias. |
| Funcionalidade de origem | F003 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada na aplicação móvel. |
| Critério de aceitação | A página apresentada contém os quatro elementos registados pelo Recrutador da Empresa da vaga. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F003); Declaração de Âmbito v02 (F003) |

#### RF018 — Recusar uma vaga

| Campo | Conteúdo |
| --- | --- |
| ID | `RF018` |
| Título | Recusar uma vaga |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato recuse uma vaga apresentada, sem limite de número de recusas e sem consumo da quota de interesses. |
| Funcionalidade de origem | F006 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada na aplicação móvel. |
| Critério de aceitação | Após 15 recusas consecutivas, as 15 recusas ficam registadas e o número de interesses disponíveis mantém-se inalterado. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F006); Declaração de Âmbito v02 (F006) |

#### RF019 — Manifestar interesse numa vaga

| Campo | Conteúdo |
| --- | --- |
| ID | `RF019` |
| Título | Manifestar interesse numa vaga |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato manifeste interesse numa vaga apresentada, uma única vez por vaga, rejeitando uma segunda manifestação de interesse na mesma vaga. |
| Funcionalidade de origem | F006 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada e dispõe de, pelo menos, um interesse na quota. |
| Critério de aceitação | Após o interesse, o Candidato surge na lista de candidatos em espera dessa vaga; uma segunda manifestação de interesse na mesma vaga é rejeitada. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F006); Declaração de Âmbito v02 (F006); parâmetros P11 da secção 3 |

#### RF020 — Bloquear novos interesses após esgotar a quota

| Campo | Conteúdo |
| --- | --- |
| ID | `RF020` |
| Título | Bloquear novos interesses após esgotar a quota |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve impedir que o Candidato manifeste um novo interesse quando tiver esgotado a quota de interesses, durante as vinte e quatro horas seguintes ao instante em que o último interesse foi registado, apresentando-lhe o tempo em falta até ao fim do período de bloqueio. |
| Funcionalidade de origem | F006 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada e consumiu a totalidade da quota de interesses. |
| Critério de aceitação | Com a quota esgotada há menos de vinte e quatro horas, uma tentativa de manifestar interesse é rejeitada e é apresentado o tempo em falta; decorridas vinte e quatro horas sobre o último interesse registado, a mesma tentativa é aceite. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F006); Declaração de Âmbito v02 (F006); parâmetros P11 da secção 3 |

#### RF021 — Repor integralmente a quota de interesses

| Campo | Conteúdo |
| --- | --- |
| ID | `RF021` |
| Título | Repor integralmente a quota de interesses |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato volte a dispor de 10 interesses no instante em que termina o período de bloqueio, sem reposição parcial da quota e sem reposição antes de a quota estar esgotada. |
| Funcionalidade de origem | F006 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato esgotou a quota de interesses. |
| Critério de aceitação | Decorridas vinte e quatro horas sobre o último interesse, o número de interesses disponíveis é 10; com 3 interesses consumidos, o número não aumenta com o decurso do tempo. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F006); Declaração de Âmbito v02 (F006); parâmetros P11 da secção 3 |

#### RF022 — Consultar os interesses disponíveis na quota

| Campo | Conteúdo |
| --- | --- |
| ID | `RF022` |
| Título | Consultar os interesses disponíveis na quota |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato consulte, na área de exploração de vagas, o número de interesses disponíveis, igual a 10 menos o número de interesses manifestados desde a última reposição da quota de interesses. |
| Funcionalidade de origem | F006 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada na aplicação móvel. |
| Critério de aceitação | Com 4 interesses manifestados desde a última reposição, é apresentado o número 6. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F006); Declaração de Âmbito v02 (F006); parâmetros P11 da secção 3 |

#### RF023 — Ajustar preferências na área de exploração

| Campo | Conteúdo |
| --- | --- |
| ID | `RF023` |
| Título | Ajustar preferências na área de exploração |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato altere as preferências de procura a partir da área de exploração de vagas, aplicando os novos valores às vagas apresentadas a seguir à alteração. |
| Funcionalidade de origem | F006 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada na aplicação móvel. |
| Critério de aceitação | Após reduzir a distância máxima de 50 para 10 km na área de exploração, a vaga seguinte apresentada está a 10 km ou menos, ou é remota. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F006); Declaração de Âmbito v02 (F006); parâmetros P06 da secção 3 |

#### RF024 — Consultar a lista de matches

| Campo | Conteúdo |
| --- | --- |
| ID | `RF024` |
| Título | Consultar a lista de matches |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato consulte a lista dos seus matches, com a identificação da vaga e da Empresa de cada match, incluindo os matches de vagas encerradas ou de Empresas suspensas. |
| Funcionalidade de origem | F008 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada na aplicação móvel. |
| Critério de aceitação | A lista apresenta todos os matches do Candidato; um match cuja vaga foi encerrada continua na lista. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F008); Declaração de Âmbito v02 (F008) |

#### RF025 — Consultar os dados de contacto após match

| Campo | Conteúdo |
| --- | --- |
| ID | `RF025` |
| Título | Consultar os dados de contacto após match |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato consulte o endereço de correio eletrónico e o contacto telefónico da Empresa indicados no registo da Empresa, apenas para Empresas com as quais exista match. |
| Funcionalidade de origem | F008 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada e existe um match entre o Candidato e a Empresa. |
| Critério de aceitação | Com match, os contactos da Empresa são apresentados; sem match com essa Empresa, os contactos não são apresentados. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F008); Declaração de Âmbito v02 (F008) |

#### RF026 — Enviar mensagem ao Recrutador

| Campo | Conteúdo |
| --- | --- |
| ID | `RF026` |
| Título | Enviar mensagem ao Recrutador |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato envie ao Recrutador responsável pela vaga uma mensagem de texto com 1 a 1000 caracteres, na conversa associada ao match, rejeitando o envio quando a mensagem estiver vazia, exceder 1000 caracteres ou a conversa estiver no modo apenas de consulta. |
| Funcionalidade de origem | F010 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada e existe um match entre o Candidato e a Empresa. |
| Critério de aceitação | Uma mensagem com 20 caracteres é entregue e fica no histórico; uma mensagem vazia, com 1001 caracteres ou numa conversa no modo apenas de consulta é rejeitada. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F010); Declaração de Âmbito v02 (F010); parâmetros P03 da secção 3 |

#### RF027 — Consultar a lista de conversas

| Campo | Conteúdo |
| --- | --- |
| ID | `RF027` |
| Título | Consultar a lista de conversas |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato consulte a lista das suas conversas, acessível também a partir da lista de matches, com o número de mensagens por ler em cada conversa. |
| Funcionalidade de origem | F010 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada na aplicação móvel. |
| Critério de aceitação | Com 2 mensagens recebidas e não abertas numa conversa, essa conversa apresenta o número 2. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F010); Declaração de Âmbito v02 (F010) |

#### RF028 — Consultar o histórico de uma conversa

| Campo | Conteúdo |
| --- | --- |
| ID | `RF028` |
| Título | Consultar o histórico de uma conversa |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato consulte o histórico de uma conversa, com o texto, a data e hora de envio de cada mensagem e a indicação de leitura pelo destinatário. |
| Funcionalidade de origem | F010 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada e existe um match entre o Candidato e a Empresa. |
| Critério de aceitação | O histórico apresenta todas as mensagens trocadas por ordem de envio, com data, hora e indicação de leitura. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F010); Declaração de Âmbito v02 (F010) |

#### RF029 — Receber mensagens sem atualização manual

| Campo | Conteúdo |
| --- | --- |
| ID | `RF029` |
| Título | Receber mensagens sem atualização manual |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato receba na conversa aberta cada nova mensagem do Recrutador sem atualização manual da aplicação, no máximo 5 segundos após o envio, enquanto a aplicação móvel estiver aberta com sessão iniciada. |
| Funcionalidade de origem | F010 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada e existe um match entre o Candidato e a Empresa. |
| Critério de aceitação | Com a conversa aberta, uma mensagem enviada pelo Recrutador surge na conversa em 5 segundos ou menos, sem qualquer ação do Candidato. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F010); Declaração de Âmbito v02 (F010); parâmetros P17 da secção 3 |

#### RF030 — Encerrar uma conversa

| Campo | Conteúdo |
| --- | --- |
| ID | `RF030` |
| Título | Encerrar uma conversa |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato encerre uma conversa, passando essa conversa ao modo apenas de consulta para o Candidato e para o Recrutador. |
| Funcionalidade de origem | F010 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada e existe um match entre o Candidato e a Empresa. |
| Critério de aceitação | Após o encerramento, o histórico continua visível para as duas partes e qualquer tentativa de envio de mensagem é rejeitada. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F010); Declaração de Âmbito v02 (F010) |

#### RF031 — Notificar o Candidato de novo match

| Campo | Conteúdo |
| --- | --- |
| ID | `RF031` |
| Título | Notificar o Candidato de novo match |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato receba na área de notificações uma notificação que identifica a vaga e a Empresa, no instante em que é confirmado um match com esse Candidato. |
| Funcionalidade de origem | F011 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | Existe interesse do Candidato numa vaga. |
| Critério de aceitação | Após a aceitação do Candidato pelo Recrutador, surge na área de notificações do Candidato uma notificação que identifica a vaga e a Empresa. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F011); Declaração de Âmbito v02 (F011) |

#### RF032 — Notificar o Candidato de nova mensagem

| Campo | Conteúdo |
| --- | --- |
| ID | `RF032` |
| Título | Notificar o Candidato de nova mensagem |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato receba na área de notificações uma notificação que identifica a conversa, no instante em que o Recrutador lhe envia uma nova mensagem. |
| Funcionalidade de origem | F011 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada e existe um match entre o Candidato e a Empresa. |
| Critério de aceitação | Após o envio de uma mensagem pelo Recrutador, surge na área de notificações do Candidato uma notificação que identifica a conversa. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F011); Declaração de Âmbito v02 (F011) |

#### RF033 — Notificar o Candidato da reposição da quota

| Campo | Conteúdo |
| --- | --- |
| ID | `RF033` |
| Título | Notificar o Candidato da reposição da quota |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato receba na área de notificações uma notificação de reposição da quota, no instante em que a quota de interesses é reposta em 10 interesses. |
| Funcionalidade de origem | F011 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato esgotou a quota de interesses. |
| Critério de aceitação | Decorridas vinte e quatro horas sobre o último interesse que esgotou a quota, surge uma notificação de reposição na área de notificações do Candidato. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F011); Declaração de Âmbito v02 (F011); parâmetros P11 da secção 3 |

#### RF034 — Consultar a área de notificações

| Campo | Conteúdo |
| --- | --- |
| ID | `RF034` |
| Título | Consultar a área de notificações |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato consulte a área de notificações com as notificações recebidas e o número de notificações por ler. |
| Funcionalidade de origem | F011 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada na aplicação móvel. |
| Critério de aceitação | Com 3 notificações não lidas, a área apresenta as notificações e o número 3. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F011); Declaração de Âmbito v02 (F011) |

#### RF035 — Marcar notificação como lida

| Campo | Conteúdo |
| --- | --- |
| ID | `RF035` |
| Título | Marcar notificação como lida |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato marque uma notificação como lida, reduzindo em uma unidade o número de notificações por ler. |
| Funcionalidade de origem | F011 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada na aplicação móvel. |
| Critério de aceitação | Após marcar uma de 3 notificações por ler, o número apresentado é 2. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F011); Declaração de Âmbito v02 (F011) |

#### RF036 — Aceder ao elemento associado à notificação

| Campo | Conteúdo |
| --- | --- |
| ID | `RF036` |
| Título | Aceder ao elemento associado à notificação |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato aceda, a partir de uma notificação, ao elemento a que a notificação se refere: match, conversa ou área de exploração de vagas. |
| Funcionalidade de origem | F011 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada na aplicação móvel. |
| Critério de aceitação | A seleção de uma notificação de nova mensagem abre a conversa correspondente. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F011); Declaração de Âmbito v02 (F011) |

#### RF105 — Terminar sessão na aplicação móvel

| Campo | Conteúdo |
| --- | --- |
| ID | `RF105` |
| Título | Terminar sessão na aplicação móvel |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato termine a sessão na aplicação móvel, exigindo novo início de sessão para qualquer operação seguinte. |
| Funcionalidade de origem | F001 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada na aplicação móvel. |
| Critério de aceitação | Após terminar a sessão, a abertura da área de exploração de vagas exige novo início de sessão. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F001); Declaração de Âmbito v02 (F001) |

#### RF108 — Marcar como lidas as mensagens ao abrir a conversa

| Campo | Conteúdo |
| --- | --- |
| ID | `RF108` |
| Título | Marcar como lidas as mensagens ao abrir a conversa |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato marque como lidas todas as mensagens do Recrutador contidas numa conversa, através da abertura dessa conversa. |
| Funcionalidade de origem | F010 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada e existe um match entre o Candidato e a Empresa. |
| Critério de aceitação | Antes de o Candidato abrir a conversa, a mensagem surge ao Recrutador como não lida; depois da abertura, surge como lida. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F010); Declaração de Âmbito v02 (F010) |

#### RF110 — Receber notificações sem atualização manual

| Campo | Conteúdo |
| --- | --- |
| ID | `RF110` |
| Título | Receber notificações sem atualização manual |
| Tipo | Funcional |
| Ator | Candidato |
| Descrição | O sistema deve permitir que o Candidato receba cada nova notificação na área de notificações sem atualização manual da aplicação, no máximo 5 segundos após o acontecimento que a origina, enquanto a aplicação móvel estiver aberta com sessão iniciada. |
| Funcionalidade de origem | F011 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada na aplicação móvel. |
| Critério de aceitação | Com a aplicação aberta, após a confirmação de um match, a notificação surge na área de notificações em 5 segundos ou menos, sem qualquer ação do Candidato. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F011); Declaração de Âmbito v02 (F011); parâmetros P17 da secção 3 |

### 4.2. Requisitos funcionais do Recrutador (Issue `I021`)

Requisitos `RF037` a `RF082` e `RF106`, `RF109`, `RF111`, `RF113`, `RF114`, `RF115`, `RF117`, `RF118`. Os identificadores acrescentados depois da numeração inicial mantêm a ordem de criação (secção 3.1 dos critérios). Os requisitos sem ator direto incluídos nesta secção decorrem da mesma Issue.

#### RF037 — Criar conta de Recrutador

| Campo | Conteúdo |
| --- | --- |
| ID | `RF037` |
| Título | Criar conta de Recrutador |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que um Recrutador crie conta na área de gestão web indicando endereço de correio eletrónico no formato local@domínio e único no sistema, palavra-passe com pelo menos 8 caracteres, uma letra e um algarismo, e aceitação das condições de utilização, rejeitando, com indicação do motivo, dados em falta, não conformes ou endereço já registado. |
| Funcionalidade de origem | F001 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | Não aplicável |
| Critério de aceitação | Com dados conformes, a conta é criada; com um endereço de correio eletrónico já registado, a criação é rejeitada com indicação do motivo. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F001); Declaração de Âmbito v02 (F001); parâmetros P04, P05 da secção 3 |

#### RF038 — Iniciar sessão na área de gestão web

| Campo | Conteúdo |
| --- | --- |
| ID | `RF038` |
| Título | Iniciar sessão na área de gestão web |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador inicie sessão na área de gestão web indicando o endereço de correio eletrónico e a palavra-passe, rejeitando o acesso, com indicação do motivo, quando as credenciais não coincidirem com as registadas ou a conta estiver no estado bloqueada. |
| Funcionalidade de origem | F001 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem conta registada. |
| Critério de aceitação | Com credenciais corretas e conta ativa, a sessão é iniciada; com conta bloqueada, a sessão não é iniciada e é apresentado o motivo. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F001); Declaração de Âmbito v02 (F001) |

#### RF039 — Impedir operações reservadas sem Empresa aprovada

| Campo | Conteúdo |
| --- | --- |
| ID | `RF039` |
| Título | Impedir operações reservadas sem Empresa aprovada |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve impedir que o Recrutador publique vagas ou aceda a candidatos enquanto a Empresa que representa não estiver no estado aprovada, indicando o estado atual da Empresa. |
| Funcionalidade de origem | F001 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada na área de gestão web. |
| Critério de aceitação | Com a Empresa no estado pendente, recusada ou suspensa, a publicação de vaga e o acesso a candidatos são rejeitados e é apresentado o estado da Empresa. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F001); Declaração de Âmbito v02 (F001) |

#### RF040 — Alterar a própria palavra-passe

| Campo | Conteúdo |
| --- | --- |
| ID | `RF040` |
| Título | Alterar a própria palavra-passe |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador altere a sua palavra-passe indicando a palavra-passe atual e a nova palavra-passe, rejeitando a alteração, com indicação do motivo, quando a palavra-passe atual estiver errada ou a nova palavra-passe não tiver pelo menos 8 caracteres, com pelo menos uma letra e um algarismo. |
| Funcionalidade de origem | F001 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada na área de gestão web. |
| Critério de aceitação | Com a palavra-passe atual correta e a nova palavra-passe «katch2026», o início de sessão seguinte só é aceite com a nova palavra-passe; com a palavra-passe atual errada, ou com a nova palavra-passe «katch», a alteração é rejeitada. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F001); Declaração de Âmbito v02 (F001); parâmetros P05 da secção 3 |

#### RF041 — Registar a Empresa

| Campo | Conteúdo |
| --- | --- |
| ID | `RF041` |
| Título | Registar a Empresa |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador registe a Empresa que representa indicando designação social, número de identificação fiscal, setor de atividade entre tecnologia, saúde, comércio, hotelaria e restauração, construção, indústria, educação, finanças, logística, serviços e outro, morada, localidade da lista pré-definida de localidades, endereço de correio eletrónico, contacto telefónico e nome do responsável da Empresa. |
| Funcionalidade de origem | F003 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada na área de gestão web. |
| Critério de aceitação | Um registo com os oito campos preenchidos e conformes com P04 é aceite. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F003); Declaração de Âmbito v02 (F003); parâmetros P04, P15 da secção 3 |

#### RF042 — Impedir registo de Empresa sem validação automática

| Campo | Conteúdo |
| --- | --- |
| ID | `RF042` |
| Título | Impedir registo de Empresa sem validação automática |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve impedir que o Recrutador conclua o registo da Empresa sem cumprir a validação automática, por campo obrigatório vazio, endereço de correio eletrónico fora do formato local@domínio, contacto telefónico ou número de identificação fiscal sem 9 algarismos, ou número de identificação fiscal já registado, indicando ao Recrutador cada campo em causa e o motivo. |
| Funcionalidade de origem | F003 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada na área de gestão web. |
| Critério de aceitação | Um registo com número de identificação fiscal já existente ou com 8 algarismos é rejeitado e são apresentados o campo e o motivo. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F003); Declaração de Âmbito v02 (F003); parâmetros P04 da secção 3 |

#### RF043 — Colocar a Empresa registada em estado pendente

| Campo | Conteúdo |
| --- | --- |
| ID | `RF043` |
| Título | Colocar a Empresa registada em estado pendente |
| Tipo | Funcional |
| Ator | Não aplicável |
| Descrição | O sistema deve atribuir o estado pendente a cada registo de Empresa que cumpra a validação automática, mantendo esse estado até à decisão do Administrador. |
| Funcionalidade de origem | F003 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O registo da Empresa cumpriu a validação automática. |
| Critério de aceitação | Após um registo válido, a Empresa tem o estado pendente até à aprovação ou à recusa do registo pelo Administrador. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F003); Declaração de Âmbito v02 (F003) |

#### RF044 — Consultar o estado do pedido de registo

| Campo | Conteúdo |
| --- | --- |
| ID | `RF044` |
| Título | Consultar o estado do pedido de registo |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador consulte o estado do pedido de registo da Empresa, entre pendente, aprovada, recusada e suspensa, e, no estado recusada, o motivo indicado pelo Administrador. |
| Funcionalidade de origem | F003 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada na área de gestão web. |
| Critério de aceitação | Após uma recusa do registo com motivo, o Recrutador consulta o estado recusada e o texto do motivo registado. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F003); Declaração de Âmbito v02 (F003) |

#### RF045 — Corrigir e submeter novamente o pedido

| Campo | Conteúdo |
| --- | --- |
| ID | `RF045` |
| Título | Corrigir e submeter novamente o pedido |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador corrija os dados de um registo de Empresa recusado e o submeta novamente, aplicando a validação automática e atribuindo de novo o estado pendente. |
| Funcionalidade de origem | F003 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada e a Empresa está no estado recusada. |
| Critério de aceitação | Após correção e nova submissão válida, a Empresa passa de recusada a pendente. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F003); Declaração de Âmbito v02 (F003) |

#### RF046 — Gravar a página de apresentação da Empresa

| Campo | Conteúdo |
| --- | --- |
| ID | `RF046` |
| Título | Gravar a página de apresentação da Empresa |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador grave na página de apresentação da Empresa a descrição da atividade, com um máximo de 1000 caracteres, e o endereço do sítio na Internet iniciado por "http://" ou "https://". |
| Funcionalidade de origem | F003 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada e a Empresa que representa está aprovada. |
| Critério de aceitação | A descrição e o endereço gravados são apresentados na página; um endereço sem "http://" ou "https://" é rejeitado. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F003); Declaração de Âmbito v02 (F003); parâmetros P08, P10 da secção 3 |

#### RF047 — Carregar o logótipo da Empresa

| Campo | Conteúdo |
| --- | --- |
| ID | `RF047` |
| Título | Carregar o logótipo da Empresa |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador carregue o logótipo da Empresa nos formatos JPEG ou PNG com dimensão máxima de 2 MB, rejeitando, com indicação do motivo, ficheiros noutro formato ou com dimensão superior. |
| Funcionalidade de origem | F003 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada e a Empresa que representa está aprovada. |
| Critério de aceitação | Um PNG de 1 MB é aceite e apresentado na página; um PNG de 3 MB é rejeitado com indicação do motivo. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F003); Declaração de Âmbito v02 (F003); parâmetros P01 da secção 3 |

#### RF048 — Carregar fotografias da galeria da Empresa

| Campo | Conteúdo |
| --- | --- |
| ID | `RF048` |
| Título | Carregar fotografias da galeria da Empresa |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador carregue até 6 fotografias do local de trabalho na galeria da Empresa, nos formatos JPEG ou PNG com dimensão máxima de 5 MB cada, rejeitando, com indicação do motivo, a sétima fotografia e ficheiros não conformes. |
| Funcionalidade de origem | F003 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada e a Empresa que representa está aprovada. |
| Critério de aceitação | Com 6 fotografias na galeria, uma sétima é rejeitada; um ficheiro GIF é rejeitado. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F003); Declaração de Âmbito v02 (F003); parâmetros P01, P13 da secção 3 |

#### RF049 — Alterar os dados de registo da Empresa aprovada

| Campo | Conteúdo |
| --- | --- |
| ID | `RF049` |
| Título | Alterar os dados de registo da Empresa aprovada |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador altere os dados de registo da Empresa aprovada, com exceção do número de identificação fiscal, aplicando a validação automática e mantendo a Empresa no estado aprovada, e rejeitando, com indicação do motivo, qualquer alteração do número de identificação fiscal. |
| Funcionalidade de origem | F003 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada e a Empresa que representa está aprovada. |
| Critério de aceitação | Com a Empresa aprovada, a alteração do contacto telefónico para outro número com 9 algarismos é gravada e a Empresa mantém o estado aprovada; a alteração do número de identificação fiscal é rejeitada e o valor anterior mantém-se. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F003); Declaração de Âmbito v02 (F003); parâmetros P04, P15 da secção 3 |

#### RF050 — Criar uma vaga

| Campo | Conteúdo |
| --- | --- |
| ID | `RF050` |
| Título | Criar uma vaga |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador crie uma vaga da sua Empresa, no estado não publicada, com os campos obrigatórios função, descrição da oportunidade, intervalo salarial, localidade da lista pré-definida de localidades, proposta por omissão igual à da Empresa, tipo de contrato entre sem termo, a termo, estágio e prestação de serviços, regime de trabalho entre presencial, híbrido e remoto, e competências pretendidas da lista pré-definida de competências, e com os campos opcionais benefícios da lista pré-definida de benefícios, indicação de urgência e data-limite de publicação, rejeitando, com indicação do motivo, a criação com um campo obrigatório vazio. |
| Funcionalidade de origem | F005 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada e a Empresa que representa está aprovada. |
| Critério de aceitação | Uma vaga com os sete campos obrigatórios preenchidos é criada no estado não publicada e o campo localidade surge preenchido com a localidade da Empresa; uma vaga sem competências pretendidas é rejeitada com indicação do motivo. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F005); Declaração de Âmbito v02 (F005); parâmetros P09, P16 da secção 3 |

#### RF051 — Validar o intervalo salarial da vaga

| Campo | Conteúdo |
| --- | --- |
| ID | `RF051` |
| Título | Validar o intervalo salarial da vaga |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve impedir que o Recrutador grave uma vaga cujo valor mínimo do intervalo salarial seja superior ao valor máximo, ou em que algum dos valores não seja um número positivo em euros, indicando o motivo. |
| Funcionalidade de origem | F005 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada e a Empresa que representa está aprovada. |
| Critério de aceitação | Um intervalo de 1500 a 1200 euros é rejeitado com indicação do motivo; um intervalo de 1200 a 1500 euros é aceite. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F005); Declaração de Âmbito v02 (F005) |

#### RF052 — Carregar fotografias da vaga

| Campo | Conteúdo |
| --- | --- |
| ID | `RF052` |
| Título | Carregar fotografias da vaga |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador carregue até 5 fotografias por vaga, nos formatos JPEG ou PNG com dimensão máxima de 5 MB cada, rejeitando, com indicação do motivo, a sexta fotografia e ficheiros não conformes. |
| Funcionalidade de origem | F005 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada e a Empresa que representa está aprovada. |
| Critério de aceitação | Com 5 fotografias na vaga, uma sexta é rejeitada; um JPEG de 6 MB é rejeitado. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F005); Declaração de Âmbito v02 (F005); parâmetros P01, P13 da secção 3 |

#### RF053 — Publicar uma vaga

| Campo | Conteúdo |
| --- | --- |
| ID | `RF053` |
| Título | Publicar uma vaga |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador publique uma vaga da sua Empresa no estado não publicada, passando a vaga ao estado publicada. |
| Funcionalidade de origem | F005 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada e a Empresa que representa está aprovada. |
| Critério de aceitação | Após a publicação, a vaga tem o estado publicada e é apresentada a um Candidato cujas preferências de procura a admitam; antes da publicação, não é apresentada. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F005); Declaração de Âmbito v02 (F005) |

#### RF054 — Alterar uma vaga

| Campo | Conteúdo |
| --- | --- |
| ID | `RF054` |
| Título | Alterar uma vaga |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador altere os dados de uma vaga da sua Empresa que não esteja encerrada, aplicando as mesmas validações da criação. |
| Funcionalidade de origem | F005 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada e a Empresa que representa está aprovada. |
| Critério de aceitação | A alteração do intervalo salarial de uma vaga publicada é gravada e apresentada aos Candidatos; a alteração de uma vaga encerrada é rejeitada. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F005); Declaração de Âmbito v02 (F005) |

#### RF055 — Suspender uma vaga

| Campo | Conteúdo |
| --- | --- |
| ID | `RF055` |
| Título | Suspender uma vaga |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador suspenda uma vaga publicada da sua Empresa, passando a vaga ao estado suspensa sem eliminar os interesses registados nessa vaga. |
| Funcionalidade de origem | F005 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada e a Empresa que representa está aprovada. |
| Critério de aceitação | Após a suspensão, a vaga tem o estado suspensa, não é apresentada aos Candidatos e os interesses registados continuam consultáveis pelo Recrutador. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F005); Declaração de Âmbito v02 (F005) |

#### RF056 — Encerrar uma vaga

| Campo | Conteúdo |
| --- | --- |
| ID | `RF056` |
| Título | Encerrar uma vaga |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador encerre uma vaga da sua Empresa, passando a vaga ao estado encerrada sem eliminar os interesses e os matches associados. |
| Funcionalidade de origem | F005 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada e a Empresa que representa está aprovada. |
| Critério de aceitação | Após o encerramento, a vaga tem o estado encerrada, não é apresentada aos Candidatos e os matches dessa vaga continuam registados. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F005); Declaração de Âmbito v02 (F005) |

#### RF057 — Encerrar automaticamente a vaga na data-limite

| Campo | Conteúdo |
| --- | --- |
| ID | `RF057` |
| Título | Encerrar automaticamente a vaga na data-limite |
| Tipo | Funcional |
| Ator | Não aplicável |
| Descrição | O sistema deve passar ao estado encerrada cada vaga publicada no instante em que é atingida a data-limite de publicação definida pelo Recrutador, sem renovação automática da vaga. |
| Funcionalidade de origem | F005 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | A vaga está publicada e tem data-limite de publicação definida. |
| Critério de aceitação | Atingida a data-limite, a vaga passa ao estado encerrada sem ação do Recrutador e deixa de ser apresentada. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F005); Declaração de Âmbito v02 (F005) |

#### RF058 — Impedir eliminação de vaga com interesses

| Campo | Conteúdo |
| --- | --- |
| ID | `RF058` |
| Título | Impedir eliminação de vaga com interesses |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve impedir que o Recrutador elimine uma vaga com, pelo menos, um interesse registado, indicando que essa vaga apenas pode ser encerrada. |
| Funcionalidade de origem | F005 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada e a Empresa que representa está aprovada. |
| Critério de aceitação | A eliminação de uma vaga com um interesse registado é rejeitada e é indicado que a vaga apenas pode ser encerrada. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F005); Declaração de Âmbito v02 (F005) |

#### RF059 — Restringir a gestão às vagas da própria Empresa

| Campo | Conteúdo |
| --- | --- |
| ID | `RF059` |
| Título | Restringir a gestão às vagas da própria Empresa |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve impedir que o Recrutador publique, altere, suspenda, volte a publicar, encerre ou elimine vagas de uma Empresa que não seja a que representa. |
| Funcionalidade de origem | F005 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada e a Empresa que representa está aprovada. |
| Critério de aceitação | Uma tentativa de alterar uma vaga de outra Empresa é rejeitada e a vaga mantém-se inalterada. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F005); Declaração de Âmbito v02 (F005) |

#### RF060 — Consultar candidatos em espera por vaga

| Campo | Conteúdo |
| --- | --- |
| ID | `RF060` |
| Título | Consultar candidatos em espera por vaga |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador consulte, por vaga da sua Empresa, a lista dos Candidatos que manifestaram interesse e aguardam resposta, com a data do interesse, sem ações de aceitação ou recusa nessa lista. |
| Funcionalidade de origem | F007 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada e a Empresa que representa está aprovada. |
| Critério de aceitação | A lista apresenta apenas Candidatos com interesse na vaga e sem decisão, com a data do interesse, e não contém ações de decisão. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F007); Declaração de Âmbito v02 (F007) |

#### RF061 — Abrir o perfil completo do Candidato

| Campo | Conteúdo |
| --- | --- |
| ID | `RF061` |
| Título | Abrir o perfil completo do Candidato |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador abra o perfil completo de um Candidato em espera numa vaga da sua Empresa, com fotografia, dados profissionais, hiperligações, competências e curriculum vitae, quando exista. |
| Funcionalidade de origem | F007 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada e a Empresa que representa está aprovada. |
| Critério de aceitação | O perfil apresenta os dados enumerados que estejam registados; o perfil de um Candidato sem interesse nas vagas da Empresa não pode ser aberto. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F007); Declaração de Âmbito v02 (F007) |

#### RF062 — Destacar competências coincidentes

| Campo | Conteúdo |
| --- | --- |
| ID | `RF062` |
| Título | Destacar competências coincidentes |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador identifique em destaque, no perfil completo do Candidato aberto para uma vaga, as competências do Candidato coincidentes com as competências pretendidas nessa vaga. |
| Funcionalidade de origem | F007 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada e a Empresa que representa está aprovada. |
| Critério de aceitação | Com a vaga a pretender Java e SQL e o Candidato com SQL e Python, apenas SQL surge destacada. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F007); Declaração de Âmbito v02 (F007) |

#### RF063 — Aceitar um Candidato após abrir o perfil

| Campo | Conteúdo |
| --- | --- |
| ID | `RF063` |
| Título | Aceitar um Candidato após abrir o perfil |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador aceite um Candidato em espera numa vaga apenas depois de estar registada a abertura do perfil completo desse Candidato para essa vaga, rejeitando qualquer aceitação sem esse registo. |
| Funcionalidade de origem | F007 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada e abriu o perfil do Candidato para a vaga. |
| Critério de aceitação | Uma aceitação após a abertura do perfil é registada; um pedido de aceitação sem abertura prévia registada é rejeitado. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F007); Declaração de Âmbito v02 (F007) |

#### RF064 — Recusar um Candidato após abrir o perfil

| Campo | Conteúdo |
| --- | --- |
| ID | `RF064` |
| Título | Recusar um Candidato após abrir o perfil |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador recuse um Candidato em espera numa vaga apenas depois de estar registada a abertura do perfil completo desse Candidato para essa vaga, deixando o Candidato de ser apresentado para essa vaga sem efeito nas restantes vagas da Empresa. |
| Funcionalidade de origem | F007 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada e abriu o perfil do Candidato para a vaga. |
| Critério de aceitação | Após a recusa, o Candidato não surge na lista dessa vaga e continua na lista de outra vaga da mesma Empresa em que manifestou interesse. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F007); Declaração de Âmbito v02 (F007) |

#### RF065 — Registar a decisão sobre o Candidato

| Campo | Conteúdo |
| --- | --- |
| ID | `RF065` |
| Título | Registar a decisão sobre o Candidato |
| Tipo | Funcional |
| Ator | Não aplicável |
| Descrição | O sistema deve registar cada aceitação ou recusa de um Candidato numa vaga com a identificação do Recrutador que decidiu e a data e hora da decisão. |
| Funcionalidade de origem | F007 |
| Componentes abrangidos | Backend |
| Pré-condições | Existe interesse do Candidato na vaga. |
| Critério de aceitação | Após cada aceitação e cada recusa, existe um registo com o Recrutador que decidiu e a data e hora da decisão, verificável por inspeção dos dados de teste. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F007); Declaração de Âmbito v02 (F007) |

#### RF066 — Confirmar o match após aceitação

| Campo | Conteúdo |
| --- | --- |
| ID | `RF066` |
| Título | Confirmar o match após aceitação |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador confirme o match com um Candidato exclusivamente através da aceitação desse Candidato numa vaga em que o Candidato manifestou interesse, sem que nenhuma outra ação confirme o match. |
| Funcionalidade de origem | F008 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | Existe interesse do Candidato na vaga. |
| Critério de aceitação | Após a aceitação, existe um match; sem interesse do Candidato ou sem aceitação, não existe match. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F008); Declaração de Âmbito v02 (F008) |

#### RF067 — Consultar os matches da Empresa

| Campo | Conteúdo |
| --- | --- |
| ID | `RF067` |
| Título | Consultar os matches da Empresa |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador consulte os matches da sua Empresa com a identificação da vaga e do Candidato e o endereço de correio eletrónico e contacto telefónico do Candidato, incluindo matches de vagas encerradas. |
| Funcionalidade de origem | F008 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada e a Empresa que representa está aprovada. |
| Critério de aceitação | A lista apresenta os matches com vaga, Candidato e contactos; um match de vaga encerrada continua na lista. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F008); Declaração de Âmbito v02 (F008) |

#### RF068 — Criar automaticamente a conversa do match

| Campo | Conteúdo |
| --- | --- |
| ID | `RF068` |
| Título | Criar automaticamente a conversa do match |
| Tipo | Funcional |
| Ator | Não aplicável |
| Descrição | O sistema deve criar uma única conversa no instante em que um match é confirmado, associada a esse match e à vaga de origem. |
| Funcionalidade de origem | F008, F010 |
| Componentes abrangidos | Backend, Frontend web, Frontend mobile |
| Pré-condições | Um match foi confirmado. |
| Critério de aceitação | Após a confirmação de um match, existe exatamente uma conversa associada ao match e à vaga. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F008, F010); Declaração de Âmbito v02 (F008, F010) |

#### RF069 — Enviar mensagem ao Candidato

| Campo | Conteúdo |
| --- | --- |
| ID | `RF069` |
| Título | Enviar mensagem ao Candidato |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador responsável pela vaga envie ao Candidato uma mensagem de texto com 1 a 1000 caracteres, na conversa associada ao match, rejeitando o envio quando a mensagem estiver vazia, exceder 1000 caracteres ou a conversa estiver no modo apenas de consulta. |
| Funcionalidade de origem | F010 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada e existe um match com o Candidato numa vaga da sua Empresa. |
| Critério de aceitação | Uma mensagem com 20 caracteres é entregue; uma mensagem vazia, com 1001 caracteres ou numa conversa no modo apenas de consulta é rejeitada. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F010); Declaração de Âmbito v02 (F010); parâmetros P03 da secção 3 |

#### RF070 — Impedir comunicação sem match

| Campo | Conteúdo |
| --- | --- |
| ID | `RF070` |
| Título | Impedir comunicação sem match |
| Tipo | Funcional |
| Ator | Não aplicável |
| Descrição | O sistema deve impedir o envio de mensagens entre um Candidato e um Recrutador quando não exista match entre o Candidato e a Empresa associada à vaga. |
| Funcionalidade de origem | F010 |
| Componentes abrangidos | Backend, Frontend web, Frontend mobile |
| Pré-condições | Não aplicável |
| Critério de aceitação | Uma tentativa de envio de mensagem sem match é rejeitada. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F010); Declaração de Âmbito v02 (F010) |

#### RF071 — Consultar a lista de conversas

| Campo | Conteúdo |
| --- | --- |
| ID | `RF071` |
| Título | Consultar a lista de conversas |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador consulte a lista das conversas dos matches das vagas da sua Empresa, com o número de mensagens por ler em cada conversa. |
| Funcionalidade de origem | F010 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada e a Empresa que representa está aprovada. |
| Critério de aceitação | Com 2 mensagens recebidas e não abertas numa conversa, essa conversa apresenta o número 2. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F010); Declaração de Âmbito v02 (F010) |

#### RF072 — Consultar o histórico de uma conversa

| Campo | Conteúdo |
| --- | --- |
| ID | `RF072` |
| Título | Consultar o histórico de uma conversa |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador consulte o histórico de uma conversa, com o texto, a data e hora de envio de cada mensagem e a indicação de leitura pelo destinatário. |
| Funcionalidade de origem | F010 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada e existe um match com o Candidato numa vaga da sua Empresa. |
| Critério de aceitação | O histórico apresenta todas as mensagens trocadas por ordem de envio, com data, hora e indicação de leitura. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F010); Declaração de Âmbito v02 (F010) |

#### RF073 — Receber mensagens sem atualização manual

| Campo | Conteúdo |
| --- | --- |
| ID | `RF073` |
| Título | Receber mensagens sem atualização manual |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador receba na conversa aberta cada nova mensagem do Candidato sem atualização manual da página, no máximo 5 segundos após o envio, enquanto a área de gestão web estiver aberta com sessão iniciada. |
| Funcionalidade de origem | F010 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada e existe um match com o Candidato numa vaga da sua Empresa. |
| Critério de aceitação | Com a conversa aberta, uma mensagem enviada pelo Candidato surge na conversa em 5 segundos ou menos, sem qualquer ação do Recrutador. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F010); Declaração de Âmbito v02 (F010); parâmetros P17 da secção 3 |

#### RF074 — Encerrar uma conversa

| Campo | Conteúdo |
| --- | --- |
| ID | `RF074` |
| Título | Encerrar uma conversa |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador encerre uma conversa, passando essa conversa ao modo apenas de consulta para o Recrutador e para o Candidato. |
| Funcionalidade de origem | F010 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada e existe um match com o Candidato numa vaga da sua Empresa. |
| Critério de aceitação | Após o encerramento, o histórico continua visível para as duas partes e qualquer envio é rejeitado. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F010); Declaração de Âmbito v02 (F010) |

#### RF075 — Passar conversa a consulta por bloqueio ou suspensão

| Campo | Conteúdo |
| --- | --- |
| ID | `RF075` |
| Título | Passar conversa a consulta por bloqueio ou suspensão |
| Tipo | Funcional |
| Ator | Não aplicável |
| Descrição | O sistema deve passar ao modo apenas de consulta cada conversa em que a conta de uma das partes passe ao estado bloqueada ou suspensa, ou em que a Empresa passe ao estado suspensa, no instante dessa mudança de estado, mantendo a conversa nesse modo após qualquer reativação. |
| Funcionalidade de origem | F010 |
| Componentes abrangidos | Backend, Frontend web, Frontend mobile |
| Pré-condições | Existe uma conversa que não está no modo apenas de consulta. |
| Critério de aceitação | Após a suspensão da conta de um Candidato ou da Empresa, as conversas afetadas permitem a consulta do histórico e rejeitam o envio de mensagens, também depois da reativação. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F010); Declaração de Âmbito v02 (F010) |

#### RF076 — Notificar o Recrutador de novo interesse

| Campo | Conteúdo |
| --- | --- |
| ID | `RF076` |
| Título | Notificar o Recrutador de novo interesse |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador receba na área de notificações uma notificação que identifica a vaga, no instante em que um Candidato manifesta interesse numa vaga da sua Empresa. |
| Funcionalidade de origem | F011 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | A vaga está publicada. |
| Critério de aceitação | Após um interesse, surge na área de notificações do Recrutador uma notificação que identifica a vaga. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F011); Declaração de Âmbito v02 (F011) |

#### RF077 — Notificar o Recrutador de novo match

| Campo | Conteúdo |
| --- | --- |
| ID | `RF077` |
| Título | Notificar o Recrutador de novo match |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador receba na área de notificações uma notificação que identifica a vaga e o Candidato, no instante em que é confirmado um match numa vaga da sua Empresa. |
| Funcionalidade de origem | F011 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | Existe interesse do Candidato na vaga. |
| Critério de aceitação | Após a aceitação, surge na área de notificações do Recrutador uma notificação que identifica a vaga e o Candidato. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F011); Declaração de Âmbito v02 (F011) |

#### RF078 — Notificar o Recrutador de nova mensagem

| Campo | Conteúdo |
| --- | --- |
| ID | `RF078` |
| Título | Notificar o Recrutador de nova mensagem |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador receba na área de notificações uma notificação que identifica a conversa, no instante em que um Candidato lhe envia uma nova mensagem. |
| Funcionalidade de origem | F011 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada e existe um match com o Candidato numa vaga da sua Empresa. |
| Critério de aceitação | Após o envio de uma mensagem pelo Candidato, surge uma notificação que identifica a conversa. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F011); Declaração de Âmbito v02 (F011) |

#### RF079 — Notificar o Recrutador da decisão sobre a Empresa

| Campo | Conteúdo |
| --- | --- |
| ID | `RF079` |
| Título | Notificar o Recrutador da decisão sobre a Empresa |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador receba na área de notificações uma notificação com a decisão e, na recusa do registo, o motivo, no instante em que o Administrador aprova ou recusa o registo da Empresa. |
| Funcionalidade de origem | F011 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | A Empresa está no estado pendente. |
| Critério de aceitação | Após uma recusa do registo, surge na área de notificações do Recrutador uma notificação com a decisão e o motivo registado pelo Administrador. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F011); Declaração de Âmbito v02 (F011) |

#### RF080 — Consultar a área de notificações

| Campo | Conteúdo |
| --- | --- |
| ID | `RF080` |
| Título | Consultar a área de notificações |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador consulte a área de notificações com as notificações recebidas e o número de notificações por ler. |
| Funcionalidade de origem | F011 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada na área de gestão web. |
| Critério de aceitação | Com 3 notificações não lidas, a área apresenta as notificações e o número 3. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F011); Declaração de Âmbito v02 (F011) |

#### RF081 — Marcar notificação como lida

| Campo | Conteúdo |
| --- | --- |
| ID | `RF081` |
| Título | Marcar notificação como lida |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador marque uma notificação como lida, reduzindo em uma unidade o número de notificações por ler. |
| Funcionalidade de origem | F011 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada na área de gestão web. |
| Critério de aceitação | Após marcar uma de 3 notificações por ler, o número apresentado é 2. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F011); Declaração de Âmbito v02 (F011) |

#### RF082 — Aceder ao elemento associado à notificação

| Campo | Conteúdo |
| --- | --- |
| ID | `RF082` |
| Título | Aceder ao elemento associado à notificação |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador aceda, a partir de uma notificação, ao elemento a que a notificação se refere: match, conversa, vaga ou pedido de registo da Empresa. |
| Funcionalidade de origem | F011 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada na área de gestão web. |
| Critério de aceitação | A seleção de uma notificação de novo interesse abre a vaga correspondente com a lista de candidatos em espera. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F011); Declaração de Âmbito v02 (F011) |

#### RF106 — Terminar sessão na área de gestão web

| Campo | Conteúdo |
| --- | --- |
| ID | `RF106` |
| Título | Terminar sessão na área de gestão web |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador termine a sessão na área de gestão web, exigindo novo início de sessão para qualquer operação seguinte. |
| Funcionalidade de origem | F001 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada na área de gestão web. |
| Critério de aceitação | Após terminar a sessão, a abertura da lista de vagas da Empresa exige novo início de sessão. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F001); Declaração de Âmbito v02 (F001) |

#### RF109 — Marcar como lidas as mensagens ao abrir a conversa

| Campo | Conteúdo |
| --- | --- |
| ID | `RF109` |
| Título | Marcar como lidas as mensagens ao abrir a conversa |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador marque como lidas todas as mensagens do Candidato contidas numa conversa, através da abertura dessa conversa. |
| Funcionalidade de origem | F010 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada e existe um match com o Candidato numa vaga da sua Empresa. |
| Critério de aceitação | Antes de o Recrutador abrir a conversa, a mensagem surge ao Candidato como não lida; depois da abertura, surge como lida. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F010); Declaração de Âmbito v02 (F010) |

#### RF111 — Receber notificações sem atualização manual

| Campo | Conteúdo |
| --- | --- |
| ID | `RF111` |
| Título | Receber notificações sem atualização manual |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador receba cada nova notificação na área de notificações sem atualização manual da página, no máximo 5 segundos após o acontecimento que a origina, enquanto a área de gestão web estiver aberta com sessão iniciada. |
| Funcionalidade de origem | F011 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada na área de gestão web. |
| Critério de aceitação | Com a área de gestão web aberta, após um novo interesse numa vaga da Empresa, a notificação surge na área de notificações em 5 segundos ou menos, sem qualquer ação do Recrutador. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F011); Declaração de Âmbito v02 (F011); parâmetros P17 da secção 3 |

#### RF113 — Manter o interesse em espera até à decisão

| Campo | Conteúdo |
| --- | --- |
| ID | `RF113` |
| Título | Manter o interesse em espera até à decisão |
| Tipo | Funcional |
| Ator | Não aplicável |
| Descrição | O sistema deve manter o interesse do Candidato numa vaga no estado em espera de resposta desde o instante em que é registado até à aceitação ou recusa pelo Recrutador, sem prazo de expiração. |
| Funcionalidade de origem | F007, F008 |
| Componentes abrangidos | Backend |
| Pré-condições | Existe interesse do Candidato na vaga. |
| Critério de aceitação | Um interesse sem decisão continua em espera e presente na lista de candidatos em espera da vaga após 30 dias simulados; após a decisão, deixa de estar em espera. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F007, F008); Declaração de Âmbito v02 (F007, F008) |

#### RF114 — Eliminar uma vaga sem interesses registados

| Campo | Conteúdo |
| --- | --- |
| ID | `RF114` |
| Título | Eliminar uma vaga sem interesses registados |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador elimine uma vaga da sua Empresa que não tenha nenhum interesse registado. |
| Funcionalidade de origem | F005 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada, a Empresa que representa está aprovada e a vaga não tem interesses registados. |
| Critério de aceitação | Após a eliminação de uma vaga sem interesses, a vaga deixa de constar da lista de vagas da Empresa e da lista global de vagas publicadas. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F005, regra de eliminação de vagas); Declaração de Âmbito v02 (F005) |

#### RF115 — Voltar a publicar uma vaga suspensa

| Campo | Conteúdo |
| --- | --- |
| ID | `RF115` |
| Título | Voltar a publicar uma vaga suspensa |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve permitir que o Recrutador volte a publicar uma vaga suspensa da sua Empresa, passando a vaga ao estado publicada. |
| Funcionalidade de origem | F005 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada, a Empresa que representa está aprovada e a vaga está no estado suspensa. |
| Critério de aceitação | Após a nova publicação, a vaga tem o estado publicada e volta a ser apresentada a um Candidato cujas preferências de procura a admitam e que não a tenha recusado nem manifestado interesse nela. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F005); Declaração de Âmbito v02 (F005); decisão do grupo sobre a reversibilidade da suspensão, registada na versão v01 desta especificação |

#### RF117 — Impedir a alteração de uma decisão registada

| Campo | Conteúdo |
| --- | --- |
| ID | `RF117` |
| Título | Impedir a alteração de uma decisão registada |
| Tipo | Funcional |
| Ator | Recrutador |
| Descrição | O sistema deve impedir que o Recrutador altere uma aceitação ou uma recusa de Candidato já registada. |
| Funcionalidade de origem | F007 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Recrutador tem sessão iniciada e existe uma decisão registada sobre o Candidato na vaga. |
| Critério de aceitação | Uma tentativa de alterar uma aceitação para recusa, ou uma recusa para aceitação, é rejeitada e a decisão registada mantém-se. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F007); Declaração de Âmbito v02 (F007, exclusão da alteração de uma decisão já registada) |

#### RF118 — Registar a abertura do perfil por vaga

| Campo | Conteúdo |
| --- | --- |
| ID | `RF118` |
| Título | Registar a abertura do perfil por vaga |
| Tipo | Funcional |
| Ator | Não aplicável |
| Descrição | O sistema deve registar, para cada vaga, a abertura do perfil completo de cada Candidato em espera pelo Recrutador, com a data e hora da abertura. |
| Funcionalidade de origem | F007 |
| Componentes abrangidos | Backend |
| Pré-condições | Existe interesse do Candidato na vaga. |
| Critério de aceitação | Após a abertura do perfil de um Candidato a partir da lista de candidatos em espera de uma vaga, existe um registo dessa abertura para essa vaga, com data e hora, verificável por inspeção dos dados de teste. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F007); Declaração de Âmbito v02 (F007, registo da abertura do perfil pelo recrutador) |

### 4.3. Requisitos funcionais do Administrador (Issue `I022`)

Requisitos `RF083` a `RF104` e `RF107`, `RF112`, `RF116`. Os identificadores acrescentados depois da numeração inicial mantêm a ordem de criação (secção 3.1 dos critérios). Os requisitos sem ator direto incluídos nesta secção decorrem da mesma Issue.

#### RF083 — Iniciar sessão na área de gestão web

| Campo | Conteúdo |
| --- | --- |
| ID | `RF083` |
| Título | Iniciar sessão na área de gestão web |
| Tipo | Funcional |
| Ator | Administrador |
| Descrição | O sistema deve permitir que o Administrador inicie sessão na área de gestão web indicando o endereço de correio eletrónico e a palavra-passe, rejeitando o acesso, com indicação do motivo, quando as credenciais não coincidirem com as registadas. |
| Funcionalidade de origem | F001 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Administrador tem conta registada no sistema. |
| Critério de aceitação | Com credenciais corretas e conta ativa, a sessão é iniciada; com palavra-passe errada, a sessão não é iniciada e é apresentado o motivo. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F001); Declaração de Âmbito v02 (F001) |

#### RF084 — Alterar a própria palavra-passe

| Campo | Conteúdo |
| --- | --- |
| ID | `RF084` |
| Título | Alterar a própria palavra-passe |
| Tipo | Funcional |
| Ator | Administrador |
| Descrição | O sistema deve permitir que o Administrador altere a sua palavra-passe indicando a palavra-passe atual e a nova palavra-passe, rejeitando a alteração, com indicação do motivo, quando a palavra-passe atual estiver errada ou a nova palavra-passe não tiver pelo menos 8 caracteres, com pelo menos uma letra e um algarismo. |
| Funcionalidade de origem | F001 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Administrador tem sessão iniciada na área de gestão web. |
| Critério de aceitação | Com a palavra-passe atual correta e a nova palavra-passe «katch2026», o início de sessão seguinte só é aceite com a nova palavra-passe; com a palavra-passe atual errada, ou com a nova palavra-passe «katch», a alteração é rejeitada. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F001); Declaração de Âmbito v02 (F001); parâmetros P05 da secção 3 |

#### RF085 — Consultar as contas de utilizador

| Campo | Conteúdo |
| --- | --- |
| ID | `RF085` |
| Título | Consultar as contas de utilizador |
| Tipo | Funcional |
| Ator | Administrador |
| Descrição | O sistema deve permitir que o Administrador consulte a lista das contas de utilizador com o endereço de correio eletrónico, o tipo de conta, entre Candidato, Recrutador e Administrador, e o estado de cada conta, entre ativa, bloqueada e suspensa. |
| Funcionalidade de origem | F001 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Administrador tem sessão iniciada na área de gestão web. |
| Critério de aceitação | A lista apresenta todas as contas registadas com os três dados enumerados. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F001); Declaração de Âmbito v02 (F001) |

#### RF086 — Bloquear uma conta de utilizador

| Campo | Conteúdo |
| --- | --- |
| ID | `RF086` |
| Título | Bloquear uma conta de utilizador |
| Tipo | Funcional |
| Ator | Administrador |
| Descrição | O sistema deve permitir que o Administrador bloqueie uma conta de Candidato ou de Recrutador, passando a conta ao estado bloqueada. |
| Funcionalidade de origem | F001 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Administrador tem sessão iniciada na área de gestão web. |
| Critério de aceitação | Após o bloqueio, a conta tem o estado bloqueada e o início de sessão com essa conta é rejeitado com indicação do motivo. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F001); Declaração de Âmbito v02 (F001) |

#### RF087 — Reativar uma conta bloqueada

| Campo | Conteúdo |
| --- | --- |
| ID | `RF087` |
| Título | Reativar uma conta bloqueada |
| Tipo | Funcional |
| Ator | Administrador |
| Descrição | O sistema deve permitir que o Administrador reative uma conta bloqueada, passando a conta ao estado ativa. |
| Funcionalidade de origem | F001 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Administrador tem sessão iniciada e a conta está bloqueada. |
| Critério de aceitação | Após a reativação, a conta tem o estado ativa e o início de sessão com essa conta é aceite. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F001); Declaração de Âmbito v02 (F001) |

#### RF088 — Consultar a lista de Candidatos

| Campo | Conteúdo |
| --- | --- |
| ID | `RF088` |
| Título | Consultar a lista de Candidatos |
| Tipo | Funcional |
| Ator | Administrador |
| Descrição | O sistema deve permitir que o Administrador consulte a lista dos Candidatos registados com o nome, o endereço de correio eletrónico, a localidade, a data de registo e o estado de cada conta, entre ativa, bloqueada e suspensa. |
| Funcionalidade de origem | F002 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Administrador tem sessão iniciada na área de gestão web. |
| Critério de aceitação | A lista apresenta todos os Candidatos registados com os cinco dados enumerados. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F002); Declaração de Âmbito v02 (F002) |

#### RF089 — Suspender a conta de um Candidato

| Campo | Conteúdo |
| --- | --- |
| ID | `RF089` |
| Título | Suspender a conta de um Candidato |
| Tipo | Funcional |
| Ator | Administrador |
| Descrição | O sistema deve permitir que o Administrador suspenda a conta de um Candidato, passando a conta ao estado suspensa. |
| Funcionalidade de origem | F002 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Administrador tem sessão iniciada na área de gestão web. |
| Critério de aceitação | Após a suspensão, a conta tem o estado suspensa e o início de sessão do Candidato é rejeitado com indicação do motivo. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F002); Declaração de Âmbito v02 (F002) |

#### RF090 — Reativar a conta de um Candidato suspensa

| Campo | Conteúdo |
| --- | --- |
| ID | `RF090` |
| Título | Reativar a conta de um Candidato suspensa |
| Tipo | Funcional |
| Ator | Administrador |
| Descrição | O sistema deve permitir que o Administrador reative a conta suspensa de um Candidato, passando a conta ao estado ativa. |
| Funcionalidade de origem | F002 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Administrador tem sessão iniciada e a conta do Candidato está suspensa. |
| Critério de aceitação | Após a reativação, o Candidato consegue iniciar sessão e o estado apresentado é ativa. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F002); Declaração de Âmbito v02 (F002) |

#### RF091 — Consultar as Empresas pendentes de aprovação

| Campo | Conteúdo |
| --- | --- |
| ID | `RF091` |
| Título | Consultar as Empresas pendentes de aprovação |
| Tipo | Funcional |
| Ator | Administrador |
| Descrição | O sistema deve permitir que o Administrador consulte a lista das Empresas no estado pendente, com a designação social, o número de identificação fiscal e a data de submissão do pedido. |
| Funcionalidade de origem | F003, F009 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Administrador tem sessão iniciada na área de gestão web. |
| Critério de aceitação | A lista apresenta apenas as Empresas pendentes, com os três dados enumerados. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F003, F009); Declaração de Âmbito v02 (F003, F009) |

#### RF092 — Consultar os dados submetidos pela Empresa

| Campo | Conteúdo |
| --- | --- |
| ID | `RF092` |
| Título | Consultar os dados submetidos pela Empresa |
| Tipo | Funcional |
| Ator | Administrador |
| Descrição | O sistema deve permitir que o Administrador consulte todos os dados submetidos no registo de uma Empresa pendente: designação social, número de identificação fiscal, setor de atividade, morada, localidade, endereço de correio eletrónico, contacto telefónico e nome do responsável. |
| Funcionalidade de origem | F003 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Administrador tem sessão iniciada na área de gestão web. |
| Critério de aceitação | Os oito dados apresentados coincidem com os submetidos pelo Recrutador. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F003); Declaração de Âmbito v02 (F003) |

#### RF093 — Aprovar o registo de uma Empresa

| Campo | Conteúdo |
| --- | --- |
| ID | `RF093` |
| Título | Aprovar o registo de uma Empresa |
| Tipo | Funcional |
| Ator | Administrador |
| Descrição | O sistema deve permitir que o Administrador aprove o registo de uma Empresa pendente, passando a Empresa ao estado aprovada. |
| Funcionalidade de origem | F003 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Administrador tem sessão iniciada e a Empresa está no estado pendente. |
| Critério de aceitação | Após a aprovação, a Empresa tem o estado aprovada e o Recrutador consegue publicar vagas. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F003); Declaração de Âmbito v02 (F003) |

#### RF094 — Recusar o registo de uma Empresa com motivo

| Campo | Conteúdo |
| --- | --- |
| ID | `RF094` |
| Título | Recusar o registo de uma Empresa com motivo |
| Tipo | Funcional |
| Ator | Administrador |
| Descrição | O sistema deve permitir que o Administrador recuse o registo de uma Empresa pendente indicando um motivo com 10 a 500 caracteres, passando a Empresa ao estado recusada e rejeitando a decisão sem motivo conforme. |
| Funcionalidade de origem | F003 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Administrador tem sessão iniciada e a Empresa está no estado pendente. |
| Critério de aceitação | Uma recusa do registo com motivo de 30 caracteres passa a Empresa ao estado recusada; uma recusa do registo sem motivo ou com motivo de 5 caracteres é rejeitada. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F003); Declaração de Âmbito v02 (F003); parâmetros P12 da secção 3 |

#### RF095 — Suspender uma Empresa aprovada

| Campo | Conteúdo |
| --- | --- |
| ID | `RF095` |
| Título | Suspender uma Empresa aprovada |
| Tipo | Funcional |
| Ator | Administrador |
| Descrição | O sistema deve permitir que o Administrador suspenda uma Empresa aprovada, passando a Empresa ao estado suspensa. |
| Funcionalidade de origem | F003 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Administrador tem sessão iniciada e a Empresa está no estado aprovada. |
| Critério de aceitação | Após a suspensão, nenhuma vaga da Empresa é apresentada aos Candidatos e os matches existentes continuam registados. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F003); Declaração de Âmbito v02 (F003) |

#### RF096 — Consultar o painel de indicadores por período

| Campo | Conteúdo |
| --- | --- |
| ID | `RF096` |
| Título | Consultar o painel de indicadores por período |
| Tipo | Funcional |
| Ator | Administrador |
| Descrição | O sistema deve permitir que o Administrador consulte, para o período definido por uma data inicial e uma data final, os indicadores seguintes: número de Candidatos com conta no estado ativa na data final; número de Empresas no estado aprovada na data final; número de Empresas no estado pendente na data final; número de vagas publicadas pela primeira vez no período; número de interesses manifestados no período; número de matches confirmados no período; número de conversas cuja primeira mensagem foi enviada no período. |
| Funcionalidade de origem | F009 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Administrador tem sessão iniciada na área de gestão web. |
| Critério de aceitação | Com dados fictícios conhecidos, os sete valores apresentados para o período coincidem com os contados diretamente nesses dados. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F009); Declaração de Âmbito v02 (F009) |

#### RF097 — Consultar a lista global de Empresas

| Campo | Conteúdo |
| --- | --- |
| ID | `RF097` |
| Título | Consultar a lista global de Empresas |
| Tipo | Funcional |
| Ator | Administrador |
| Descrição | O sistema deve permitir que o Administrador consulte a lista de todas as Empresas registadas com a designação social, o estado, entre pendente, aprovada, recusada e suspensa, e o número de vagas no estado publicada de cada Empresa. |
| Funcionalidade de origem | F009 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Administrador tem sessão iniciada na área de gestão web. |
| Critério de aceitação | A lista apresenta todas as Empresas, em qualquer estado, com os três dados enumerados. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F009); Declaração de Âmbito v02 (F009) |

#### RF098 — Consultar a lista global de vagas publicadas

| Campo | Conteúdo |
| --- | --- |
| ID | `RF098` |
| Título | Consultar a lista global de vagas publicadas |
| Tipo | Funcional |
| Ator | Administrador |
| Descrição | O sistema deve permitir que o Administrador consulte a lista de todas as vagas no estado publicada com a função, a Empresa, a data de publicação e o número de interesses de cada vaga. |
| Funcionalidade de origem | F005, F009 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Administrador tem sessão iniciada na área de gestão web. |
| Critério de aceitação | A lista apresenta todas as vagas publicadas de todas as Empresas com os quatro dados enumerados. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F005, F009); Declaração de Âmbito v02 (F005, F009) |

#### RF099 — Acrescentar competência à lista pré-definida

| Campo | Conteúdo |
| --- | --- |
| ID | `RF099` |
| Título | Acrescentar competência à lista pré-definida |
| Tipo | Funcional |
| Ator | Administrador |
| Descrição | O sistema deve permitir que o Administrador acrescente uma competência à lista pré-definida de competências, rejeitando designações vazias ou já existentes na lista. |
| Funcionalidade de origem | F009 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Administrador tem sessão iniciada na área de gestão web. |
| Critério de aceitação | Uma competência nova passa a estar disponível para seleção pelo Candidato; uma designação repetida é rejeitada. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F009); Declaração de Âmbito v02 (F009) |

#### RF100 — Alterar a designação de uma competência

| Campo | Conteúdo |
| --- | --- |
| ID | `RF100` |
| Título | Alterar a designação de uma competência |
| Tipo | Funcional |
| Ator | Administrador |
| Descrição | O sistema deve permitir que o Administrador altere a designação de uma competência da lista pré-definida, rejeitando designações vazias ou já existentes na lista. |
| Funcionalidade de origem | F009 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Administrador tem sessão iniciada na área de gestão web. |
| Critério de aceitação | A designação alterada é apresentada nos perfis e vagas que usam essa competência; uma designação repetida é rejeitada. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F009); Declaração de Âmbito v02 (F009) |

#### RF101 — Acrescentar benefício à lista pré-definida

| Campo | Conteúdo |
| --- | --- |
| ID | `RF101` |
| Título | Acrescentar benefício à lista pré-definida |
| Tipo | Funcional |
| Ator | Administrador |
| Descrição | O sistema deve permitir que o Administrador acrescente um benefício à lista pré-definida de benefícios, rejeitando designações vazias ou já existentes na lista. |
| Funcionalidade de origem | F009 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Administrador tem sessão iniciada na área de gestão web. |
| Critério de aceitação | Um benefício novo passa a estar disponível para seleção pelo Recrutador; uma designação repetida é rejeitada. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F009); Declaração de Âmbito v02 (F009) |

#### RF102 — Alterar a designação de um benefício

| Campo | Conteúdo |
| --- | --- |
| ID | `RF102` |
| Título | Alterar a designação de um benefício |
| Tipo | Funcional |
| Ator | Administrador |
| Descrição | O sistema deve permitir que o Administrador altere a designação de um benefício da lista pré-definida, rejeitando designações vazias ou já existentes na lista. |
| Funcionalidade de origem | F009 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Administrador tem sessão iniciada na área de gestão web. |
| Critério de aceitação | A designação alterada é apresentada nas vagas que usam esse benefício; uma designação repetida é rejeitada. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F009); Declaração de Âmbito v02 (F009) |

#### RF103 — Impedir o acesso do Administrador às conversas

| Campo | Conteúdo |
| --- | --- |
| ID | `RF103` |
| Título | Impedir o acesso do Administrador às conversas |
| Tipo | Funcional |
| Ator | Administrador |
| Descrição | O sistema deve impedir que o Administrador consulte o conteúdo das conversas entre Candidatos e Recrutadores. |
| Funcionalidade de origem | F009, F010 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Administrador tem sessão iniciada na área de gestão web. |
| Critério de aceitação | Uma tentativa do Administrador de aceder ao histórico de uma conversa é rejeitada. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F009, F010); Declaração de Âmbito v02 (F009, F010) |

#### RF104 — Reservar a área de supervisão ao Administrador

| Campo | Conteúdo |
| --- | --- |
| ID | `RF104` |
| Título | Reservar a área de supervisão ao Administrador |
| Tipo | Funcional |
| Ator | Não aplicável |
| Descrição | O sistema deve impedir o acesso à área de supervisão a qualquer conta que não seja do tipo Administrador. |
| Funcionalidade de origem | F009 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | Não aplicável |
| Critério de aceitação | Uma tentativa de acesso à área de supervisão com uma conta do tipo Recrutador é rejeitada. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F009); Declaração de Âmbito v02 (F009) |

#### RF107 — Terminar sessão na área de gestão web

| Campo | Conteúdo |
| --- | --- |
| ID | `RF107` |
| Título | Terminar sessão na área de gestão web |
| Tipo | Funcional |
| Ator | Administrador |
| Descrição | O sistema deve permitir que o Administrador termine a sessão na área de gestão web, exigindo novo início de sessão para qualquer operação seguinte. |
| Funcionalidade de origem | F001 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Administrador tem sessão iniciada na área de gestão web. |
| Critério de aceitação | Após terminar a sessão, a abertura da área de supervisão exige novo início de sessão. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F001); Declaração de Âmbito v02 (F001) |

#### RF112 — Registar o autor das operações relevantes

| Campo | Conteúdo |
| --- | --- |
| ID | `RF112` |
| Título | Registar o autor das operações relevantes |
| Tipo | Funcional |
| Ator | Não aplicável |
| Descrição | O sistema deve registar, com a identificação do utilizador e a data e hora, as operações seguintes: submissão e alteração de registo de Empresa; criação, publicação, alteração, suspensão, nova publicação, encerramento e eliminação de vaga; interesse e recusa de vaga; bloqueio, reativação e suspensão de conta; aprovação, recusa, suspensão e reativação de Empresa; alteração das listas pré-definidas. |
| Funcionalidade de origem | F001 |
| Componentes abrangidos | Backend |
| Pré-condições | Não aplicável |
| Critério de aceitação | Após cada uma das operações enumeradas, existe um registo com o utilizador que a realizou e a data e hora, verificável por inspeção dos dados de teste. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F001); Declaração de Âmbito v02 (F001) |

#### RF116 — Reativar uma Empresa suspensa

| Campo | Conteúdo |
| --- | --- |
| ID | `RF116` |
| Título | Reativar uma Empresa suspensa |
| Tipo | Funcional |
| Ator | Administrador |
| Descrição | O sistema deve permitir que o Administrador reative uma Empresa suspensa, passando a Empresa ao estado aprovada. |
| Funcionalidade de origem | F003 |
| Componentes abrangidos | Backend, Frontend web |
| Pré-condições | O Administrador tem sessão iniciada e a Empresa está no estado suspensa. |
| Critério de aceitação | Após a reativação, a Empresa tem o estado aprovada e as suas vagas no estado publicada voltam a ser apresentadas aos Candidatos cujas preferências de procura as admitam. |
| Prioridade | Obrigatório |
| Origem | Proposta de Sistema v02 (F003); Declaração de Âmbito v02 (F003); decisão do grupo sobre a reversibilidade da suspensão, registada na versão v01 desta especificação |

---

## 5. Requisitos não funcionais (Issue `I023`)

Requisitos `RNF001` a `RNF018`. Os valores-limite e as condições de medição constam dos parâmetros `P18` a `P21` da secção 3.

#### RNF001 — Responder às operações do servidor em 2 segundos

| Campo | Conteúdo |
| --- | --- |
| ID | `RNF001` |
| Título | Responder às operações do servidor em 2 segundos |
| Tipo | Não funcional |
| Ator | Não aplicável |
| Descrição | O sistema deve responder a cada operação da interface do servidor, com exceção do carregamento de ficheiros, com um tempo de resposta não superior a 2 segundos, no ambiente de demonstração, com os dados de demonstração e um único utilizador. |
| Funcionalidade de origem | F002, F003, F004, F005, F006, F007, F008, F009, F010, F011 |
| Componentes abrangidos | Backend |
| Pré-condições | Os dados de demonstração estão carregados no ambiente de demonstração. |
| Critério de aceitação | Na execução completa da coleção de testes da interface do servidor no ambiente de demonstração, nenhuma operação, excluindo o carregamento de ficheiros, apresenta tempo de resposta superior a 2 segundos. |
| Prioridade | Obrigatório |
| Origem | Backlog de Projeto v02 (I023) (tempos de resposta da interface do servidor); parâmetros P18, P19 da secção 3 |

#### RNF002 — Apresentar o cartão de vaga seguinte em 2 segundos

| Campo | Conteúdo |
| --- | --- |
| ID | `RNF002` |
| Título | Apresentar o cartão de vaga seguinte em 2 segundos |
| Tipo | Não funcional |
| Ator | Candidato |
| Descrição | O sistema deve apresentar ao Candidato o cartão de vaga seguinte num tempo não superior a 2 segundos após o registo de uma recusa ou de um interesse sobre o cartão anterior, no ambiente de demonstração, com os dados de demonstração. |
| Funcionalidade de origem | F006 |
| Componentes abrangidos | Backend, Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada na aplicação móvel e existem, pelo menos, 6 vagas que cumprem as suas preferências de procura. |
| Critério de aceitação | Em 5 recusas ou interesses consecutivos, cada cartão de vaga seguinte é apresentado em 2 segundos ou menos após a ação anterior. |
| Prioridade | Obrigatório |
| Origem | Backlog de Projeto v02 (I023) (navegação na área de exploração de vagas); parâmetros P18, P19 da secção 3 |

#### RNF003 — Conservar palavras-passe sob a forma de resumo irreversível

| Campo | Conteúdo |
| --- | --- |
| ID | `RNF003` |
| Título | Conservar palavras-passe sob a forma de resumo irreversível |
| Tipo | Não funcional |
| Ator | Não aplicável |
| Descrição | O sistema deve conservar cada palavra-passe exclusivamente sob a forma de resumo criptográfico irreversível, calculado com um valor aleatório próprio de cada conta, com 0 palavras-passe legíveis ou recuperáveis nos dados persistidos. |
| Funcionalidade de origem | F001, F002 |
| Componentes abrangidos | Backend |
| Pré-condições | Não aplicável |
| Critério de aceitação | Por inspeção dos dados persistidos após o registo de duas contas com a palavra-passe «katch2026», a palavra-passe não surge legível e os dois valores conservados são diferentes. |
| Prioridade | Obrigatório |
| Origem | Backlog de Projeto v02 (I023) (hashing de palavras-passe); Proposta de Sistema v02 (F001) |

#### RNF004 — Exigir credencial de sessão válida nas operações reservadas

| Campo | Conteúdo |
| --- | --- |
| ID | `RNF004` |
| Título | Exigir credencial de sessão válida nas operações reservadas |
| Tipo | Não funcional |
| Ator | Não aplicável |
| Descrição | O sistema deve rejeitar 100% dos pedidos a operações reservadas que não apresentem uma credencial de sessão válida, incluindo os pedidos diretos. |
| Funcionalidade de origem | F001 |
| Componentes abrangidos | Backend |
| Pré-condições | Não aplicável |
| Critério de aceitação | Na coleção de testes da interface do servidor, para cada operação reservada, um pedido direto sem credencial de sessão e um pedido direto com uma credencial de sessão adulterada são rejeitados. |
| Prioridade | Obrigatório |
| Origem | Backlog de Projeto v02 (I023) (autenticação por credencial de sessão); Proposta de Sistema v02 (F001) |

#### RNF005 — Expirar a credencial de sessão ao fim de 8 horas

| Campo | Conteúdo |
| --- | --- |
| ID | `RNF005` |
| Título | Expirar a credencial de sessão ao fim de 8 horas |
| Tipo | Não funcional |
| Ator | Não aplicável |
| Descrição | O sistema deve rejeitar 100% dos pedidos que apresentem uma credencial de sessão emitida há mais de 8 horas, nos três tipos de conta. |
| Funcionalidade de origem | F001 |
| Componentes abrangidos | Backend |
| Pré-condições | Não aplicável |
| Critério de aceitação | Num teste automático com o relógio do sistema simulado, um pedido com uma credencial de sessão emitida há 8 horas e 1 minuto é rejeitado e o mesmo pedido com uma credencial de sessão emitida há 7 horas e 59 minutos é aceite. |
| Prioridade | Importante |
| Origem | Backlog de Projeto v02 (I023) (autenticação por credencial de sessão); parâmetros P20 da secção 3 |

#### RNF006 — Verificar no servidor a autorização de cada pedido

| Campo | Conteúdo |
| --- | --- |
| ID | `RNF006` |
| Título | Verificar no servidor a autorização de cada pedido |
| Tipo | Não funcional |
| Ator | Não aplicável |
| Descrição | O sistema deve rejeitar 100% dos pedidos que solicitem uma operação não autorizada ao tipo de conta ou ao titular da credencial de sessão apresentada, incluindo os pedidos diretos. |
| Funcionalidade de origem | F001, F005, F007, F009, F010 |
| Componentes abrangidos | Backend |
| Pré-condições | Não aplicável |
| Critério de aceitação | São rejeitados um pedido direto com credencial de sessão de Candidato para publicar uma vaga, um pedido direto com credencial de sessão de Recrutador para alterar uma vaga de outra Empresa e um pedido direto com credencial de sessão de Administrador para consultar o histórico de uma conversa. |
| Prioridade | Obrigatório |
| Origem | Backlog de Projeto v02 (I023) (controlo de acesso por perfil); Proposta de Sistema v02 (F001, F005, F009, F010) |

#### RNF007 — Restringir a obtenção do curriculum vitae

| Campo | Conteúdo |
| --- | --- |
| ID | `RNF007` |
| Título | Restringir a obtenção do curriculum vitae |
| Tipo | Não funcional |
| Ator | Não aplicável |
| Descrição | O sistema deve rejeitar 100% dos pedidos de obtenção do curriculum vitae de um Candidato que não provenham do Recrutador responsável por uma vaga em que esse Candidato esteja em espera, incluindo os pedidos diretos ao endereço do ficheiro. |
| Funcionalidade de origem | F004, F007 |
| Componentes abrangidos | Backend |
| Pré-condições | O Candidato enviou um curriculum vitae. |
| Critério de aceitação | O pedido do Recrutador responsável por uma vaga em que o Candidato está em espera é aceite; são rejeitados o pedido sem credencial de sessão, o pedido de outro Recrutador e o pedido do Administrador. |
| Prioridade | Obrigatório |
| Origem | Backlog de Projeto v02 (I023) (proteção do curriculum vitae); Proposta de Sistema v02 (F004); Declaração de Âmbito v02 (F004) |

#### RNF008 — Aplicar a validação automática no servidor

| Campo | Conteúdo |
| --- | --- |
| ID | `RNF008` |
| Título | Aplicar a validação automática no servidor |
| Tipo | Não funcional |
| Ator | Não aplicável |
| Descrição | O sistema deve aplicar no servidor 100% das regras da validação automática e dos limites fixados nos parâmetros P01 a P16, rejeitando os dados não conformes recebidos em pedidos diretos. |
| Funcionalidade de origem | F002, F003, F004, F005, F010 |
| Componentes abrangidos | Backend |
| Pré-condições | Não aplicável |
| Critério de aceitação | São rejeitados um pedido direto de registo de Candidato com contacto telefónico de 8 algarismos, um pedido direto de envio de mensagem com 1001 caracteres e um pedido direto de definição de distância máxima de 501 km. |
| Prioridade | Obrigatório |
| Origem | Backlog de Projeto v02 (I023); Proposta de Sistema v02 (F002, F003); parâmetros P01 a P16 da secção 3 |

#### RNF009 — Uniformizar o motivo de rejeição por credenciais

| Campo | Conteúdo |
| --- | --- |
| ID | `RNF009` |
| Título | Uniformizar o motivo de rejeição por credenciais |
| Tipo | Não funcional |
| Ator | Não aplicável |
| Descrição | O sistema deve devolver a mesma indicação de motivo em 100% das rejeições de início de sessão por credenciais que não coincidam com as registadas, sem distinguir endereço de correio eletrónico inexistente de palavra-passe errada. |
| Funcionalidade de origem | F001 |
| Componentes abrangidos | Backend |
| Pré-condições | Não aplicável |
| Critério de aceitação | A rejeição de um início de sessão com endereço de correio eletrónico não registado e a rejeição com palavra-passe errada de uma conta existente devolvem a mesma indicação de motivo. |
| Prioridade | Importante |
| Origem | Backlog de Projeto v02 (I023); RF001, RF038, RF083 |

#### RNF010 — Exigir uma interação por ação sobre o cartão de vaga

| Campo | Conteúdo |
| --- | --- |
| ID | `RNF010` |
| Título | Exigir uma interação por ação sobre o cartão de vaga |
| Tipo | Não funcional |
| Ator | Candidato |
| Descrição | O sistema deve exigir ao Candidato um número de interações não superior a 1 para executar, sobre o cartão de vaga apresentado na área de exploração de vagas, cada uma das ações seguintes: recusa, interesse e abertura do detalhe da vaga. |
| Funcionalidade de origem | F006 |
| Componentes abrangidos | Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada e a área de exploração de vagas apresenta um cartão de vaga. |
| Critério de aceitação | A partir de um cartão de vaga apresentado, a recusa, o interesse e a abertura do detalhe são, cada uma, concluídas com 1 interação. |
| Prioridade | Obrigatório |
| Origem | Backlog de Projeto v02 (I023); Proposta de Sistema v02 (secção 2 e F006); parâmetros P21 da secção 3 |

#### RNF011 — Aceder às áreas da aplicação móvel em 3 interações

| Campo | Conteúdo |
| --- | --- |
| ID | `RNF011` |
| Título | Aceder às áreas da aplicação móvel em 3 interações |
| Tipo | Não funcional |
| Ator | Candidato |
| Descrição | O sistema deve exigir ao Candidato um número de interações não superior a 3 para aceder, a partir da área de exploração de vagas, a cada uma das áreas seguintes: perfil profissional, preferências de procura, lista de matches, lista de conversas e área de notificações. |
| Funcionalidade de origem | F004, F006, F008, F010, F011 |
| Componentes abrangidos | Frontend mobile |
| Pré-condições | O Candidato tem sessão iniciada na aplicação móvel. |
| Critério de aceitação | A partir da área de exploração de vagas, cada uma das cinco áreas enumeradas é aberta com 3 interações ou menos. |
| Prioridade | Importante |
| Origem | Backlog de Projeto v02 (I023); parâmetros P21 da secção 3 |

#### RNF012 — Apresentar a área de gestão web sem deslocamento horizontal

| Campo | Conteúdo |
| --- | --- |
| ID | `RNF012` |
| Título | Apresentar a área de gestão web sem deslocamento horizontal |
| Tipo | Não funcional |
| Ator | Não aplicável |
| Descrição | O sistema deve apresentar cada área da área de gestão web sem deslocamento horizontal em larguras de janela entre 1280 e 1920 píxeis, inclusive. |
| Funcionalidade de origem | F001, F003, F005, F007, F008, F009, F010, F011 |
| Componentes abrangidos | Frontend web |
| Pré-condições | Não aplicável |
| Critério de aceitação | Com a janela a 1280 e a 1920 píxeis de largura, nenhuma área da área de gestão web apresenta barra de deslocamento horizontal. |
| Prioridade | Importante |
| Origem | Backlog de Projeto v02 (I023); parâmetros P21 da secção 3 |

#### RNF013 — Conservar os dados confirmados após reinício do servidor

| Campo | Conteúdo |
| --- | --- |
| ID | `RNF013` |
| Título | Conservar os dados confirmados após reinício do servidor |
| Tipo | Não funcional |
| Ator | Não aplicável |
| Descrição | O sistema deve conservar 100% dos dados cuja gravação foi confirmada ao utilizador após o reinício do servidor e da base de dados no ambiente de demonstração. |
| Funcionalidade de origem | F002, F003, F004, F005, F006, F007, F008, F010, F011 |
| Componentes abrangidos | Backend |
| Pré-condições | Não aplicável |
| Critério de aceitação | Depois de gravar um registo de Candidato, um interesse, uma decisão do Recrutador e uma mensagem, e de reiniciar o servidor e a base de dados, os quatro dados são apresentados sem alteração. |
| Prioridade | Obrigatório |
| Origem | Backlog de Projeto v02 (I023) (disponibilidade); Proposta de Sistema v02 (F008, F010) |

#### RNF014 — Indicar falha de ligação na aplicação móvel em 10 segundos

| Campo | Conteúdo |
| --- | --- |
| ID | `RNF014` |
| Título | Indicar falha de ligação na aplicação móvel em 10 segundos |
| Tipo | Não funcional |
| Ator | Candidato |
| Descrição | O sistema deve apresentar ao Candidato uma indicação de falha de ligação num tempo não superior a 10 segundos após o envio de um pedido que não obtenha resposta do servidor. |
| Funcionalidade de origem | F002, F004, F006, F008, F010, F011 |
| Componentes abrangidos | Frontend mobile |
| Pré-condições | O Candidato utiliza a aplicação móvel. |
| Critério de aceitação | Com o servidor desligado, uma tentativa de manifestar interesse apresenta a indicação de falha de ligação em 10 segundos ou menos. |
| Prioridade | Importante |
| Origem | Backlog de Projeto v02 (I023) (disponibilidade); Declaração de Âmbito v02 (secção 3); parâmetros P19 da secção 3 |

#### RNF015 — Carregar ficheiro de 5 MB em 5 segundos

| Campo | Conteúdo |
| --- | --- |
| ID | `RNF015` |
| Título | Carregar ficheiro de 5 MB em 5 segundos |
| Tipo | Não funcional |
| Ator | Não aplicável |
| Descrição | O sistema deve concluir o carregamento de um ficheiro de imagem ou de curriculum vitae com 5 MB num tempo não superior a 5 segundos, medido entre o início do envio e a confirmação da aceitação, no ambiente de demonstração. |
| Funcionalidade de origem | F003, F004, F005 |
| Componentes abrangidos | Backend |
| Pré-condições | O utilizador tem sessão iniciada e a operação de carregamento está autorizada ao seu tipo de conta. |
| Critério de aceitação | Um ficheiro PNG de 5 MB e um ficheiro PDF de 5 MB, carregados um de cada vez, são confirmados, cada um, em 5 segundos ou menos. |
| Prioridade | Importante |
| Origem | Backlog de Projeto v02 (I023); parâmetros P01, P02, P19 da secção 3 |

#### RNF016 — Rejeitar ficheiros com conteúdo diferente do formato admitido

| Campo | Conteúdo |
| --- | --- |
| ID | `RNF016` |
| Título | Rejeitar ficheiros com conteúdo diferente do formato admitido |
| Tipo | Não funcional |
| Ator | Não aplicável |
| Descrição | O sistema deve rejeitar 100% dos ficheiros carregados cujo conteúdo não corresponda ao formato JPEG, PNG ou PDF admitido para esse carregamento, independentemente da extensão do nome do ficheiro. |
| Funcionalidade de origem | F003, F004, F005 |
| Componentes abrangidos | Backend |
| Pré-condições | O utilizador tem sessão iniciada e a operação de carregamento está autorizada ao seu tipo de conta. |
| Critério de aceitação | Um ficheiro de texto com o nome «foto.png» carregado como fotografia de perfil e um ficheiro de imagem com o nome «cv.pdf» carregado como curriculum vitae são rejeitados. |
| Prioridade | Importante |
| Origem | Backlog de Projeto v02 (I023) (proteção do curriculum vitae); parâmetros P01, P02 da secção 3 |

#### RNF017 — Excluir dados confidenciais dos registos de diagnóstico

| Campo | Conteúdo |
| --- | --- |
| ID | `RNF017` |
| Título | Excluir dados confidenciais dos registos de diagnóstico |
| Tipo | Não funcional |
| Ator | Não aplicável |
| Descrição | O sistema deve registar 0 ocorrências dos dados seguintes nos registos de diagnóstico do servidor: palavras-passe, credenciais de sessão e texto das mensagens, medidas por pesquisa textual desses registos após a execução dos testes de sistema. |
| Funcionalidade de origem | F001, F010 |
| Componentes abrangidos | Backend |
| Pré-condições | Não aplicável |
| Critério de aceitação | A pesquisa textual dos registos de diagnóstico, após os testes de sistema, não encontra a palavra-passe, a credencial de sessão nem o texto de nenhuma mensagem utilizados nos testes. |
| Prioridade | Importante |
| Origem | Backlog de Projeto v02 (I023) (proteção de dados pessoais); Proposta de Sistema v02 (F010) |

#### RNF018 — Indicar falha de ligação na área web em 10 segundos

| Campo | Conteúdo |
| --- | --- |
| ID | `RNF018` |
| Título | Indicar falha de ligação na área web em 10 segundos |
| Tipo | Não funcional |
| Ator | Não aplicável |
| Descrição | O sistema deve apresentar ao Recrutador e ao Administrador uma indicação de falha de ligação num tempo não superior a 10 segundos após o envio, a partir da área de gestão web, de um pedido que não obtenha resposta do servidor. |
| Funcionalidade de origem | F001, F003, F005, F007, F009, F010 |
| Componentes abrangidos | Frontend web |
| Pré-condições | O utilizador tem a área de gestão web aberta. |
| Critério de aceitação | Com o servidor desligado, uma tentativa de publicar uma vaga apresenta a indicação de falha de ligação em 10 segundos ou menos. |
| Prioridade | Importante |
| Origem | Backlog de Projeto v02 (I023) (disponibilidade); Declaração de Âmbito v02 (secção 3); parâmetros P19 da secção 3 |

---

## 6. Restrições

Secção reservada aos requisitos `RSTR` que fixam imposições tecnológicas ou normativas, designadamente as tecnologias definidas na secção 11.2 do Regulamento Interno do Grupo e os limites transversais da secção 3 da Declaração de Âmbito v02, com numeração iniciada em `RSTR001`. A Issue responsável é definida no Backlog Refinement.

---

## 7. Cobertura funcional

Número de requisitos funcionais por funcionalidade e por ator. Apoia a métrica `MR-05` dos critérios de qualidade, aplicada às funcionalidades `F001` a `F011`, e a primeira versão da RTM (Issue `I024`).

| Funcionalidade | Candidato | Recrutador | Administrador | Sem ator direto | Total |
| --- | ---: | ---: | ---: | ---: | ---: |
| F001 | 3 | 5 | 6 | 1 | 15 |
| F002 | 3 | 0 | 3 | 0 | 6 |
| F003 | 1 | 8 | 6 | 1 | 16 |
| F004 | 7 | 0 | 0 | 0 | 7 |
| F005 | 0 | 11 | 1 | 1 | 13 |
| F006 | 9 | 0 | 0 | 1 | 10 |
| F007 | 0 | 6 | 0 | 3 | 9 |
| F008 | 2 | 2 | 0 | 2 | 6 |
| F009 | 0 | 0 | 9 | 1 | 10 |
| F010 | 6 | 6 | 1 | 3 | 16 |
| F011 | 7 | 8 | 0 | 0 | 15 |

Total de requisitos funcionais: 118. Um requisito associado a duas funcionalidades é contado em ambas.
