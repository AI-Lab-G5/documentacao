# ADR-004 — Estratégia de testes

## Status
Revisado em 23/10/2026 pelo [ADR-008](ADR-008-recorte-crud.md).

## Decisão
Priorizar testes de negócio/contrato/integração com emuladores e smoke de interface das jornadas. Ligar cada caso ao critério de aceite e registrar comando, ambiente, versão, resultado e CI.

Cobrir Decimal, conversão kWh/MWh, zero/negativos/ausentes, duplicidade/idempotência, revisão-base concorrente, correção/cancelamento e fatores A/B. Provar diferença zero em 100 recálculos do snapshot original e consistência dos agregados em 1.000 cenários com semente. Testar mediana, limite ±50%, zeros/amostra insuficiente e unidades incompletas.

Autorização em todas as rotas com token ausente/inválido/expirado, papel sem permissão, tenant incorreto e acesso direto Firestore deny-all. Mesmo tenant tem acesso a todas as próprias unidades, sem matriz de quatro perfis.

Validar respostas contra OpenAPI-alvo 0.2.0 quando a API for adaptada. Documentação validada não significa testes de contrato da aplicação executados. Teste pulado não conta como aprovado. EP/BVA e relatório no CP2; J5 inclui concordância PDF/CSV do mesmo snapshot até o Demo Day.

## Exigências externas e limites
Preservar metas do caderno nos marcos aplicáveis: domínio ≥80%, global ≥60%, Lighthouse ≥90, p95 leitura ≤800ms/escrita ≤1,5s, segurança, rollback e demais evidências. Não reduzir por decisão unilateral de produto.

Hipóteses comerciais não são provadas por teste de software. Não exigir testes de aprovação/fechamento/auditor, agora adiados. O workflow existente possui TODOs; CI verde em placeholder não comprova execução real. Esta revisão não altera workflow.
