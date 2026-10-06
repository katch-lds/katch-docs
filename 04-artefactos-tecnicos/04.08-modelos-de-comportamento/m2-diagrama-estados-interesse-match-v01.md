# Diagrama de Estados — Interesse e Match

**Unidade curricular:** Laboratório de Desenvolvimento de Software — LDS
**Milestone:** `m2`
**Ficheiro:** `m2-diagrama-estados-interesse-match-v01.md`
**Pasta de arquivo:** `04-artefactos-tecnicos/04.08-modelos-de-comportamento`

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
| Documentos de origem | Ver a secção 1.1 |

### 1.1. Documentos de origem

| Documento | Caminho em `/projeto` | Versão | Estado |
| --- | --- | --- | --- |
| Proposta de Sistema | `04-artefactos-tecnicos/04.01-proposta-de-sistema-e-declaracao-de-ambito/m1-proposta-sistema-v02.pdf` | v02 | Homologada |
| Declaração de Âmbito | `04-artefactos-tecnicos/04.01-proposta-de-sistema-e-declaracao-de-ambito/m1-declaracao-ambito-v02.pdf` | v02 | Homologada |
| Especificação de Requisitos (RF001 a RF118) | `m2-especificacao-requisitos-v01.md` — caminho em `/projeto` por confirmar | v01 | Rascunho, não arquivada em `/projeto` à data deste documento |
| Especificações de Casos de Uso (UC05, UC11, UC12 e UC14) | `m2-especificacoes-casos-uso-v01.md` — caminho em `/projeto` por confirmar (rascunho em `Documentação/Rascunhos`) | v01 | Rascunho, sem versão aprovada |
| Modelo de dados PostgreSQL | `04-artefactos-tecnicos/04.06-modelos-de-dados/` — ficheiro `m2-modelo-de-dados-<nome-da-bd>-v01.md` previsto no Regulamento da UC e ainda não arquivado (Issue `I035`) | v02 (versão de trabalho) | **Provisório** |

**Correspondência provisória com o modelo de dados.** Os nomes de tabelas, campos e valores de `match_status` usados neste documento (incluindo `profile_opened_at` e `profile_opened_by`, acrescentados à tabela `match`) seguem a versão de trabalho do modelo de dados. Até à aprovação da `I035`, esta correspondência é provisória e deve ser reconferida quando o modelo for aprovado. Só a coluna «Correspondência no modelo de dados» da secção 4 e a secção 4.1 dependem dele.

### 1.2. Responsáveis

| Issue | Executor | Revisor | Auditor |
| --- | --- | --- | --- |
| `I086` — Diagrama de estados do interesse e do match | João Coelho | João Borguem | Miguel Santos |

### 1.3. Histórico de versões

| Versão | Data | Descrição das alterações | Issue |
| --- | --- | --- | --- |
| v01 | 2026-10-02 | Criação do documento e do diagrama de estados do interesse e do match. | `I086` |
| v01 | 2026-10-06 | Correções após a revisão: retirada a transição de encerramento sem decisão do Recrutador (D01); pseudoestado de escolha corrigido (D02); documentos de origem identificados (D03); correspondência com os estados do critério de aceitação e transições T5 e T6 a partir de «Perfil aberto» (M01, M02). | `I086` |

---

## 2. Contexto

O elemento modelado é a **resposta de um Candidato a uma vaga**. Começa com a recusa ou com a manifestação de interesse e pode terminar num match. Existe um único registo por par Candidato–vaga: o Candidato só responde uma vez a cada vaga.

O registo é criado quando o Candidato responde a uma vaga que lhe é apresentada (UC05). Se a quota de interesses estiver esgotada, o interesse é rejeitado e não é criado nenhum registo (RF020). No diagrama, este caso é o ramo `[interesse e quota esgotada]` do pseudoestado `resposta`, que termina sem criar registo e sem consumir interesse. A quota de interesses pertence ao Candidato e não a cada interesse, por isso o seu saldo não é modelado aqui; só a guarda a consulta.

O match não é uma entidade separada: é o estado final positivo do mesmo registo. A conversa entre as partes existe apenas enquanto o registo está em match e é modelada como subestado desse estado.

`REJECTED` é o estado final negativo e significa **encerrado sem match**. Tem duas causas: recusa do Candidato e recusa do Recrutador. A secção 4.1 mostra como as distinguir.

O interesse em espera não tem prazo de expiração (RF113): mantém-se em `WAITING` até o Recrutador aceitar ou recusar. O encerramento ou suspensão da vaga, a suspensão da Empresa e o bloqueio ou suspensão da conta do Candidato não alteram, nesta versão, o estado do interesse em espera. Ver a nota da secção 4.1.

**Correspondência com os estados do critério de aceitação.**

