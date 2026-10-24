# Índice de Registos de Decisão de Arquitetura (ADR)

Revisão de 23/10/2026. Cada ADR registra contexto, decisão, alternativas, consequências, riscos e gatilho de revisão. O [ADR-008](ADR-008-recorte-crud.md) mantém as cinco jornadas e registra o corte de perfis/fluxos. Decisão documentada não comprova implementação.

| Número | Título | Status | Resumo (Para o PO) |
| :--- | :--- | :--- | :--- |
| [ADR-001](ADR-001-stack-principal.md) | Stack principal | Mantida/revisada | Reaproveitar FastAPI, React e Firebase evita migração durante o ciclo e concentra esforço nas jornadas. |
| [ADR-002](ADR-002-autenticacao.md) | Autenticação e autorização | Revisada; implementação a verificar | Login individual preserva autoria; um perfil reduz fluxos sem abrir dados de outra empresa. |
| [ADR-003](ADR-003-persistencias.md) | Persistência | Revisada; implementação a verificar | Revisões e snapshots preservam o anterior; strings decimais evitam perder precisão no armazenamento. |
| [ADR-004](ADR-004-estrategia-testes.md) | Estratégia de testes | Revisada; execução a verificar | Testes ligam critérios a resultados reproduzíveis, sem confundir documento ou CI TODO com software validado. |
| [ADR-005](ADR-005-fatores-imutaveis.md) | Fatores e metodologia | Revisada; implementação a verificar | Manter A depois de B permite explicar o cálculo antigo, sem tela de publicação ou hashes obrigatórios. |
| [ADR-006](ADR-006-idempotencia.md) | Idempotência e identificação | Revisada; implementação a verificar | Chave de negócio/operação e revisão-base evitam duplicação por rede e perda de correções concorrentes. |
| [ADR-007](ADR-007-arimetica-decimal.md) | Decimal e arredondamento | Mantida/alinhada | Uma regra de precisão e apresentação evita divergência entre unidades, rede e exportação. |
| [ADR-008](ADR-008-recorte-crud.md) | Recorte CRUD com requisitos originais | Produto aprovado; implementação pendente | Cortar perfis e fechamento preservando versões, rede e relatórios reduz escopo sem descaracterizar a proposta. |

Registro original dos ADRs 001-007 em 16/10/2026, atribuído ao Tech Lead (Grupo 05); texto anterior preservado no histórico Git. Mudanças nos requisitos técnicos dos checkpoints dependem do tutor. As decisões atuais orientam o alvo, não modificam automaticamente a API de CarbonTrace.
