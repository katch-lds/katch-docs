# Decisão — Encerramento de interesses em espera

**Unidade curricular:** Laboratório de Desenvolvimento de Software — LDS
**Milestone:** `m2`
**Ficheiro:** `m2-decisao-encerramento-interesses-em-espera-v01.md`
**Pasta de arquivo:** `01-artefactos-de-gestao/01.08-decisoes`

---

## 1. Identificação

| Campo | Informação |
| --- | --- |
| Grupo | G11 |
| Sistema | Katch |
| Ano letivo | 2026/2027 |
| Versão do documento | v01 |
| Data | 2026-10-02 |
| Issue de origem | `I086` — Diagrama de estados do interesse e do match |
| Proposta por | João Coelho (Executor) |
| Revisor | João Borguem |
| Auditor | Miguel Santos |
| Estado | Proposta, por ratificar em reunião do grupo |
| Reunião e ata de ratificação | A preencher depois da reunião |

Este documento complementa a fundamentação de uma decisão. A decisão formal fica sempre na ata da reunião que a ratificar.

---

## 2. Contexto

A Proposta de Sistema e a Declaração de Âmbito não dizem o que acontece a um interesse em espera quando a vaga é encerrada ou suspensa, a Empresa é suspensa ou a conta do Candidato é bloqueada ou suspensa. Só dizem que as ligações já confirmadas (match) se mantêm.

O diagrama de estados do interesse e do match (`m2-diagrama-estados-interesse-match-v01.md`) precisa de uma resposta, porque o interesse em espera não pode ficar indefinidamente na lista de candidatos de uma vaga que já não está publicada.

O RF113 ("Manter o interesse em espera até à decisão") mantém o interesse em espera de resposta "até à aceitação ou recusa pelo Recrutador, sem prazo de expiração". À letra, é incompatível com esse encerramento. O critério de consistência (CQ-04, regra 5) exige que a resolução de uma incompatibilidade seja registada em decisão formal.

---

## 3. Decisão

1. O interesse em espera é **encerrado sem match** quando se verifica uma destas situações:
   - a vaga é encerrada ou suspensa;
   - a Empresa que publicou a vaga é suspensa;
   - a conta do Candidato é bloqueada ou suspensa.
2. O encerramento é automático e não exige decisão do Recrutador. No diagrama de estados corresponde à transição T7, que leva o registo ao estado final `REJECTED`.
3. O encerramento é definitivo. A reativação da vaga, da Empresa ou da conta não repõe o interesse em espera.
4. Os matches já confirmados não são afetados.
5. O RF113 **mantém-se com o texto atual**. O comportamento novo é especificado em três requisitos autónomos (RF119, RF120 e RF121, no anexo), que tratam o encerramento como exceção ao RF113 para as condições que enunciam.

---

## 4. Alternativas consideradas

| Alternativa | Resultado |
| --- | --- |
| A. Reescrever o RF113 para admitir o encerramento. | Rejeitada: o grupo não quer alterar o RF113. |
| B. Retirar a transição T7 e deixar o interesse em espera para sempre. | Rejeitada: o RF113 ficava cumprido à letra, mas o Recrutador continuaria a ver candidatos em espera numa vaga ou Empresa que já não estão ativas. |
| C. Manter a T7 e especificá-la em requisitos próprios. | **Escolhida.** |

---

## 5. Consequências

| Elemento | Consequência |
| --- | --- |
| Requisitos | Acrescentar RF119, RF120 e RF121 à especificação de requisitos, com a redação proposta no anexo. |
| RTM | Acrescentar uma linha por cada novo requisito. |
| Diagrama de estados | A transição T7 passa a referir os RF119 a RF121. |
| Candidato | Quando uma vaga suspensa for republicada (RF115) ou uma Empresa for reativada (RF116), o Candidato cujo interesse foi encerrado não pode voltar a manifestar interesse nessa vaga, porque só existe um registo por par Candidato–vaga. |
| Modelo de dados | O encerramento usa os valores já existentes: `status = REJECTED`, `candidate_status = INTERESTED` e `recruiter_status = PENDING`. O modelo ganhou os campos `profile_opened_at` e `profile_opened_by` na tabela `match`, por causa do RF118 e não por esta decisão. |

