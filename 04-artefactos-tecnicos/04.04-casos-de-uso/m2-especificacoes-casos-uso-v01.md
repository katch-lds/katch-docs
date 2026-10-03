# Especificações de Casos de Uso — Katch

**Unidade curricular:** Laboratório de Desenvolvimento de Software — LDS
**Milestone:** `m2`
**Ficheiro:** `m2-especificacoes-casos-uso-v01.md`
**Pasta de arquivo:** `04-artefactos-tecnicos/04.04-casos-de-uso`

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
| Documentos de origem | `m1-proposta-sistema-v02.pdf`, `m1-declaracao-ambito-v02.pdf` e `m2-especificacao-requisitos-v01.md` (RF001 a RF118) |

O documento segue a estrutura prevista na secção 19 do Regulamento de Funcionamento da Unidade Curricular para `04.04-casos-de-uso`: atores, objetivos, fronteira e diagrama geral de casos de uso (secções 2 a 7), seguidos da especificação dos casos de uso relevantes (secção 8).

### 1.1. Responsáveis por secção

| Secção | Issue | Executor | Revisor | Auditor |
| --- | --- | --- | --- | --- |
| 2 a 7. Atores, objetivos, fronteira, diagrama geral, catálogo e cobertura | `I025` | Roberto Baptista | João Borguem | João Coelho |
| 8.1. UC03 — Registar-se como Candidato | `I026` | João Coelho | Roberto Baptista | Miguel Santos |
| 8.2. UC07 — Registar a Empresa | `I027` | Roberto Baptista | João Borguem | João Coelho |
| 8.3. UC16 — Aprovar ou recusar o registo de Empresa | `I028` | João Borguem | Roberto Baptista | João Coelho |
| 8.4. UC09 — Publicar vaga | `I029` | Miguel Santos | João Borguem | Roberto Baptista |
| 8.5. UC05 — Dar swipe numa vaga | `I030` | Miguel Santos | João Borguem | João Coelho |
| 8.6. UC12 — Aceitar ou recusar candidato | `I031` | Miguel Santos | João Coelho | Roberto Baptista |
| 8.7. UC11 — Consultar perfil de candidato | `I032` | Roberto Baptista | João Borguem | Miguel Santos |

### 1.2. Histórico de versões

| Versão | Data | Descrição das alterações | Issue |
| --- | --- | --- | --- |
| v01 | 2026-10-01 | Criação do documento e das secções 2 a 7 (atores, objetivos, fronteira, diagrama de casos de uso geral, catálogo e cobertura). | `I025` |
| v01 | 2026-10-02 | Criação da secção 8.5 - Swipe Vaga  | `I030` |

Cada alteração posterior acrescenta uma linha. As versões anteriores são conservadas, nos termos da secção 18.2 do Regulamento de Funcionamento da Unidade Curricular.

---

## 2. Atores

Os três atores são os da secção 3 da Proposta de Sistema v02. Cada um acede ao sistema por um único ponto de acesso.

| Ator | Descrição | Ponto de acesso |
| --- | --- | --- |
| Candidato | Pessoa que procura oportunidades de trabalho. Mantém o perfil profissional, as competências e as preferências de procura, consulta as vagas que lhe são apresentadas e manifesta ou não interesse em cada uma. Depois de confirmado o match, comunica com o Recrutador responsável pela vaga. | Aplicação móvel (exclusivo) |
| Recrutador | Profissional que atua em nome de uma Empresa. Regista a Empresa, mantém a página de apresentação, publica e gere as vagas, consulta os candidatos que manifestaram interesse, decide sobre cada um e comunica com os candidatos com quem existe match. Só exerce estas funções depois de a Empresa ser aprovada. | Área de gestão web |
| Administrador | Responsável pela supervisão da plataforma. Decide sobre os registos de Empresas pendentes, mantém as listas pré-definidas, acompanha a atividade e consulta indicadores agregados. Não participa nas conversas nem acede ao seu conteúdo. | Área de gestão web |

---

## 3. Objetivos dos atores

