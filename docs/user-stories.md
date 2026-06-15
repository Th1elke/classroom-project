# User Stories — ClassHub

Este documento detalha as histórias de utilizador (User Stories) que definem os requisitos funcionais do sistema, priorizadas para o MVP e para a funcionalidade inovadora.

## US01: Criação de Turmas
* **Descrição:** Como professor, desejo criar uma nova turma definindo um nome e uma disciplina, para que o sistema gere um código de acesso único.
* **Critérios de Aceitação:**
  * O sistema deve validar o preenchimento dos campos obrigatórios (Nome e Disciplina).
  * O sistema deve gerar e exibir um código alfanumérico único de 7 caracteres após a gravação.
  * A nova turma deve ficar imediatamente visível no painel principal do professor.

## US02: Ingresso de Estudantes
* **Descrição:** Como estudante, desejo inserir um código de acesso no sistema para me matricular numa turma específica.
* **Critérios de Aceitação:**
  * O sistema deve apresentar um campo para inserção do código no painel do aluno.
  * O sistema deve validar se o código existe e está ativo.
  * Em caso de sucesso, o aluno deve ser redirecionado para o mural da turma e adicionado à lista de participantes.

## US03: Publicação de Atividades
* **Descrição:** Como professor, desejo publicar uma atividade no mural definindo título, instruções e data/hora limite, para avaliar os estudantes.
* **Critérios de Aceitação:**
  * A interface deve permitir a configuração de um prazo limite de entrega.
  * Ao publicar, a atividade deve aparecer no mural de todos os alunos matriculados.
  * O sistema deve disparar um evento assíncrono para notificar os alunos (preparação para envio de e-mails).

## US04: Submissão de Entregas (Upload)
* **Descrição:** Como estudante, desejo fazer o upload de um ficheiro numa atividade para registar a minha entrega antes do prazo limite.
* **Critérios de Aceitação:**
  * O sistema deve aceitar ficheiros (PDF, DOCX, ZIP) com um limite técnico de 50MB.
  * O status da atividade no painel do aluno deve ser alterado para "Entregue".
  * Se o prazo limite tiver expirado, o sistema deve bloquear o envio ou marcar visualmente como "Entregue com atraso", dependendo da configuração do professor.

## US05: Avaliação e Lançamento de Notas
* **Descrição:** Como professor, desejo aceder à entrega de um aluno e atribuir uma nota numérica, para compor a sua avaliação final.
* **Critérios de Aceitação:**
  * O professor deve conseguir visualizar o ficheiro submetido pelo aluno.
  * A nota deve ser gravada no banco de dados e vinculada apenas àquele aluno específico (garantia de sigilo).
  * O aluno deve receber uma notificação no sistema indicando que a sua atividade foi avaliada.

## US06: Dashboard de Pendências
* **Descrição:** Como estudante, desejo visualizar um quadro consolidado com as minhas atividades pendentes, ordenadas pelo prazo mais próximo, para organizar a minha rotina de estudos.
* **Critérios de Aceitação:**
  * Atividades que já possuem submissão registada não devem aparecer na lista de pendências.
  * Atividades cujo prazo expira em menos de 24 horas devem receber um destaque visual (ex: cor vermelha ou ícone de alerta).

## US07: Módulo MindFlow (Funcionalidade Inovadora)
* **Descrição:** Como estudante, desejo que o sistema organize as minhas tarefas automaticamente num painel Kanban pessoal baseado em pesos de prioridade (proximidade do prazo e complexidade), para reduzir a minha procrastinação.
* **Critérios de Aceitação:**
  * O sistema deve calcular o peso de cada tarefa ativa e reordenar a coluna "A Fazer" diariamente.
  * O aluno deve receber "badges" (conquistas) visuais ao entregar múltiplas atividades de forma antecipada ou no prazo correto.
  * Coordenadores devem ter acesso a um relatório que sinaliza alunos com alto índice de tarefas acumuladas no painel.
