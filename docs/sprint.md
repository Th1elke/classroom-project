# Diário de Sprint — AulaConecta

Registro do processo de construção da solução ao longo das duas aulas da avaliação, incluindo divisão de responsabilidades, principais decisões, dificuldades encontradas e soluções adotadas.

---

## Sprint 1 — Concepção da Solução (Aula 1)

**Objetivo:** definir o problema, os stakeholders, as regras de negócio, as user stories, o MVP e organizar backlog/Kanban/repositório.

### Divisão de responsabilidades

| Integrante | Responsabilidade |
|------------|-------------------|
| _Pedro Ribas Meotti_ | Visão do Produto, Stakeholders, README |
| _Murilo Gomes Monteiro Morgado_ | Regras de Negócio, User Stories |
| _Matheus Meggiolaro Bönmann_ | MVP, Backlog |
| _Gustavo Henrique Thielke_ | Configuração do repositório GitHub e do quadro Kanban |

> Substituir pelos nomes reais do grupo. A divisão acima foi a planejada no início do sprint.

### Principais decisões tomadas
- Nome do produto definido como **AulaConecta**, inspirado no Google Classroom.
- Adoção do método **MoSCoW** para priorizar o MVP (Must/Should/Could/Won't), em vez de simplesmente listar "tudo que dá tempo".
- Escolha do **GitHub Projects** como gestor de projetos, por já integrar com as Issues do repositório e facilitar a evidência de colaboração exigida no edital.
- Definição da funcionalidade inovadora já nesta fase — **Painel de Risco de Evasão** — para que ela pudesse influenciar as decisões de MVP e, mais adiante, de arquitetura.

### Dificuldades encontradas
- Dificuldade inicial em **limitar o escopo do MVP**: a primeira versão do backlog tinha funcionalidades demais classificadas como "essenciais".
- Risco de **regras de negócio genéricas demais**, sem conexão clara com requisitos e processos.

### Soluções adotadas
- Revisão do MVP usando o critério "o que é necessário para validar o ciclo completo de uma atividade (criar → publicar → entregar → corrigir → notificar)" — isso cortou itens como mural e gestão de usuários para a fase *Should have*.
- Para cada regra de negócio, exigimos explicitamente pelo menos um impacto rastreável em User Story, processo ou arquitetura (não bastava só descrever a regra) — o que resultou na tabela de rastreabilidade RN03/RN06 documentada em `regras-negocio.md`.

---

## Sprint 2 — Projeto da Solução (Aula 2)

**Objetivo:** modelar o BPMN, o diagrama de casos de uso, definir a arquitetura, o modelo C4 e as decisões arquiteturais.

### Divisão de responsabilidades

| Integrante | Responsabilidade |
|------------|-------------------|
| _Matheus Meggiolaro Bönmann_ | BPMN — processo de Entrega de Atividade |
| _Murilo Gomes Monteiro Morgado_ | UML — Diagrama de Casos de Uso |
| _Gustavo Henrique Thielke_ | Arquitetura + Modelo C4 (Contexto e Containers) |
| _Pedro Ribas Meotti_ | Decisões Arquiteturais (ADRs) e revisão geral de consistência |

### Principais decisões tomadas
- Escolha da arquitetura **híbrida (Camadas + Publish/Subscribe)**, em vez de monolito puro ou microsserviços — justificada em `docs/arquitetura.md` e detalhada no ADR-01.
- Adoção de **RabbitMQ** como tecnologia de broker, deixando Kafka como evolução futura caso o volume de eventos cresça (ADR-01).
- Upload de arquivos das entregas via **URL pré-assinada** (a SPA envia direto ao object storage), para não sobrecarregar a API com tráfego pesado.
- Validação do prazo de entrega (RN03) feita **exclusivamente no servidor**, nunca confiando no relógio do cliente (ADR-05).

### Dificuldades encontradas
- O primeiro rascunho do **BPMN** tratava "entrega fora do prazo" como um único caminho, misturando dois desfechos diferentes (entrega aceita com atraso vs. entrega bloqueada) — o que deixava a notificação ao professor inconsistente em um dos casos.
- O **diagrama de casos de uso** inicial ligava o ator Estudante diretamente ao caso de uso "Notificar participantes", o que contraria a semântica de um caso de uso incluído (`<<include>>`), já que o estudante não inicia essa ação.
- Encontramos **nomes divergentes** para os mesmos elementos entre os documentos de arquitetura e os diagramas C4 (ex.: "Serviço de Notificação" no texto vs. "Provedor de Notificação" nos diagramas), e uma lacuna nos ADRs, que citavam apenas dois dos três eventos do domínio.

### Soluções adotadas
- Reestruturação do BPMN com **dois gateways em sequência**: o primeiro avalia se o prazo é válido (RN03); o segundo, somente no caminho "Não", avalia se a turma aceita atraso. Os dois caminhos que geram entrega (no prazo e atrasada) convergem para o mesmo evento de notificação ao professor; só o caminho de bloqueio segue separado, notificando o estudante.
- Remoção da associação indevida no diagrama de casos de uso e padronização de todos os nomes de caso de uso no formato verbo + objeto (ex.: "Acessar mural da turma", "Monitorar risco de evasão").
- Revisão cruzada de todos os artefatos de arquitetura para unificar nomenclatura entre `arquitetura.md`, `decisoes-arquiteturais.md` e os diagramas C4, e complementação do ADR-03 com o evento `EntregaRegistrada`, que faltava.

---

## Retrospectiva geral

- **O que funcionou bem:** definir a funcionalidade inovadora (Painel de Risco de Evasão) já na Sprint 1 permitiu que ela fosse incorporada à arquitetura sem retrabalho — o serviço apenas reaproveita os eventos já publicados para notificações.
- **O que faria diferente:** revisar a consistência de nomenclatura entre documentos e diagramas **durante** a criação de cada artefato, e não apenas ao final — algumas divergências de nome só foram percebidas na revisão final.
