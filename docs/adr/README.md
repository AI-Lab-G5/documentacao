# Decisões arquiteturais — CarbonTrace

Revisão de 23/10/2026. [ADR-008](ADR-008-recorte-crud.md) registra o corte aprovado e as partes substituídas. Decisão documentada não comprova implementação, deploy ou homologação.

| ADR | Decisão vigente | Estado |
| --- | --- | --- |
| [001](ADR-001-stack-principal.md) | Manter stack GCP/Firebase e base existente | Mantida/revisada; execução a verificar |
| [002](ADR-002-autenticacao.md) | Um perfil funcionario, isolamento por tenant | Revisada; implementação pendente |
| [003](ADR-003-persistencias.md) | Revisões/cancelamentos e snapshots; sem Storage obrigatório | Revisada; implementação pendente |
| [004](ADR-004-estrategia-testes.md) | Contratos, histórico, precisão, segurança e evidências | Revisada; execução pendente |
| [005](ADR-005-fatores-imutaveis.md) | Fatores A/B imutáveis sem publicação pela aplicação | Revisada; implementação pendente |
| [006](ADR-006-idempotencia.md) | Chave de negócio/operação e revisão-base; sem SHA-256 | Revisada; implementação pendente |
| [007](ADR-007-arimetica-decimal.md) | Decimal, strings e ROUND_HALF_EVEN | Mantida/alinhada; implementação a verificar |
| [008](ADR-008-recorte-crud.md) | CRUD + cinco jornadas com operação simplificada | Produto aprovado em 23/10/2026; implementação pendente |

Os textos anteriores permanecem no histórico Git. Mudanças de requisitos técnicos do caderno dependem de validação com o tutor. Alterações em contrato planejado não modificam automaticamente a API do repositório CarbonTrace.
