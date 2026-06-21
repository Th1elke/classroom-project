# MVP — AulaConecta

Definição do **Produto Mínimo Viável**: o menor conjunto de funcionalidades que entrega valor real e permite validar a solução com usuários reais. Usamos a priorização **MoSCoW** (Must / Should / Could / Won't).

## 1. Funcionalidades essenciais (MVP — *Must have*)

Cobrem o **ciclo central de valor** da plataforma: *criar turma → matricular → publicar atividade → entregar → corrigir → consultar nota*.

| Funcionalidade | User Story | Por que é essencial |
|----------------|------------|---------------------|
| Autenticação e controle de acesso por papel | (base de todas) | Sem login e papéis, nenhuma regra de permissão funciona (RN01, RN08). |
| Criar turma com código de convite | US01 | Ponto de partida do uso. |
| Matricular-se em turma | US02 | Sem matrícula, o estudante não acessa nada. |
| Publicar atividade (prazo + nota máxima) | US03 | Coração do uso pelo professor. |
| Entregar atividade (com regra de prazo) | US04 | Coração do uso pelo estudante. |
| Corrigir e lançar nota com feedback | US05 | Fecha o ciclo de avaliação. |
| Consultar notas e feedback | US06 | Entrega de valor ao estudante. |
| Notificações de atividade e nota | US07 | Diferencial mínimo de usabilidade; reduz perda de prazos. |

## 2. Funcionalidades futuras

### *Should have* (próxima iteração)
| Funcionalidade | User Story | Justificativa |
|----------------|------------|---------------|
| Mural de comunicação da turma | US08 | Importante, mas a comunicação pode começar pelas notificações no MVP. |
| Gestão de usuários e papéis pelo admin | US09 | No MVP, cadastros podem ser feitos de forma simplificada; o painel completo vem depois. |

### *Could have* (desejável)
| Funcionalidade | User Story | Justificativa |
|----------------|------------|---------------|
| **Painel de Risco de Evasão** *(Inovação)* | US10 | Alto valor diferencial, mas depende de um histórico de eventos já acumulado para gerar indicadores confiáveis. Ideal como **MVP+1**. |

### *Won't have (por enquanto)*
- Videoconferência integrada;
- Aplicativo mobile nativo;
- Gamificação (pontos, medalhas);
- Detecção automática de plágio;
- Integração com sistemas acadêmicos externos.

## 3. Justificativa da priorização

1. **Foco no fluxo principal:** o MVP precisa permitir que um professor crie uma turma e que um estudante a use de ponta a ponta. Por isso US01→US06 são *Must*.
2. **Notificações no MVP (US07):** apesar de parecerem "extra", são baratas de implementar sobre o mesmo mecanismo de eventos e atacam diretamente a dor de **perda de prazos**, aumentando muito a percepção de valor.
3. **Inovação como MVP+1 (US10):** o Painel de Risco depende de **dados de uso acumulados** (entregas, atrasos, acessos). Lançá-lo no MVP daria resultados pobres por falta de histórico. Adiá-lo é uma decisão técnica consciente — e a arquitetura orientada a eventos do MVP **já o prepara**.
4. **Comunicação e administração (US08, US09)** agregam valor, mas não são pré-requisito para validar a hipótese central do produto, por isso ficam para iterações seguintes.

> **Resumo:** o MVP valida a hipótese "*professores e estudantes conseguem gerir o ciclo completo de atividades em um único ambiente, sem perder prazos*", com o menor esforço possível e já deixando a fundação técnica pronta para a funcionalidade inovadora.
