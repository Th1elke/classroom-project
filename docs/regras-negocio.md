# Regras de Negócio — AulaConecta

Foram identificadas **8 regras de negócio** relevantes para o domínio de gestão de sala de aula online. Ao final, demonstramos como **duas delas (RN03 e RN06)** influenciam User Stories, processos de negócio (BPMN), modelagem e decisões arquiteturais.

## 1. Regras de Negócio

| ID | Regra de Negócio |
|----|------------------|
| **RN01** | Apenas **professores** (e coordenadores) podem criar turmas e atividades. Estudantes não têm essa permissão. |
| **RN02** | Um estudante só acessa o conteúdo de uma turma **após estar matriculado e aprovado** nela. |
| **RN03** | Entregas de atividades só são aceitas **até a data/hora limite**. Após o prazo, a entrega é bloqueada ou marcada como *atrasada*, conforme a política definida pelo professor na turma. |
| **RN04** | Cada atividade possui uma **nota máxima**, e a correção só pode ser realizada pelo **professor responsável** pela turma. |
| **RN05** | A matrícula em uma turma exige um **código de convite** (ou aprovação do professor), garantindo que apenas estudantes autorizados ingressem. |
| **RN06** | O sistema deve **notificar automaticamente** os estudantes quando uma nova atividade for publicada e quando uma nota for lançada. |
| **RN07** | Um estudante **não pode editar uma entrega após ela ter sido corrigida** (nota lançada). |
| **RN08** | Dados pessoais e notas de um estudante só são visíveis ao **próprio estudante** e aos **professores/coordenadores** da turma (conformidade com a LGPD). |

## 2. Demonstração de impacto

### RN03 — Entregas só são aceitas até o prazo

| Dimensão | Impacto |
|----------|---------|
| **User Story** | US04 — *Como estudante, desejo entregar uma atividade antes do prazo para que minha tarefa seja registrada e avaliada.* (Critério de aceitação inclui o bloqueio/marcação após o prazo.) |
| **Caso de Uso** | "Entregar Atividade" — com fluxo alternativo de **prazo expirado**. |
| **BPMN** | No processo *Entrega de Atividade*, há um **gateway exclusivo** que verifica `data_atual ≤ data_limite`. Se verdadeiro, registra a entrega; se falso, segue o caminho de *entrega bloqueada/atrasada*. |
| **Arquitetura** | A validação de prazo deve ser feita **no backend** (não confiar no relógio do cliente), exigindo um serviço com **fonte de tempo confiável**; impacta a consistência e a necessidade de **agendamento** (jobs para fechar prazos e marcar atrasos). |
| **Requisito Não Funcional** | **Confiabilidade/Consistência temporal** — o horário do servidor é a referência única. |

### RN06 — Notificações automáticas de eventos

| Dimensão | Impacto |
|----------|---------|
| **User Story** | US07 — *Como estudante, desejo receber notificações de novas atividades e notas para não perder prazos.* |
| **Caso de Uso** | "Notificar Estudante" — disparado por *Publicar Atividade* e *Lançar Nota*. |
| **BPMN** | Nos processos *Publicação de Atividade* e *Correção de Atividade*, há um **evento de envio de notificação** ao final do fluxo. |
| **Arquitetura** | A notificação não deve **bloquear** a publicação/correção. Isso favorece uma abordagem **assíncrona, orientada a eventos** (publish/subscribe ou fila de mensagens), em que o serviço de notificações **consome eventos** emitidos por outros componentes. |
| **Decisão Arquitetural** | Justifica a adoção de um **mecanismo de mensageria/eventos** (a ser detalhado em `arquitetura.md`), que também é **reaproveitado** pela Funcionalidade Inovadora (Painel de Risco de Evasão), que consome os mesmos eventos de engajamento. |
| **Requisito Não Funcional** | **Desempenho e desacoplamento** — operações principais não esperam o envio das notificações. |

## 3. Rastreabilidade (resumo)

```
RN03 → US04 → UC "Entregar Atividade" → BPMN (gateway de prazo) → Backend valida tempo → RNF Consistência
RN06 → US07 → UC "Notificar Estudante" → BPMN (evento de notificação) → Mensageria assíncrona → Decisão Arquitetural
RN01 → US01/US03 → Controle de acesso por papel → Autenticação/Autorização (RNF Segurança)
```

> O objetivo não é apenas listar regras, mas mostrar como elas **moldam** os requisitos, os processos e a arquitetura da solução.