| Ator | Objetivo | Casos de uso |
| --- | --- | --- |
| Candidato | Entrar na plataforma e começar a usá-la de imediato, sem aprovação. | UC03, UC01, UC02 |
| Candidato | Apresentar-se às empresas através de um perfil profissional completo. | UC04 |
| Candidato | Encontrar vagas compatíveis com as suas preferências e mostrar interesse nas que lhe agradam. | UC05, UC06 |
| Candidato | Saber quando há match e falar com o Recrutador. | UC13, UC14, UC15 |
| Recrutador | Colocar a Empresa na plataforma e obter a aprovação. | UC07, UC08, UC01, UC02 |
| Recrutador | Divulgar as vagas da Empresa e mantê-las atualizadas. | UC09, UC10 |
| Recrutador | Avaliar apenas candidatos com interesse real e decidir sobre cada um. | UC11, UC12 |
| Recrutador | Contactar os candidatos com quem existe match. | UC13, UC14, UC15 |
| Administrador | Garantir que só Empresas verificadas publicam vagas. | UC16, UC17 |
| Administrador | Manter a plataforma segura e as listas de referência atualizadas. | UC18, UC20, UC01, UC02 |
| Administrador | Acompanhar a atividade da plataforma. | UC19, UC17 |

---

## 4. Fronteira do sistema

A fronteira segue a Declaração de Âmbito v02.

| Dentro da fronteira (assegurado pelo sistema Katch) | Fora da fronteira (excluído pela DA v02) |
| --- | --- |
| Registo, autenticação e gestão das contas dos três atores, com validação automática dos registos. | Fornecedores externos de identidade. |
| Aprovação manual das Empresas pelo Administrador. | Integração com portais de emprego, redes profissionais e sistemas de gestão de recursos humanos. |
| Perfil profissional, exploração de vagas, quota de interesses e match. | Serviços externos de mapas ou de geolocalização; a distância é calculada com as coordenadas da lista pré-definida de localidades. |
| Conversa entre Candidato e Recrutador depois do match e notificações no interior das aplicações. | Envio de correio eletrónico, mensagens telefónicas ou notificações para fora da plataforma; serviços comerciais de comunicação; envio de ficheiros, imagens e mensagens de voz na conversa. |
| Área de supervisão e indicadores do Administrador. | Serviços de pagamento; migração de dados de aplicações anteriores; exploração real junto de candidatos ou empresas. |

No diagrama, a fronteira é o retângulo «Sistema Katch». Tudo o que está fora dele não é desenvolvido no projeto.

---

## 5. Diagrama de casos de uso geral

### 5.1. Convenções

Como o Mermaid não tem um tipo de diagrama UML de casos de uso, o diagrama usa um `flowchart` com as convenções seguintes:

| Elemento UML | Representação no diagrama |
| --- | --- |
| Ator | Retângulo fora da fronteira, com o nome do ator |
| Fronteira do sistema | Retângulo «Sistema Katch» |
| Caso de uso | Elipse (`([...])`) dentro da fronteira, identificada por `UCnn` |
| Associação ator–caso de uso | Linha contínua sem seta |
| Relação «include» | Seta tracejada do caso de uso base para o incluído (o incluído é sempre executado) |
| Relação «extend» | Seta tracejada do caso de uso que estende para o caso de uso base (a extensão é opcional) |

As cores estão fixadas no próprio diagrama (fundo branco, linhas escuras), para que o diagrama se leia da mesma forma em tema claro e em tema escuro.

Os comportamentos que o sistema executa sem intervenção direta de um ator (por exemplo, o cálculo da distância, o estado pendente da Empresa, o encerramento automático da vaga na data-limite, a criação da conversa e a geração das notificações) não são casos de uso autónomos. Fazem parte do caso de uso que os desencadeia e estão identificados no catálogo (secção 6).

### 5.2. Diagrama

