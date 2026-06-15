# Regras de Negócio — ClassHub


## 1. Listagem das Regras de Negócio

* **RN01 - Restrição de Criação de Turmas:** Apenas usuários autenticados com o perfil "Professor" ou "Administrador" possuem permissão para criar, editar ou excluir turmas no sistema.
* **RN02 - Acesso Exclusivo a Matriculados:** Um estudante só pode acessar os materiais, o mural de avisos e as atividades de uma turma se possuir o status de "Matriculado" nela, mediante a inserção prévia de um código de acesso válido.
* **RN03 - Controle de Prazo de Entregas:** O prazo limite (data e hora) de envio de uma atividade encerra automaticamente a aceitação de novas submissões, a menos que o professor habilite explicitamente a opção "Aceitar entregas com atraso" nas configurações da atividade.
* **RN04 - Sigilo de Avaliações:** As notas atribuídas a uma entrega são estritamente sigilosas. Elas só podem ser visualizadas pelo próprio estudante que realizou a submissão, pelos professores vinculados àquela turma e por coordenadores com privilégios gerenciais.
* **RN05 - Limite Técnico de Anexos:** Arquivos anexados em entregas de atividades (pelos alunos) ou materiais de aula (pelos professores) não podem ultrapassar o limite de 50MB por arquivo.
* **RN06 - Automação do MindFlow (Inovação):** O painel Kanban pessoal do aluno deve priorizar e reordenar as tarefas automaticamente todos os dias, calculando a urgência com base na quantidade de dias restantes para o encerramento do prazo.

---

## 2. Rastreabilidade e Impactos na Solução

Para demonstrar a influência direta das regras de negócio na concepção e modelagem do sistema, detalhamos abaixo os impactos estruturais de duas regras fundamentais:

### Impactos da RN01 (Restrição de Criação de Turmas)
O fato de o sistema proibir alunos de criarem turmas afeta múltiplas camadas da aplicação:
* **Impacto em Requisitos (User Story):** Gera a criação da **US01**, que descreve explicitamente: *"Como professor, desejo criar uma nova turma..."*.
* **Impacto em Modelagem (Casos de Uso):** No Diagrama de Casos de Uso (UML), a ação "Criar Turma" possui uma linha de associação apontando exclusivamente para o ator `Professor` (e `Administrador`), sem ligação com o ator `Estudante`.
* **Impacto em Processos (BPMN):** Ao modelar o fluxo de "Abertura de Turma", o processo obrigatoriamente se inicia dentro de uma raia (*swimlane*) dedicada ao ator Professor, isolando as responsabilidades.
* **Impacto em Arquitetura e Segurança:** Exige a implementação de uma arquitetura com controle de acesso baseado em funções (RBAC). A API no back-end precisa validar se o *token* (JWT) do usuário que faz a requisição possui a *claim* de "professor" antes de gravar a turma no banco de dados.

### Impactos da RN03 (Controle de Prazo de Entregas)
A necessidade de bloquear envios ou sinalizar atrasos altera a lógica de validação de dados e a interface:
* **Impacto em Requisitos (User Story):** A **US04** (Submissão de Entregas) inclui em seus critérios de aceitação que *"Se o prazo limite tiver expirado, o sistema deve bloquear o envio ou marcar visualmente como entregue com atraso"*.
* **Impacto em Processos (BPMN):** No diagrama do processo de "Entrega de Atividade", logo após a ação do aluno de enviar o arquivo, é necessário incluir um **Gateway Exclusivo (Losango com um 'X')** com a pergunta "Prazo Expirado?". Se "Sim", o fluxo diverge para um evento de recusa ou para uma marcação de atraso. Se "Não", segue o fluxo normal.
* **Impacto em Arquitetura (Decisão de Banco de Dados):** O banco de dados precisa registrar obrigatoriamente o *timestamp* (data e hora exatas) no momento exato em que a API recebe o arquivo (*upload*), para que esse valor seja matematicamente comparado com a coluna `data_vencimento` da tabela de Atividades.
