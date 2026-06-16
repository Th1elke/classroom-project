# Documento de Arquitetura de Software — ClassHub

## 1. Escolha e Justificativa da Arquitetura

A solução adota uma **Arquitetura Híbrida**, combinando um padrão **Em Camadas (Monolito Modular)** para o núcleo da aplicação com um padrão **Publish-Subscribe (Event-Driven)** para processamento assíncrono.

**Justificativa:**
No contexto de um sistema escolar, as entidades (Estudantes, Turmas, Avaliações) possuem forte relacionamento relacional. Um monolito em camadas isola bem a regra de negócios sem o alto custo inicial e a complexidade de rede de microsserviços. No entanto, para não travar o servidor durante o envio massivo de notificações ou uploads pesados, acoplamos o modelo *Publish-Subscribe* para jogar essas tarefas em segundo plano.

---

## 2. Decisões Arquiteturais (ADRs)

Conforme as necessidades do sistema, tomamos as seguintes decisões técnicas:

### ADR 01: Escolha do Mecanismo de Autenticação (RBAC e JWT)
* **Contexto:** O sistema precisa garantir o sigilo de notas (RN04) e restringir a criação de turmas apenas a professores (RN01).
* **Decisão:** Implementação de tokens JWT (JSON Web Tokens) com controle de acesso baseado em funções (Role-Based Access Control). O perfil do usuário viaja criptografado no token.
* **Justificativa:** Essa abordagem é *stateless* (não sobrecarrega a memória do servidor com sessões) e protege a API contra vulnerabilidades de IDOR ao validar a posse dos dados a cada requisição.

### ADR 02: Estratégia de Upload de Arquivos
* **Contexto:** Os alunos enviarão anexos de até 50MB (RN05). Processar isso na API principal causaria gargalos.
* **Decisão:** Utilização do padrão *Pre-signed URLs* com armazenamento em Object Storage (S3).
* **Justificativa:** O back-end apenas autoriza a transferência; o upload pesado é feito diretamente do navegador do aluno para a nuvem, poupando os recursos de processamento da nossa API.

### ADR 03: Estratégia de Notificações Assíncronas
* **Contexto:** Publicar uma atividade para 100 alunos exige o disparo de 100 e-mails.
* **Decisão:** O back-end publicará um evento `NovaAtividade` em um Message Broker (Redis). Um *Worker* separado consumirá essa fila para disparar os e-mails.
* **Justificativa:** Garante que a requisição do professor seja respondida em milissegundos, enquanto o processamento demorado ocorre de forma invisível em segundo plano.

---

## 3. Mapeamento do Modelo C4

O detalhamento visual desta arquitetura encontra-se na pasta `c4/` do repositório:
* **Nível 1 (Contexto):** Demonstra a interação do ClassHub com os atores principais (Estudantes, Professores, Coordenadores e Admins) e sistemas externos (Serviço de E-mail).
* **Nível 2 (Containers):** Detalha a subdivisão técnica em: *Single-Page Application* (Front-end), *API Application* (Back-end em camadas), *Banco de Dados Relacional* e *Message Broker/Worker*.

---

## 4. Impacto da Funcionalidade Inovadora (Módulo MindFlow)

A funcionalidade gamificada "MindFlow" (organização automática de tarefas prioritárias) causa as seguintes adaptações arquiteturais:
* **Camada de Processamento:** Exige a criação de *cron jobs* (rotinas agendadas) no Worker assíncrono para recalcular os pesos de prioridade das tarefas toda madrugada.
* **Camada de Dados:** Necessidade de cache em memória (Redis) para entregar o dashboard do aluno rapidamente, evitando varrer o banco de dados transacional completo a cada login.