```mermaid
flowchart LR
    ACand["Candidato"]
    ARec["Recrutador"]
    AAdm["Administrador"]

    subgraph Katch["Sistema Katch"]
        direction TB
        UC03(["UC03 — Registar-se como Candidato"])
        UC04(["UC04 — Gerir o perfil profissional"])
        UC05(["UC05 — Dar swipe numa vaga"])
        UC06(["UC06 — Consultar a página de apresentação da Empresa"])
        UC01(["UC01 — Iniciar e terminar sessão"])
        UC02(["UC02 — Alterar a própria palavra-passe"])
        UC13(["UC13 — Consultar matches e dados de contacto"])
        UC14(["UC14 — Trocar mensagens na conversa do match"])
        UC15(["UC15 — Consultar notificações"])
        UC07(["UC07 — Registar a Empresa"])
        UC08(["UC08 — Gerir a página e os dados da Empresa"])
        UC09(["UC09 — Publicar vaga"])
        UC10(["UC10 — Gerir vagas"])
        UC11(["UC11 — Consultar perfil de candidato"])
        UC12(["UC12 — Aceitar ou recusar candidato"])
        UC16(["UC16 — Aprovar ou recusar o registo de Empresa"])
        UC17(["UC17 — Supervisionar Empresas e vagas"])
        UC18(["UC18 — Gerir contas de utilizador"])
        UC19(["UC19 — Consultar o painel de indicadores"])
        UC20(["UC20 — Gerir listas pré-definidas"])
    end

    ACand --- UC01
    ACand --- UC02
    ACand --- UC03
    ACand --- UC04
    ACand --- UC05
    ACand --- UC06
    ACand --- UC13
    ACand --- UC14
    ACand --- UC15

    ARec --- UC01
    ARec --- UC02
    ARec --- UC07
    ARec --- UC08
    ARec --- UC09
    ARec --- UC10
    ARec --- UC11
    ARec --- UC12
    ARec --- UC13
    ARec --- UC14
    ARec --- UC15

    UC01 --- AAdm
    UC02 --- AAdm
    UC16 --- AAdm
    UC17 --- AAdm
    UC18 --- AAdm
    UC19 --- AAdm
    UC20 --- AAdm

    UC12 -. "«include»" .-> UC11
    UC06 -. "«extend»" .-> UC05

    classDef ator fill:#ffffff,stroke:#1f2937,stroke-width:2px,color:#111827
    classDef uc fill:#eef2ff,stroke:#3730a3,stroke-width:1.5px,color:#111827
    class ACand,ARec,AAdm ator
    class UC01,UC02,UC03,UC04,UC05,UC06,UC07,UC08,UC09,UC10,UC11,UC12,UC13,UC14,UC15,UC16,UC17,UC18,UC19,UC20 uc
    style Katch fill:#ffffff,stroke:#1f2937,stroke-width:2px,color:#111827
    linkStyle default stroke:#374151,stroke-width:1.5px
```

### 5.3. Relações entre casos de uso

| Relação | Significado | Requisitos |
| --- | --- | --- |
| UC12 «include» UC11 | O Recrutador só pode aceitar ou recusar um Candidato depois de abrir o perfil completo; a consulta do perfil faz sempre parte da decisão. | RF061, RF063, RF064, RF118 |
| UC06 «extend» UC05 | Ao consultar uma vaga, o Candidato pode, se quiser, abrir a página de apresentação da Empresa; não é obrigatório para recusar a vaga ou manifestar interesse. | RF016, RF017 |

---

## 6. Catálogo dos casos de uso

