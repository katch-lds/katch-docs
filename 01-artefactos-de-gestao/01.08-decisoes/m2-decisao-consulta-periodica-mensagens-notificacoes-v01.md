# Decisão — Consulta periódica de mensagens e notificações

**Unidade curricular:** Laboratório de Desenvolvimento de Software — LDS
**Milestone:** `m2`
**Ficheiro:** `m2-decisao-consulta-periodica-mensagens-notificacoes-v01.md`
**Pasta de arquivo:** `01-artefactos-de-gestao/01.08-decisoes`

---

## 1. Identificação

| Campo | Informação |
| --- | --- |
| Grupo | G11 |
| Sistema | Katch |
| Ano letivo | 2026/2027 |
| Versão do documento | v01 |
| Data | 2026-10-03 |
| Issue de origem | `I033` — Diagrama de arquitetura do sistema |
| Proposta por | João Coelho (Executor) |
| Revisor | João Borguem |
| Auditor | Roberto Baptista |
| Estado | Proposta, por ratificar em reunião do grupo |
| Reunião e ata de ratificação | A preencher depois da reunião |

Este documento complementa a fundamentação de uma decisão. A decisão formal fica sempre na ata da reunião que a ratificar.

---

## 2. Contexto

A Declaração de Âmbito v02 prevê que a conversa entre Candidato e Recrutador (F010) e as notificações (F011) sejam entregues em tempo real quando o destinatário tem a aplicação aberta. No âmbito backend da F010, a DA descreve essa entrega como uma ligação bidirecional persistente cliente-servidor, e a F011 reutiliza esse canal.

Os requisitos funcionais correspondentes (RF029, RF073, RF110 e RF111) exigem que as mensagens e as notificações surjam sem atualização manual, no máximo 5 segundos após o acontecimento que as origina, com a aplicação aberta e sessão iniciada (parâmetro `P17`). Não impõem nenhuma tecnologia.

A secção 3 da documentação de arquitetura (`m2-documentacao-arquitetura-v01.md`, Issue `I034`) propõe, na decisão D-08, uma ligação persistente com ASP.NET Core SignalR, e exclui a consulta periódica por entender que a DA a impede. A proposta está sujeita a confirmação em reunião formal.

Ao desenhar a arquitetura do sistema (`I033`), o grupo considerou que a ligação persistente traz uma complexidade desproporcionada para um sistema académico com poucos utilizadores:

- o cliente móvel (Flutter) não tem biblioteca oficial para SignalR e depende de pacotes da comunidade;
- a autenticação da ligação com o token JWT exige configuração própria;
- os testes de integração e de sistema ficam mais difíceis de escrever e menos estáveis com ligações persistentes.

---

## 3. Decisão

1. As mensagens e as notificações são entregues por **consulta periódica** (polling) à API REST, em HTTPS, sem ligação persistente entre os clientes e o backend.
2. As **mensagens** são consultadas a cada **3 segundos**, apenas enquanto a conversa está aberta.
3. As **notificações** são consultadas a cada **3 segundos**, apenas enquanto a aplicação está aberta com sessão iniciada.
4. Cada consulta devolve só os elementos novos desde o pedido anterior.
5. No pior caso, um acontecimento surge logo a seguir a uma consulta. O destinatário espera então até 3 segundos pela consulta seguinte e até 2 segundos pela resposta (RNF001), num total de 5 segundos, que é o limite do parâmetro `P17`.
6. Mantém-se tudo o resto que a DA prevê para estas funcionalidades: nenhuma notificação nativa do sistema operativo, nenhum envio por e-mail ou SMS e nenhuma entrega com a aplicação fechada.
7. Esta decisão **afasta-se da redação da DA**, que descreve uma ligação bidirecional persistente. O comportamento que o utilizador observa mantém-se: as mensagens e as notificações aparecem sem ação manual, dentro do limite de 5 segundos.

---

## 4. Alternativas consideradas

| Alternativa | Resultado |
| --- | --- |
| A. Ligação persistente por WebSocket, com SignalR (proposta D-08). | Rejeitada. Cumpre a redação da DA, mas tem a complexidade descrita na secção 2. |
| B. Consulta periódica à API REST, a cada 3 segundos. | **Escolhida.** Usa o mesmo canal HTTPS que o resto da aplicação, cumpre o limite de 5 segundos do `P17` e é simples de implementar e de testar. |

---

## 5. Consequências

| Elemento | Consequência |
| --- | --- |
| Arquitetura | O diagrama de arquitetura não tem o componente «Canal em tempo real». A API REST responde às consultas periódicas (`m2-documentacao-arquitetura-v01.md`, secção 2). |
| Stack tecnológica | A decisão D-08, o mapa da stack (secção 3.2) e o ponto 1 da secção 3.6 da documentação de arquitetura referem SignalR e ligação persistente. Se esta decisão for ratificada, têm de ser atualizados. |
| Declaração de Âmbito | A redação da F010 (âmbito backend) fica diferente da solução adotada. É preciso que o docente aceite o desvio, ou que a DA seja revista. A forma de o tratar é decidida na reunião de ratificação. |
| Requisitos | RF029, RF073, RF110, RF111 e o parâmetro `P17` mantêm-se sem alterações. O limite de 5 segundos fica cumprido no pior caso, como mostra o ponto 5 da secção 3. |
| Experiência do utilizador | Uma mensagem ou notificação pode demorar até 5 segundos a aparecer ao destinatário. |
| Carga no backend | Cada cliente faz uma consulta a cada 3 segundos por cada conversa aberta e uma consulta de notificações com a aplicação aberta. Com os utilizadores de demonstração do parâmetro `P18`, a carga é da ordem de 20 pedidos por segundo no máximo, aceitável no ambiente académico da DA. |
| Modelo de dados | Sem alterações. Os índices `message(match_id, sent_at)` e `notification(user_id, created_at DESC)` suportam as consultas dos elementos novos, e os índices parciais sobre `read_at IS NULL` suportam os contadores de não lidas. |
| Testes | Os testes de integração e de sistema deixam de depender de ligações persistentes. |

---

## 6. Requisitos relacionados

RF029, RF073, RF110, RF111, RNF001; parâmetro `P17`.
