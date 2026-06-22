# AulaConecta — Sistema de Gestão de Sala de Aula Online

Projeto da disciplina **Modelagem e Projetos em Engenharia de Software** — Profª Fabricia Roos.
Sistema de gestão de turmas, atividades, avaliações e comunicação entre professores e estudantes, semelhante ao Google Classroom.

## 👥 Integrantes
- _Gabriel Ribeiro Rosa_
- _Gustavo Henrique Thielke_
- _Matheus Meggiolaro Bönmann_
- _Murilo Gomes Monteiro Morgado_

## 🔗 Links
- **Gestor de Projetos (Kanban):** _inserir link do GitHub Projects / Trello / Jira_
- **Apresentação final:** `apresentacao/`

## ✨ Funcionalidade Inovadora
**Painel de Risco de Evasão / Alertas de Engajamento** — calcula um indicador de risco por estudante a partir dos eventos de uso (acessos, entregas, atrasos, notas) e alerta o professor para intervir precocemente. Reaproveita os mesmos eventos do mecanismo de notificações (ver `docs/arquitetura.md`).

## 🏗️ Arquitetura (resumo)
Estilo **híbrido**: núcleo em camadas (apresentação, aplicação/negócio, persistência) + **publish/subscribe** (RabbitMQ) para notificações e para o Painel de Risco de Evasão. Upload de arquivos via URL pré-assinada, direto da SPA para o object storage. Detalhes completos em `docs/arquitetura.md` e `docs/decisoes-arquiteturais.md` (5 ADRs).

## 📂 Estrutura do repositório

```
classroom-project/
├── docs/
│   ├── visao-produto.md            # Problema, público-alvo, objetivos, benefícios
│   ├── stakeholders.md             # Envolvidos, interesses e influência
│   ├── regras-negocio.md           # 8 regras + impacto (RN03 e RN06)
│   ├── user-stories.md             # 10 user stories com critérios de aceitação
│   ├── mvp.md                      # Essenciais, futuras e priorização (MoSCoW)
│   ├── backlog-kanban.md           # Backlog e guia do quadro Kanban
│   ├── arquitetura.md              # Estilo arquitetural, justificativa, C4 (texto)
│   ├── decisoes-arquiteturais.md   # ADR-01 a ADR-05
│   └── sprint.md                   # Diário de sprint
├── bpmn/
│   ├── entrega-de-atividade.bpmn   # Modelo editável (bpmn.io)
│   └── entrega-de-atividade.png    # Imagem do processo
├── uml/
│   ├── casos-de-uso.puml           # Script editável (PlantUML)
│   └── casos-de-uso.png            # Diagrama de casos de uso
├── c4/
│   ├── contexto.png                # C4 Nível 1 — Contexto
│   └── containers.png              # C4 Nível 2 — Containers
└── apresentacao/
    └── apresentacao-final.pdf
```

## ✅ Concepção da Solução (Aula 1) — concluído
- [x] Definição do problema (`docs/visao-produto.md`)
- [x] Identificação dos stakeholders (`docs/stakeholders.md`)
- [x] Regras de negócio (`docs/regras-negocio.md`)
- [x] User stories (`docs/user-stories.md`)
- [x] MVP (`docs/mvp.md`)
- [x] Backlog e quadro Kanban (`docs/backlog-kanban.md`)
- [x] Repositório GitHub

## ✅ Projeto da Solução (Aula 2) — concluído
- [x] BPMN — processo de Entrega de Atividade (`bpmn/`)
- [x] Casos de Uso — UML (`uml/`)
- [x] Arquitetura + Modelo C4 — Contexto e Containers (`docs/arquitetura.md`, `c4/`)
- [x] Decisões arquiteturais — 5 ADRs (`docs/decisoes-arquiteturais.md`)
- [x] Diário de sprint (`docs/sprint.md`)
- [x] Apresentação final (`apresentacao/`)