| ID | Caso de uso | Ator(es) | Funcionalidades | Requisitos funcionais | Comportamento do sistema incluído | Especificação detalhada |
| --- | --- | --- | --- | --- | --- | --- |
| UC01 | Iniciar e terminar sessão | Candidato, Recrutador, Administrador | F001 | RF001, RF105 (Candidato); RF038, RF106 (Recrutador); RF083, RF107 (Administrador) | RF039 (operações reservadas só com Empresa aprovada), RF112 (registo do autor das operações) | — |
| UC02 | Alterar a própria palavra-passe | Candidato, Recrutador, Administrador | F001 | RF002 (Candidato); RF040 (Recrutador); RF084 (Administrador) | — | — |
| UC03 | Registar-se como Candidato | Candidato | F002 | RF003, RF004, RF005 | Validação automática e ativação imediata da conta | `I026` (secção 8.1) |
| UC04 | Gerir o perfil profissional | Candidato | F004 | RF006 a RF012 | — | — |
| UC05 | Dar swipe numa vaga | Candidato | F006, F011 | RF013, RF014, RF016, RF018 a RF023 | RF015 (distância em linha reta), RF113 (interesse em espera), RF033 (notificação da reposição da quota), RF076 (notificação de novo interesse ao Recrutador) | `I030` (secção 8.5) |
| UC06 | Consultar a página de apresentação da Empresa | Candidato | F003 | RF017 | — | — |
| UC07 | Registar a Empresa | Recrutador | F001, F003 | RF037, RF041, RF042, RF044, RF045 | RF043 (Empresa em estado pendente) | `I027` (secção 8.2) |
| UC08 | Gerir a página e os dados da Empresa | Recrutador | F003 | RF046, RF047, RF048, RF049 | — | — |
| UC09 | Publicar vaga | Recrutador | F005 | RF050, RF051, RF052, RF053 | RF039 (só com Empresa aprovada) | `I029` (secção 8.4) |
| UC10 | Gerir vagas | Recrutador | F005 | RF054, RF055, RF056, RF058, RF059, RF114, RF115 | RF057 (encerramento automático na data-limite) | — |
| UC11 | Consultar perfil de candidato | Recrutador | F004, F007 | RF060, RF061, RF062 | RF113 (interesse em espera até à decisão), RF118 (registo da abertura do perfil por vaga) | `I032` (secção 8.7) |
| UC12 | Aceitar ou recusar candidato | Recrutador | F007, F008, F010 | RF063, RF064, RF066, RF117 | RF065 (registo da decisão), RF068 (criação automática da conversa), RF031 e RF077 (notificação de novo match) | `I031` (secção 8.6) |
| UC13 | Consultar matches e dados de contacto | Candidato, Recrutador | F008 | RF024, RF025 (Candidato); RF067 (Recrutador) | — | — |
| UC14 | Trocar mensagens na conversa do match | Candidato, Recrutador | F010 | RF026 a RF030, RF108 (Candidato); RF069, RF071 a RF074, RF109 (Recrutador) | RF070 (comunicação só depois do match), RF075 (conversa só de consulta por bloqueio ou suspensão), RF103 (Administrador sem acesso às conversas), RF032 e RF078 (notificação de nova mensagem) | — |
| UC15 | Consultar notificações | Candidato, Recrutador | F011 | RF034, RF035, RF036, RF110 (Candidato); RF080, RF081, RF082, RF111 (Recrutador) | Notificações geradas pelos outros casos de uso (RF031 a RF033, RF076 a RF079) | — |
| UC16 | Aprovar ou recusar o registo de Empresa | Administrador | F003, F009, F011 | RF091, RF092, RF093, RF094 | RF079 (notificação da decisão ao Recrutador) | `I028` (secção 8.3) |
| UC17 | Supervisionar Empresas e vagas | Administrador | F003, F005, F009 | RF095, RF097, RF098, RF116 | RF075 (conversas em consulta quando a Empresa é suspensa) | — |
| UC18 | Gerir contas de utilizador | Administrador | F001, F002 | RF085 a RF090 | RF075 (conversas em consulta quando a conta é bloqueada ou suspensa) | — |
| UC19 | Consultar o painel de indicadores | Administrador | F009 | RF096 | RF104 (área de supervisão reservada ao Administrador) | — |
| UC20 | Gerir listas pré-definidas | Administrador | F009 | RF099, RF100, RF101, RF102 | — | — |

---

## 7. Verificação do diagrama

### 7.1. Restrições representadas pela ausência de associação

| Restrição | Como aparece no diagrama | Requisito |
| --- | --- | --- |
| O Administrador não acede às conversas. | Não existe associação entre o Administrador e o UC14. | RF103 |
| O Recrutador não procura candidatos que não tenham manifestado interesse nas suas vagas. | Não existe caso de uso de pesquisa livre de candidatos; o Recrutador só chega a um perfil através do UC11, que parte da lista de candidatos em espera de uma vaga. | RF060, RF061 |
| Só o Administrador decide sobre o registo de Empresas. | O UC16 está associado apenas ao Administrador. | RF093, RF094 |
| O Candidato não publica vagas nem decide sobre candidatos. | O Candidato não está associado aos UC07 a UC12. | F005, F007 |

### 7.2. Cobertura

| Verificação | Resultado |
| --- | --- |
| Funcionalidades F001 a F011 com, pelo menos, um caso de uso | F001 (UC01, UC02, UC07, UC18), F002 (UC03, UC18), F003 (UC06, UC07, UC08, UC16, UC17), F004 (UC04, UC11), F005 (UC09, UC10, UC17), F006 (UC05), F007 (UC11, UC12), F008 (UC12, UC13), F009 (UC16, UC17, UC19, UC20), F010 (UC12, UC14), F011 (UC05, UC15, UC16) |
| Requisitos funcionais RF001 a RF118 associados a um caso de uso | 118 de 118 (colunas «Requisitos funcionais» e «Comportamento do sistema incluído» da secção 6) |
| Atores | Candidato, Recrutador e Administrador, os três da Proposta de Sistema v02 |
| Casos de uso especificados em detalhe | UC03, UC05, UC07, UC09, UC11, UC12 e UC16 (Issues `I026` a `I032`) |

