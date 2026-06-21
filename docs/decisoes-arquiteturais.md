# Decisões Arquiteturais (ADRs) — AulaConecta

Registro das principais decisões arquiteturais no formato **ADR** (Architecture Decision Record): contexto, decisão, alternativas consideradas e consequências. São apresentadas **5 decisões** (o mínimo exigido é 3).

---

## ADR-01 — Estilo arquitetural híbrido (Camadas + Pub/Sub)

**Contexto.** O núcleo é um CRUD com regras de negócio (RN01–RN08); ao mesmo tempo, há necessidade de processamento assíncrono (notificações — RN06) e de analytics (risco de evasão — inovação).

**Decisão.** Adotar **arquitetura em camadas** para o núcleo e **publish/subscribe** (message broker) para fluxos assíncronos.

**Alternativas consideradas.**
- *Monolito puramente em camadas:* simples, mas acoplaria notificações/analytics ao backend e degradaria desempenho.
- *Microserviços completos:* flexível, mas com custo operacional (deploy, rede, observabilidade) injustificável para a escala do MVP.

**Consequências.**
- (+) Núcleo simples de manter; eventos desacoplados; inovação se encaixa sem reescrita.
- (−) Introduz um broker como nova peça de infraestrutura a operar e monitorar.

---

## ADR-02 — Autenticação por token (JWT) com autorização por papel (RBAC)

**Contexto.** As regras **RN01** (só professor cria turma/atividade) e **RN08** (sigilo de dados/notas) exigem controle de acesso confiável por tipo de usuário.

**Decisão.** Usar **autenticação baseada em token (JWT)** e **autorização por papel (RBAC)** — estudante, professor, coordenador, administrador — verificada na camada de aplicação.

**Alternativas consideradas.**
- *Sessão em servidor:* válida, mas menos escalável horizontalmente e mais acoplada ao estado do servidor.
- *Permissões por recurso (ACL granular):* mais flexível, porém complexa demais para o MVP.

**Consequências.**
- (+) Stateless, escalável, e papéis cobrem as regras de negócio atuais.
- (−) Necessário cuidar de expiração/renovação de token e revogação.

---

## ADR-03 — Notificações assíncronas orientadas a eventos

**Contexto.** A **RN06** exige notificar estudantes em novas atividades e notas, sem travar a publicação/correção.

**Decisão.** O backend **publica eventos** (`AtividadePublicada`, `NotaLancada`) no broker; um **Serviço de Notificações** os consome e envia e-mail/push.

**Alternativas consideradas.**
- *Envio síncrono dentro da requisição:* mais simples, porém lento e frágil (falha no provedor derruba a operação principal).

**Consequências.**
- (+) Operações principais rápidas e resilientes; reprocessamento em caso de falha.
- (−) Notificação tem latência eventual (não é instantânea) e exige consumidor sempre ativo.

---

## ADR-04 — Banco relacional + Object Storage para arquivos

**Contexto.** Entregas e notas exigem **consistência transacional** (RN03 prazo, RN04 nota, RN07 não editar após correção); entregas incluem **arquivos** potencialmente grandes.

**Decisão.** Usar um **banco relacional (PostgreSQL)** para dados estruturados e um **object storage** externo para os arquivos das entregas (guardando no banco apenas a referência/URL).

**Alternativas consideradas.**
- *Guardar arquivos como BLOB no banco:* simplifica, mas incha o banco e prejudica desempenho/backup.
- *Banco NoSQL:* bom para escala, mas perde garantias transacionais importantes para notas/prazos.

**Consequências.**
- (+) Transações confiáveis para notas/entregas; arquivos escalam e ficam baratos no storage.
- (−) Duas tecnologias de persistência para integrar e manter consistentes.

---

## ADR-05 — Validação de prazo no servidor (fonte de tempo única)

**Contexto.** A **RN03** determina que entregas só sejam aceitas até o prazo. O relógio do cliente não é confiável (pode ser adulterado ou estar dessincronizado).

**Decisão.** A verificação `data_atual ≤ data_limite` é feita **exclusivamente no backend**, usando o horário do servidor como referência única; jobs agendados marcam entregas atrasadas ao fechar o prazo.

**Alternativas consideradas.**
- *Validar no front-end:* boa para UX, mas insegura como única barreira.

**Consequências.**
- (+) Regra de prazo íntegra e auditável; impossível burlar pelo cliente.
- (−) Exige fonte de tempo confiável e tratamento de fuso horário.

---

### Rastreabilidade das decisões

| ADR | Regras/Requisitos que motivam |
|-----|-------------------------------|
| ADR-01 | RN06 + necessidade da inovação (risco de evasão) |
| ADR-02 | RN01, RN08 (Segurança) |
| ADR-03 | RN06 (Desempenho/desacoplamento) |
| ADR-04 | RN03, RN04, RN07 (Consistência) |
| ADR-05 | RN03 (Confiabilidade temporal) |
