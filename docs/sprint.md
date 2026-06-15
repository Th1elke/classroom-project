# Diário de Sprint — ClassHub

## 1. Divisão das Responsabilidades
Para garantir a participação ativa de todos, o trabalho foi gerido através do GitHub Projects (Kanban), com as seguintes atribuições principais durante a concepção:
* **Membro A (Preencher Nome):** Responsável pelo setup inicial do repositório estruturado, modelagem do Modelo C4 de Arquitetura e redação das Decisões Arquiteturais (ADRs).
* **Membro B (Preencher Nome):** Responsável pelo levantamento de Requisitos, redação da Visão do Produto, mapeamento de Stakeholders e elaboração das User Stories.
* **Membro C (Preencher Nome):** Responsável pela Modelagem de Processos (BPMN), Diagrama de Casos de Uso (UML) e definição das Regras de Negócio e MVP.
* *(Nota: Todos os membros participaram das revisões dos artefatos uns dos outros antes dos commits finais na branch principal).*

## 2. Principais Decisões Tomadas
* **Gestor de Projetos:** Optamos por utilizar o GitHub Projects em vez do Jira ou Trello, pois permite centralizar o código, a documentação em Markdown e as *Issues* (tarefas) no mesmo ecossistema, facilitando a comprovação de colaboração para a banca.
* **Abordagem do MVP:** Decidimos focar exclusivamente no ciclo de "criar turma -> passar tarefa -> receber arquivo -> avaliar", deixando fóruns e chats de fora da primeira versão para garantir viabilidade técnica.
* **Arquitetura Híbrida:** Definimos o uso de um Monolito Modular acoplado a um sistema de mensageria (Redis Pub/Sub) para suportar o Módulo MindFlow (gamificação) sem travar a API principal.

## 3. Dificuldades Encontradas
* **Estruturação do Repositório:** Inicialmente, enfrentamos dificuldades para espelhar a estrutura exata de pastas vazias (`bpmn/`, `c4/`, `uml/`) exigida pela avaliação, visto que o Git nativamente não rastreia diretórios sem arquivos.
* **Rastreabilidade de Regras de Negócio:** Houve dificuldade inicial em conectar uma Regra de Negócio abstrata diretamente com uma decisão de Arquitetura e Modelagem sem parecer algo forçado.
* **Definição da Funcionalidade Inovadora:** O desafio de pensar em algo que fosse criativo para o ambiente acadêmico, mas que ao mesmo tempo mantivesse viabilidade técnica real de implementação.

## 4. Soluções Adotadas
* **Solução para o Repositório:** Resolvemos o problema das pastas criando arquivos de marcação (ex: salvando os arquivos de texto primeiro ou usando `.gitkeep` temporário) e realizando os commits via interface web do GitHub ou terminal para forçar a criação da árvore de diretórios.
* **Solução para Rastreabilidade:** Mapeamos especificamente a regra de "Sigilo de Notas" e a conectamos com o uso de tokens JWT (Stateless) na arquitetura, garantindo a proteção contra acesso indevido por IDOR.
* **Solução para a Inovação:** Desenhamos o Módulo "MindFlow", que reaproveita dados já existentes (prazos e submissões) para gerar um Kanban gamificado para os alunos, exigindo apenas lógicas de ordenação (*workers* assíncronos) no back-end.