---

## 8. Especificações dos casos de uso

As especificações são acrescentadas neste ficheiro pelas Issues `I026` a `I032`, pela numeração da secção 1.1. Cada especificação segue a estrutura seguinte, exigida pela secção 19 do Regulamento de Funcionamento da Unidade Curricular:

| Campo | Conteúdo |
| --- | --- |
| Identificação | `UCnn` e nome, igual ao do diagrama |
| Ator principal | Ator que inicia o caso de uso |
| Objetivo | Resultado que o ator pretende obter |
| Pré-condições | Estado necessário antes do início |
| Fluxo principal | Passos numerados da interação entre o ator e o sistema |
| Fluxos alternativos | Variações válidas do fluxo principal, com o passo onde começam |
| Exceções | Situações de erro ou rejeição e a resposta do sistema |
| Pós-condições | Estado do sistema no fim, com sucesso |
| Requisitos relacionados | IDs dos RF (e RNF, quando existirem) da especificação de requisitos |

### 8.1. UC03 — Registar-se como Candidato

> A preencher pela Issue `I026`.

### 8.2. UC07 — Registar a Empresa

> A preencher pela Issue `I027`.

### 8.3. UC16 — Aprovar ou recusar o registo de Empresa

> A preencher pela Issue `I028`.

### 8.4. UC09 — Publicar vaga

> A preencher pela Issue `I029`.

### 8.5. UC05 — Dar swipe numa vaga

| Campo | Conteúdo |
| --- | --- |
| Identificação | UC05 — Dar swipe numa vaga |
| Ator principal | Candidato |
| Objetivo | Percorrer as vagas compatíveis com as preferências de procura e, em cada uma, recusá-la ou manifestar interesse, para que o Recrutador possa avaliar a candidatura. |
| Requisitos relacionados | RF013, RF014, RF016, RF018, RF019, RF020, RF021, RF022, RF023; RF015 (distância em linha reta); RF113 (interesse em espera até à decisão); RF033 (notificação da reposição da quota); RF076 (notificação de novo interesse ao Recrutador); RF112 (registo do autor e da data e hora do interesse e da recusa); RF017, pelo UC06 que estende este caso de uso |

**Pré-condições**

1. O Candidato tem sessão iniciada na aplicação móvel e a conta está ativa.

**Fluxo principal (manifestar interesse)**

1. O Candidato abre a área de exploração de vagas.
2. O sistema seleciona as vagas que cumprem cumulativamente as condições seguintes: vaga publicada; Empresa aprovada; vaga ainda não recusada nem objeto de interesse do Candidato; distância não superior à distância máxima, exceto nas vagas em regime remoto; valor máximo do intervalo salarial não inferior à pretensão salarial mínima; regime de trabalho e tipo de contrato incluídos nas preferências de procura. A distância é calculada em linha reta, em quilómetros arredondados às unidades, a partir das coordenadas das localidades do Candidato e da vaga.
3. O sistema apresenta o cartão de uma vaga, com função, designação e logótipo da Empresa, intervalo salarial, localidade, distância, tipo de contrato, regime de trabalho, indicação de urgência e fotografias (logótipo e fotografias só quando registados), e o número de interesses disponíveis na quota de 10.
4. O Candidato manifesta interesse na vaga (swipe para a direita).
5. O sistema verifica que o Candidato tem, pelo menos, um interesse disponível, que ainda não manifestou interesse nessa vaga e que a vaga continua a cumprir as condições do passo 2.
6. O sistema regista o interesse no estado em espera de resposta, com o Candidato e a data e hora, e reduz em uma unidade os interesses disponíveis.
7. O sistema gera uma notificação de novo interesse, que identifica a vaga, dirigida ao Recrutador.
8. Se este interesse esgotou a quota, o sistema bloqueia novos interesses durante as vinte e quatro horas seguintes ao instante em que foi registado.
9. O sistema deixa de apresentar essa vaga ao Candidato e apresenta o cartão seguinte, com o número de interesses disponíveis atualizado. O caso de uso continua no passo 4 para cada cartão, até o Candidato sair da área de exploração.

