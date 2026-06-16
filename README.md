# ClassHub — Sistema de Gestão de Sala de Aula Online

O **ClassHub** é uma plataforma centralizada concebida para otimizar a gestão de turmas, a distribuição de atividades e a avaliação académica de forma integrada. O sistema mitiga os problemas de desorganização e perda de prazos, introduzindo o módulo inovador **MindFlow** para a gestão automatizada de tarefas com foco na redução da procrastinação dos estudantes.

Este repositório armazena toda a documentação de engenharia de requisitos, modelagem de processos, diagramas estruturais e decisões arquiteturais do projeto.

---

## 👥 Equipa de Desenvolvimento
* **Membro A:** [Preencher com o seu Nome]
* **Membro B:** [Preencher com Nome do Colega]
* **Membro C:** [Preencher com Nome do Colega]

---

## 📂 Guia de Navegação do Repositório

O projeto está estruturado de forma a garantir a total transparência e rastreabilidade dos artefatos produzidos durante as fases de Concepção e Projeto da Solução:

### 📄 1. Documentação de Requisitos e Planeamento (`docs/`)
* [**Visão do Produto**](docs/visao-produto.md) — O problema a ser resolvido, os objetivos do sistema e o público-alvo.
* [**Mapeamento de Stakeholders**](docs/stakeholders.md) — Identificação dos atores envolvidos, os seus papéis e interesses.
* [**User Stories**](docs/user-stories.md) — Histórias de utilizador completas com os seus respetivos critérios de aceitação.
* [**Regras de Negócio**](docs/regras-negocio.md) — Restrições de domínio e a análise de impacto no sistema.
* [**Definição do MVP**](docs/mvp.md) — Escopo essencial da primeira versão e a justificativa da priorização de recursos.
* [**Diário de Sprint**](docs/sprint.md) — Registo de colaboração, divisão de tarefas, dificuldades e soluções adotadas.
* [**Documento de Arquitetura**](docs/arquitetura.md) — Escolha do estilo arquitetural e os Registos de Decisões Arquiteturais (ADRs).

### 📊 2. Modelagem Visual e Diagramas
* [**Pasta `bpmn/`**](bpmn/) — Modelagem dos processos de negócio (Fluxo de publicação e entrega de atividades).
* [**Pasta `uml/`**](uml/) — Diagramas de Casos de Uso que mapeiam as interações dos utilizadores.
* [**Pasta `c4/`**](c4/) — Modelagem da arquitetura de software utilizando o Modelo C4 (Nível 1: Contexto e Nível 2: Containers).

---

## 🛠️ Resumo da Visão Tecnológica
A arquitetura do ClassHub foi desenhada seguindo um modelo **Híbrido**:
* **Backend e Core:** Estruturado em camadas (Monolito Modular) com persistência em banco de dados relacional.
* **Processamento Assíncrono:** Utilização de um Message Broker (Redis) acoplado a Workers independentes para gerir o motor de prioridades do módulo MindFlow e disparos de notificações por e-mail sem comprometer o tempo de resposta da API principal.
* **Segurança:** Controle de acessos baseado em perfis (RBAC) validado de forma assíncrona através de tokens JWT.
