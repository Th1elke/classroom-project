# User Stories — AulaConecta

10 User Stories cobrindo o ciclo central da plataforma. Cada uma possui **identificador**, **descrição** (papel / desejo / benefício) e **critérios de aceitação** no formato *Dado / Quando / Então*. Ao lado, indicamos as **Regras de Negócio** relacionadas.

---

## US01 — Criar turma
**Como** professor, **desejo** criar uma turma com nome e código de convite, **para** organizar meus estudantes e atividades.
*Regras relacionadas: RN01, RN05*

**Critérios de aceitação**
- Dado que sou um professor autenticado, quando preencho o nome da turma e confirmo, então a turma é criada e recebo um **código de convite** único.
- Dado que sou um estudante, quando tento acessar a criação de turma, então a ação é **negada**.

---

## US02 — Matricular-se em turma
**Como** estudante, **desejo** ingressar em uma turma usando um código de convite, **para** acessar suas atividades e conteúdos.
*Regras relacionadas: RN02, RN05*

**Critérios de aceitação**
- Dado um código válido, quando informo o código, então sou matriculado e passo a ver o conteúdo da turma.
- Dado um código inválido ou expirado, quando informo o código, então recebo uma mensagem de erro e **não** sou matriculado.
- Dado que não estou matriculado, quando tento abrir a turma diretamente, então o acesso é **bloqueado**.

---

## US03 — Publicar atividade
**Como** professor, **desejo** publicar uma atividade com descrição, prazo e nota máxima, **para** disponibilizar tarefas à turma.
*Regras relacionadas: RN01, RN04, RN06*

**Critérios de aceitação**
- Dado que sou professor da turma, quando publico uma atividade com **data limite** e **nota máxima**, então ela aparece para todos os estudantes matriculados.
- Dado que a atividade foi publicada, quando a publicação é concluída, então os estudantes são **notificados** automaticamente.

---

## US04 — Entregar atividade
**Como** estudante, **desejo** entregar uma atividade antes do prazo, **para** que minha tarefa seja registrada e avaliada.
*Regras relacionadas: RN03, RN07*

**Critérios de aceitação**
- Dado que o prazo **não** expirou, quando envio minha entrega, então ela é registrada com data/hora.
- Dado que o prazo **expirou**, quando tento entregar, então a entrega é **bloqueada** ou marcada como **atrasada**, conforme a política da turma.
- Dado que minha entrega **já foi corrigida**, quando tento editá-la, então a alteração é **impedida**.

---

## US05 — Corrigir e lançar nota
**Como** professor, **desejo** corrigir entregas e lançar notas com feedback, **para** avaliar o desempenho dos estudantes.
*Regras relacionadas: RN04, RN06, RN07*

**Critérios de aceitação**
- Dado que sou o professor responsável, quando lanço uma nota (≤ nota máxima) e um comentário, então a entrega passa a constar como **corrigida**.
- Dado que a nota foi lançada, quando a correção é salva, então o estudante é **notificado**.
- Dado que **não** sou o professor responsável, quando tento corrigir, então a ação é **negada**.

---

## US06 — Consultar notas e feedback
**Como** estudante, **desejo** consultar minhas notas e feedbacks, **para** acompanhar meu desempenho.
*Regras relacionadas: RN08*

**Critérios de aceitação**
- Dado que tenho entregas corrigidas, quando acesso a área de notas, então vejo a nota e o feedback de cada atividade.
- Dado que sou um estudante, quando acesso as notas, então vejo **apenas as minhas** (não as de outros estudantes).

---

## US07 — Receber notificações
**Como** estudante, **desejo** receber notificações de novas atividades e notas, **para** não perder prazos nem novidades.
*Regras relacionadas: RN06*

**Critérios de aceitação**
- Dado que uma atividade foi publicada na minha turma, quando isso ocorre, então recebo uma notificação.
- Dado que uma nota minha foi lançada, quando isso ocorre, então recebo uma notificação.

---

## US08 — Comunicar-se no mural da turma
**Como** professor ou estudante, **desejo** publicar e responder mensagens no mural da turma, **para** centralizar a comunicação.
*Regras relacionadas: RN02, RN08*

**Critérios de aceitação**
- Dado que sou membro da turma, quando publico uma mensagem, então ela fica visível aos demais membros.
- Dado que **não** sou membro da turma, quando tento ver o mural, então o acesso é **negado**.

---

## US09 — Gerenciar usuários e papéis
**Como** administrador, **desejo** cadastrar usuários e atribuir papéis, **para** controlar quem pode fazer o quê na plataforma.
*Regras relacionadas: RN01, RN08*

**Critérios de aceitação**
- Dado que sou administrador, quando cadastro um usuário e defino seu papel (estudante/professor/coordenador), então ele passa a ter as permissões correspondentes.
- Dado que um usuário foi **desativado**, quando ele tenta acessar, então o acesso é **bloqueado**.

---

## US10 — Acompanhar risco de evasão *(Funcionalidade Inovadora)*
**Como** professor, **desejo** visualizar um painel com o nível de engajamento e risco de evasão dos estudantes, **para** intervir precocemente com quem está se desengajando.
*Regras relacionadas: RN06, RN08*

**Critérios de aceitação**
- Dado que há dados de atividade (acessos, entregas, atrasos, notas), quando abro o painel, então vejo um **indicador de risco** por estudante (ex.: baixo / médio / alto).
- Dado um estudante em risco alto, quando o indicador é calculado, então o professor pode receber um **alerta**.
- Dado que sou um estudante, quando tento acessar o painel, então o acesso é **negado** (visível apenas a professor/coordenador).