**Fluxos alternativos**

*A1 — Recusar a vaga* (começa no passo 4)

1. O Candidato recusa a vaga (swipe para a esquerda).
2. O sistema regista a recusa, com o Candidato e a data e hora, sem consumir a quota de interesses. Não existe limite ao número de recusas.
3. O sistema deixa de apresentar essa vaga ao Candidato e apresenta o cartão seguinte. O caso de uso continua no passo 4.

*A2 — Consultar o detalhe da vaga* (começa no passo 4)

1. O Candidato abre o detalhe da vaga apresentada.
2. O sistema apresenta a descrição completa, as competências pretendidas, os benefícios e o acesso à página de apresentação da Empresa.
3. Se o Candidato quiser, consulta a página de apresentação da Empresa (UC06, que estende este caso de uso).
4. O Candidato regressa ao cartão e o caso de uso continua no passo 4.

*A3 — Ajustar as preferências de procura* (começa no passo 3 ou no passo 4)

1. O Candidato altera as preferências de procura a partir da área de exploração.
2. O sistema grava as novas preferências e aplica-as às vagas apresentadas a seguir à alteração. O caso de uso continua no passo 2.

*A4 — Quota esgotada* (começa no passo 3)

1. O Candidato está no período de bloqueio de vinte e quatro horas.
2. O sistema apresenta o cartão com a indicação de que a quota está esgotada e o tempo em falta até à reposição.
3. O Candidato pode recusar a vaga (A1), consultar o detalhe (A2) ou ajustar as preferências (A3). Uma tentativa de manifestar interesse corresponde à exceção E1.

*A5 — Fim do período de bloqueio* (desencadeado pelo sistema, sem ação do Candidato)

1. Decorridas vinte e quatro horas sobre o interesse que esgotou a quota, o sistema repõe integralmente a quota em 10 interesses.
2. O sistema gera uma notificação de reposição da quota, dirigida ao Candidato.

*A6 — Sem vagas para apresentar* (começa no passo 3)

1. Nenhuma vaga cumpre as condições do passo 2.
2. O sistema indica que não existem vagas compatíveis com as preferências de procura. O Candidato pode ajustar as preferências (A3).

**Exceções**

| ID | Situação | Resposta do sistema |
| --- | --- | --- |
| E1 | O Candidato tenta manifestar interesse com a quota esgotada, dentro das vinte e quatro horas de bloqueio (passo 5). | Rejeita o interesse e apresenta o tempo em falta até ao fim do bloqueio. Nada é registado e a quota não é alterada. |
| E2 | O Candidato já manifestou interesse nessa vaga (passo 5). | Rejeita a segunda manifestação de interesse. Nada é registado e a quota não é alterada. |
| E3 | Entre a apresentação do cartão e a ação do Candidato, a vaga deixou de cumprir as condições do passo 2, por exemplo por ter sido suspensa ou encerrada, ou por a Empresa ter sido suspensa (passo 5). | Rejeita a ação, informa que a vaga já não está disponível e apresenta o cartão seguinte. Nada é registado e a quota não é alterada. |

**Pós-condições**

- Interesse (sucesso): o interesse está registado no estado em espera de resposta, com o Candidato e a data e hora, e o Candidato surge na lista de candidatos em espera dessa vaga; os interesses disponíveis diminuíram uma unidade; o Recrutador recebeu a notificação de novo interesse; a vaga não volta a ser apresentada ao Candidato. Se a quota ficou esgotada, novos interesses estão bloqueados durante vinte e quatro horas a contar desse interesse, findas as quais a quota é reposta em 10 e o Candidato é notificado.
- Recusa (A1): a recusa está registada, com o Candidato e a data e hora; a quota não foi alterada; a vaga não volta a ser apresentada ao Candidato.
- Em qualquer exceção, nada é registado e a quota de interesses não é alterada.

### 8.6. UC12 — Aceitar ou recusar candidato

> A preencher pela Issue `I031`.

### 8.7. UC11 — Consultar perfil de candidato

> A preencher pela Issue `I032`.
