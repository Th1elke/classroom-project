# Arquitetura de Software — AulaConecta

## 1. Estilo arquitetural escolhido: **Híbrido (Camadas + Publish/Subscribe)**

O núcleo do sistema adota uma **arquitetura em camadas** (apresentação, aplicação/negócio e persistência), complementada por um **mecanismo orientado a eventos (publish/subscribe)** para tudo que deve acontecer de forma **assíncrona e desacoplada** (notificações e analytics de risco de evasão).

### Camadas do núcleo
| Camada | Responsabilidade |
|--------|------------------|
| **Apresentação** | Aplicação Web (SPA) — interface de professores, estudantes, coordenadores e administradores. |
| **Aplicação / Negócio** | API/Backend — orquestra os casos de uso, aplica as **regras de negócio** (RN01–RN08), autenticação e autorização. |
| **Persistência** | Acesso ao banco de dados relacional e ao armazenamento de arquivos. |

### Componentes orientados a eventos
- **Message Broker (pub/sub):** recebe eventos publicados pelo backend (ex.: `AtividadePublicada`, `NotaLancada`, `EntregaRegistrada`).
- **Serviço de Notificações:** consome eventos e envia e-mail/push (atende **RN06**).
- **Serviço de Risco de Evasão (Inovação):** consome os mesmos eventos de engajamento e calcula o indicador de risco por estudante.

## 2. Justificativa da escolha

A decisão considerou os **requisitos** e o **contexto** do sistema:

1. **Camadas para o núcleo (simplicidade e coesão):** o coração do produto é um CRUD rico (turmas, atividades, entregas, notas) com regras de negócio claras. Uma arquitetura em camadas é simples de desenvolver, testar e manter por uma equipe pequena — não há complexidade que justifique **microserviços** completos (que trariam sobrecarga operacional desnecessária para a escala do MVP).
2. **Publish/Subscribe para eventos (desacoplamento):** a **RN06** exige notificações automáticas, e a publicação/correção de atividades **não deve travar** esperando o envio. Um broker de mensagens permite que essas operações apenas **publiquem um evento** e sigam adiante, enquanto consumidores processam em segundo plano. Isso melhora desempenho percebido e resiliência (se o envio falhar, há reprocessamento).
3. **Reaproveitamento para a inovação:** o **Serviço de Risco de Evasão** consome os **mesmos eventos**, sem acoplar-se ao núcleo. A funcionalidade inovadora "encaixa" na arquitetura sem reescrever o backend.
4. **Por que não puramente em camadas?** Notificações e analytics síncronos sobrecarregariam o backend e acoplariam responsabilidades distintas. Por que não microserviços? O custo de rede, deploy e observabilidade não se justifica nesta fase. A **abordagem híbrida** captura o melhor dos dois mundos.

## 3. Modelo C4

### Nível 1 — Contexto (`c4/contexto.png`)
Mostra o sistema **AulaConecta** no centro, os quatro tipos de usuário (Estudante, Professor, Coordenador, Administrador) e os sistemas externos: **Provedor de Notificação** (e-mail/push) e **Armazenamento de Arquivos** (object storage).

### Nível 2 — Containers (`c4/containers.png`)
Detalha os containers internos:
- **Aplicação Web (SPA)** — React, interface do usuário (HTTPS); **faz o upload/download dos arquivos das entregas direto no object storage**, usando URL pré-assinada.
- **API / Backend (em camadas)** — aplica regras, autenticação e casos de uso; expõe API JSON; **emite as URLs pré-assinadas** que autorizam o envio dos arquivos.
- **Banco de Dados (PostgreSQL)** — dados relacionais (usuários, turmas, atividades, entregas, notas) e a referência (URL/chave) de cada arquivo.
- **Message Broker (pub/sub)** — distribui eventos do domínio.
- **Serviço de Notificações (worker)** — consome eventos e aciona o provedor de e-mail/push.
- **Serviço de Risco de Evasão (worker)** — consome eventos de engajamento e gera o indicador de risco (inovação).
- **Provedor de Notificação (externo)** — serviço de e-mail/push acionado pelo Serviço de Notificações.
- **Armazenamento de Arquivos (externo)** — object storage; guarda os arquivos das entregas.

## 4. Requisitos não funcionais atendidos

| RNF | Como a arquitetura atende |
|-----|---------------------------|
| **Segurança** | Autenticação e autorização por papel concentradas na camada de aplicação (RN01, RN08); arquivos acessados por URLs pré-assinadas e temporárias. |
| **Desempenho / desacoplamento** | Operações principais publicam eventos e não esperam tarefas secundárias (RN06); o upload pesado vai do navegador direto ao storage, sem passar pela API. |
| **Confiabilidade** | Validação de prazo no servidor (fonte de tempo única) garante consistência da RN03. |
| **Escalabilidade** | Workers (notificação, risco) escalam de forma independente do núcleo. |
| **Manutenibilidade** | Separação clara em camadas e por eventos facilita evolução e testes. |

> As decisões arquiteturais detalhadas (com contexto, alternativas e consequências) estão em [`decisoes-arquiteturais.md`](./decisoes-arquiteturais.md).