---

## 6. Requisitos relacionados

RF055, RF056, RF057, RF086, RF089, RF095, RF113, RF115, RF116, RF119, RF120, RF121.

---

## Anexo — Redação proposta dos requisitos

Redação no formato da secção 3.2 de `m1-criterios-qualidade-requisitos-v01.md` e das convenções da secção 2.1 da especificação de requisitos. Os identificadores são os seguintes da sequência e têm de ser confirmados na especificação. A prioridade e as funcionalidades de origem são propostas do Executor e devem ser confirmadas na revisão.

#### RF119 — Encerrar interesse em espera ao encerrar ou suspender vaga

| Campo | Conteúdo |
| --- | --- |
| ID | `RF119` |
| Título | Encerrar interesse em espera ao encerrar ou suspender vaga |
| Tipo | Funcional |
| Ator | Não aplicável |
| Descrição | O sistema deve encerrar sem match o interesse do Candidato em espera de resposta, quando a vaga a que o interesse se refere passa ao estado encerrada ou suspensa. |
| Funcionalidade de origem | F005, F007 |
| Componentes abrangidos | Backend |
| Pré-condições | Existe um interesse do Candidato em espera de resposta numa vaga publicada. |
| Critério de aceitação | Quando a vaga passa ao estado encerrada ou suspensa, o interesse deixa de constar da lista de candidatos em espera da vaga e o Recrutador deixa de poder decidir sobre ele. |
| Prioridade | Importante |
| Origem | Decisão `m2-decisao-encerramento-interesses-em-espera-v01.md` (I086) |

#### RF120 — Encerrar interesses em espera ao suspender Empresa

| Campo | Conteúdo |
| --- | --- |
| ID | `RF120` |
| Título | Encerrar interesses em espera ao suspender Empresa |
| Tipo | Funcional |
| Ator | Não aplicável |
| Descrição | O sistema deve encerrar sem match o interesse do Candidato em espera de resposta, quando a Empresa que publicou a vaga passa ao estado suspensa. |
| Funcionalidade de origem | F003, F007 |
| Componentes abrangidos | Backend |
| Pré-condições | Existe um interesse do Candidato em espera de resposta numa vaga de uma Empresa aprovada. |
| Critério de aceitação | Quando a Empresa passa ao estado suspensa, o interesse deixa de constar da lista de candidatos em espera da vaga e o Recrutador deixa de poder decidir sobre ele. |
| Prioridade | Importante |
| Origem | Decisão `m2-decisao-encerramento-interesses-em-espera-v01.md` (I086) |

#### RF121 — Encerrar interesses em espera ao bloquear conta do Candidato

| Campo | Conteúdo |
| --- | --- |
| ID | `RF121` |
| Título | Encerrar interesses em espera ao bloquear conta do Candidato |
| Tipo | Funcional |
| Ator | Não aplicável |
| Descrição | O sistema deve encerrar sem match os interesses em espera de resposta do Candidato, quando a conta do Candidato passa ao estado bloqueada ou suspensa. |
| Funcionalidade de origem | F001, F002, F007 |
| Componentes abrangidos | Backend |
| Pré-condições | O Candidato tem pelo menos um interesse em espera de resposta. |
| Critério de aceitação | Quando a conta do Candidato passa ao estado bloqueada ou suspensa, os interesses em espera do Candidato deixam de constar das listas de candidatos em espera das vagas e os Recrutadores deixam de poder decidir sobre eles. |
| Prioridade | Importante |
| Origem | Decisão `m2-decisao-encerramento-interesses-em-espera-v01.md` (I086) |