| Estado do critério de aceitação | Estado deste diagrama |
| --- | --- |
| Em espera | `WAITING` |
| Aceite | Decisão do Recrutador em T5; o registo passa a `MATCHED` |
| Ligação confirmada | `MATCHED` |
| Recusado | `REJECTED` (recusa do Candidato ou do Recrutador) |

«Aceite» não é um estado estável: a aceitação do Recrutador e a confirmação do match são atómicas (secção 6), por isso o registo nunca fica «aceite» sem estar em `MATCHED`.

No modelo de dados, o registo corresponde à tabela `match` e os estados principais coincidem com os valores do tipo `match_status`. Esta correspondência é provisória (secção 1.1).

---

## 3. Diagrama

```mermaid
stateDiagram-v2
    state resposta <<choice>>

    [*] --> resposta : Candidato responde a uma vaga [vaga elegível e sem resposta anterior]
    resposta --> REJECTED : [recusa] / regista a recusa do Candidato
    resposta --> WAITING : [interesse e quota disponível] / consome 1 interesse e notifica o Recrutador
    resposta --> [*] : [interesse e quota esgotada] / rejeita o interesse, sem registo (RF020)

    state WAITING {
        state "Perfil por abrir" as PerfilPorAbrir
        state "Perfil aberto" as PerfilAberto
        [*] --> PerfilPorAbrir
        PerfilPorAbrir --> PerfilPorAbrir : Recrutador tenta decidir / decisão rejeitada
        PerfilPorAbrir --> PerfilAberto : Recrutador abre o perfil completo / regista a abertura
    }

    PerfilAberto --> MATCHED : Recrutador aceita / confirma o match, cria a conversa e notifica as duas partes
    PerfilAberto --> REJECTED : Recrutador recusa / regista a decisão e exclui o Candidato da vaga
    REJECTED --> [*]

    state MATCHED {
        state "Conversa aberta" as ConversaAberta
        state "Conversa encerrada (só consulta)" as ConversaEncerrada
        [*] --> ConversaAberta
        ConversaAberta --> ConversaAberta : mensagem enviada por uma das partes
        ConversaAberta --> ConversaEncerrada : uma das partes encerra a conversa
        ConversaAberta --> ConversaEncerrada : conta bloqueada ou suspensa, ou Empresa suspensa
    }
```

**Notação.** As transições seguem a forma UML `evento [guarda] / ação`. O losango `resposta` é um pseudoestado de escolha: a transição que entra tem evento e guarda, e as que saem têm só guarda e ação, e cobrem todos os casos (recusa, interesse com quota disponível e interesse com quota esgotada). O ramo de quota esgotada termina sem registo, por isso não chega a nenhum estado. `WAITING` e `MATCHED` são estados compostos. `REJECTED` é um estado final: o registo mantém-se para histórico, mas não volta a mudar. `MATCHED` não tem saída, porque um match confirmado nunca é anulado. `Conversa encerrada` também não tem saída: a conversa não reabre, mesmo que a conta ou a Empresa seja reativada.

---

## 4. Estados

| Estado | Descrição | Correspondência no modelo de dados | Requisitos |
| --- | --- | --- | --- |
| `WAITING` — Em espera | O Candidato manifestou interesse e aguarda a decisão do Recrutador. Aparece na lista de candidatos em espera da vaga. | `candidate_status = INTERESTED`, `recruiter_status = PENDING`, `status = WAITING` | RF019, RF060, RF113 |
| `WAITING` › Perfil por abrir | O Recrutador ainda não abriu o perfil completo do Candidato para esta vaga. Nenhuma decisão é aceite. | `profile_opened_at IS NULL` | RF063, RF064, RF118 |
| `WAITING` › Perfil aberto | O Recrutador abriu o perfil completo e pode aceitar ou recusar. | `profile_opened_at` e `profile_opened_by` preenchidos | RF061, RF118 |
| `MATCHED` — Match confirmado | Existe interesse do Candidato e aceitação do Recrutador. Os dados de contacto estão disponíveis às duas partes. | `candidate_status = INTERESTED`, `recruiter_status = ACCEPTED`, `status = MATCHED`, `matched_at`, `decided_by` | RF025, RF066, RF067 |
| `MATCHED` › Conversa aberta | As duas partes trocam mensagens. | `conversation_status = OPEN` | RF026, RF068, RF069 |
| `MATCHED` › Conversa encerrada | A conversa fica só de consulta. O histórico mantém-se e não são aceites novas mensagens. Não reabre. | `conversation_status = CLOSED`, `conversation_closed_at`, `conversation_closed_by` | RF030, RF074, RF075 |
| `REJECTED` — Encerrado sem match | Estado final. O Candidato fica excluído desta vaga e esta não lhe volta a ser apresentada. As duas causas distinguem-se pelos campos abaixo. | `status = REJECTED` | RF018, RF064, RF065 |

### 4.1. Causas do estado `REJECTED`

