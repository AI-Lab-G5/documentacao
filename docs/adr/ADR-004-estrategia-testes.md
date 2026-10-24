# ADR-004: Estratégia de testes

## Status

Registro original: Proposto (16/10/2026) - Autor: Tech Lead (Grupo 05).
Revisado em 23/10/2026 conforme [ADR-008](ADR-008-recorte-crud.md).

## Contexto

A proposta depende de confiança nos resultados, isolamento entre empresas e reprodução de um cálculo anterior com seu fator original. Apenas testar o caminho feliz do formulário não cobre duplicidade, correção, cancelamento, arredondamento ou mudanças de fator.

O escopo menor retira a matriz de quatro perfis e o fluxo de aprovação/fechamento, mas preserva desafios de precisão e histórico. A equipe precisa demonstrar os critérios prioritários com ambiente reproduzível, relacionando requisito, aceite, teste, resultado, PR e CI.

## Decisão

Manter três pilares, complementados por smoke de interface:

1. **Unitários e de propriedade:** testar Decimal, conversão kWh/MWh, validações, idempotência e agregações; geração com semente, por Hypothesis ou massa determinística equivalente, para 1.000 cenários incluindo revisões/cancelamentos.
2. **Contrato:** verificar schemas, campos, erros e rotas implementadas contra a OpenAPI-alvo 0.2.0. Lint do documento não prova compatibilidade da aplicação.
3. **Golden tests e integração:** emuladores Auth/Firestore, fatores sintéticos A/B, entradas/revisões conhecidas e 100 recálculos do snapshot original com diferença zero.

Smoke E2E confirma J1-J4 locais no CP2, incluindo carregando/vazio/erro/sucesso. J5 adiciona concordância PDF/CSV do mesmo snapshot até o Demo Day, sem ser declarada executada nas 20 issues atuais.

EP/BVA inclui zero/ausente/negativo, unidade incompatível, duplicidade, base concorrente e limites de paginação. Cobrir mediana, ±50% exatos, >50%, zero/amostra insuficiente e incompletas. Segurança cobre todas as rotas: token ausente/inválido/expirado, papel sem permissão, tenant incorreto, acesso indevido e Firestore direto deny-all. Acesso entre unidades do mesmo tenant é permitido e deve ter caso positivo.

Cada teste precisa apontar ao aceite; relatório registra comando, ambiente, versão, esperado/observado e PR/CI real. Não contar pulos como aprovados nem CI com TODO como execução. Casos metodológicos externos e métricas exploratórias de defensibilidade são identificados separadamente, sem alegar homologação.

Alternativas descartadas:
- **Somente testes manuais ou de UI:** não isolam erros de precisão/versionamento e dificultam repetir casos.
- **Somente mocks de identidade/banco:** não demonstram emissão/validação de tokens, queries e Rules efetivas.
- **Suíte E2E ampla para recursos adiados:** consome manutenção sem atender o recorte. Manter smoke essencial, sem proibir uma ferramenta específica.
- **Executar tudo sem rastreabilidade:** testes verdes sem critério/versão não explicam o que foi comprovado.

## Consequências

- **Fica mais fácil:** Detectar regressões na origem, cálculo e isolamento e reproduzir evidências por outra pessoa.
- **Fica mais difícil:** Preparar massas esperadas independentes, tokens e emuladores no CI; preservar casos antigos quando o modelo mudar.
- **Riscos e mitigação:** Confundir exemplo sintético, lint ou placeholder com resultado real. Separar estados pendente/aprovado/reprovado/não executado, revisar expectativas e vincular execuções.
- **Gatilho de revisão:** Mudança de contrato/modelo, falhas não cobertas ou metas do caderno em risco. Permanecem cobertura ≥80% domínio/≥60% global, Lighthouse ≥90, p95 leitura ≤800 ms/escrita ≤1,5 s e exigências de segurança/rollback nos marcos aplicáveis; mudanças dependem do tutor.
