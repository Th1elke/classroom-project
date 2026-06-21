# Visão do Produto — AulaConecta

> Sistema de Gestão de Sala de Aula Online (semelhante ao Google Classroom)
> Disciplina: Modelagem e Projetos em Engenharia de Software

## 1. Problema a ser resolvido

Instituições de ensino enfrentam dificuldade para **centralizar a gestão de turmas, atividades, avaliações e comunicação** entre professores e estudantes. Hoje esse fluxo costuma estar espalhado em e-mails, grupos de mensagem, planilhas e arquivos soltos, o que gera:

- perda de prazos e entregas extraviadas;
- falta de um histórico único de notas e feedbacks;
- comunicação desorganizada entre professor e turma;
- dificuldade do professor em **enxergar quem está se desengajando** antes que seja tarde.

A instituição deseja uma **plataforma digital única** que apoie todo o ciclo de ensino: criação de turmas, matrícula, publicação de atividades, entrega, correção, lançamento de notas e comunicação.

## 2. Público-alvo

- **Estudantes** — acompanham turmas, recebem atividades, entregam tarefas e consultam notas/feedbacks.
- **Professores** — criam turmas e atividades, corrigem entregas, lançam notas e se comunicam com a turma.
- **Coordenadores** — supervisionam turmas e professores, acompanham desempenho geral.
- **Administradores** — gerenciam usuários, permissões e a configuração da instituição.

## 3. Objetivos da solução

1. Centralizar em um único ambiente o ciclo completo de uma atividade: **criar → publicar → entregar → corrigir → avaliar**.
2. Garantir **controle de acesso por papel** (professor, estudante, coordenador, administrador).
3. Organizar a **comunicação** entre professor e turma de forma rastreável.
4. Oferecer **notificações automáticas** de eventos relevantes (nova atividade, nota lançada, prazo próximo).
5. Dar ao professor **visibilidade sobre o engajamento** dos estudantes para apoiar a permanência.

## 4. Benefícios esperados

| Para quem | Benefício |
|-----------|-----------|
| Estudante | Tudo em um só lugar: atividades, prazos, notas e feedback, com lembretes automáticos. |
| Professor | Menos trabalho operacional; correção e lançamento de notas organizados; visão de quem precisa de atenção. |
| Coordenador | Acompanhamento do andamento das turmas e do desempenho dos estudantes. |
| Instituição | Padronização do processo pedagógico, histórico confiável e melhoria nos índices de permanência. |

## 5. Funcionalidade Inovadora (resumo)

**Painel de Risco de Evasão / Alertas de Engajamento.** A partir de eventos já gerados na plataforma (logins, entregas no prazo, atrasos, notas baixas, ausência de acesso), o sistema calcula um **indicador de risco** por estudante e alerta o professor sobre quem está se desengajando, permitindo intervenção precoce. Detalhada nos artefatos da fase de projeto; mencionada aqui porque influencia requisitos e arquitetura (necessidade de processamento de eventos).