| Causa | `candidate_status` | `recruiter_status` | Campos preenchidos | Requisitos |
| --- | --- | --- | --- | --- |
| Recusa do Candidato | `DECLINED` | `PENDING` | `candidate_action_at`; `recruiter_action_at` e `decided_by` ficam vazios | RF018 |
| Recusa do Recrutador | `INTERESTED` | `DECLINED` | `recruiter_action_at`, `decided_by`, `profile_opened_at` e `profile_opened_by` | RF064, RF065 |

**Nota — encerramento de interesses em espera (em análise).** Os efeitos do encerramento ou suspensão da vaga, da suspensão da Empresa e do bloqueio ou suspensão da conta do Candidato sobre os interesses em espera estão em análise em `m2-decisao-encerramento-interesses-em-espera-v01.md`, sem efeito nesta versão. Até à decisão, o interesse mantém-se em `WAITING` nesses casos (RF113: sem prazo de expiração).

---

## 5. Transições

| # | Origem | Evento | Guarda | Ação | Destino | Requisitos |
| --- | --- | --- | --- | --- | --- | --- |
| T0 | Início | Candidato responde a uma vaga apresentada | Vaga elegível e sem resposta anterior do Candidato | — | Pseudoestado `resposta` | RF013, RF014 |
| T1 | `resposta` | — | `[recusa]` | Regista a recusa; a vaga não volta a ser apresentada | `REJECTED` | RF014, RF018 |
| T2 | `resposta` | — | `[interesse e quota disponível]` | Consome 1 interesse da quota e notifica o Recrutador do novo interesse | `WAITING` › Perfil por abrir | RF019, RF076, RF113 |
| T2a | `resposta` | — | `[interesse e quota esgotada]` | Rejeita o interesse; não cria registo nem consome interesse | Fim, sem registo | RF020 |
| T3 | Perfil por abrir | Recrutador abre o perfil completo do Candidato | — | Regista a abertura do perfil para esta vaga | Perfil aberto | RF061, RF118 |
| T4 | Perfil por abrir | Recrutador tenta aceitar ou recusar | — | Rejeita a decisão | Perfil por abrir | RF063, RF064 |
| T5 | `WAITING` › Perfil aberto | Recrutador aceita o Candidato | — | Regista a decisão com autor e data, confirma o match, cria a conversa e notifica o Candidato e o Recrutador | `MATCHED` › Conversa aberta | RF063, RF065, RF066, RF068, RF031, RF077 |
| T6 | `WAITING` › Perfil aberto | Recrutador recusa o Candidato | — | Regista a decisão com autor e data e exclui o Candidato desta vaga | `REJECTED` | RF064, RF065 |
| T7 | Conversa aberta | Uma das partes envia uma mensagem | Mensagem não vazia e dentro do comprimento máximo | Guarda a mensagem e notifica o destinatário | Conversa aberta | RF026, RF069, RF032, RF078 |
| T8 | Conversa aberta | O Candidato ou o Recrutador encerra a conversa | — | Regista quem encerrou e quando | Conversa encerrada | RF030, RF074 |
| T9 | Conversa aberta | Bloqueio ou suspensão da conta de uma das partes, ou suspensão da Empresa | — | Passa a conversa a só de consulta | Conversa encerrada | RF075, RF086, RF089, RF095 |

---

## 6. Regras e invariantes

| Regra | Requisitos |
| --- | --- |
| Existe no máximo um registo por par Candidato–vaga. O Candidato não volta a responder a uma vaga já respondida. | RF014, RF019 |
| O match só se confirma com interesse do Candidato **e** aceitação do Recrutador. Nenhuma das partes o confirma sozinha. | RF066 |
| O Recrutador só decide depois de abrir o perfil completo do Candidato para essa vaga. | RF063, RF064, RF118 |
| Uma decisão registada não pode ser alterada. Se dois Recrutadores decidirem ao mesmo tempo sobre o mesmo registo, só a primeira decisão é aceite. | RF117 |
| `REJECTED` é final: o registo não volta a `WAITING` nem passa a `MATCHED`. A recusa do Candidato exclui a vaga; a decisão do Recrutador, uma vez registada, não pode ser alterada. | RF018, RF065, RF117 |
| Um interesse em `WAITING` não expira: mantém-se até o Recrutador aceitar ou recusar. | RF113 |
| Um match confirmado nunca é anulado. O encerramento da vaga e a suspensão da Empresa não eliminam o match. | F008 |
| A conversa encerrada não reabre, mesmo depois de reativada a conta ou a Empresa. | RF075, RF087, RF090, RF116 |
| Não há comunicação entre as partes fora do estado `MATCHED`. | RF070 |
| O Administrador não acede ao conteúdo das conversas em nenhum estado. | RF103 |
| A confirmação do match (T5) é atómica: a decisão, o match, a conversa e as duas notificações são registados em conjunto, ou nada é registado. | RF066, RF068, RF031, RF077 |
