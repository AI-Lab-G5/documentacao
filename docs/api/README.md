# API CarbonTrace: contrato-alvo 0.2.0

[OpenAPI](openapi.yaml) · [ADR-008](../adr/ADR-008-recorte-crud.md).
**Especificação planejada, não API já implementada.** O repositório CarbonTrace mantém seu código e contrato gerado intactos nesta revisão. Seus endpoints atuais em português e autenticação precisam de adaptação conforme issues; não usar o lint deste arquivo como prova de integração.

## Identidade e jornadas

Prefixo /v1. Token Bearer verificado pela API, claims tenant_id e papel=funcionario. Tenant nunca é escolhido em path/query/body. /me expõe tenantId/role do chamador. Perfil único acessa todas as unidades da própria empresa; sem quatro papéis, aprovação ou fechamento.

| Jornada | Operações planejadas |
| --- | --- |
| Transversal | GET /health público; GET /me autenticado |
| J1 | GET/POST /facilities; GET/PATCH /facilities/{facilityId}; GET/POST fontes; PATCH fonte para desativar; GET catálogo |
| J2 | POST/GET registros; GET registro e revisões; POST correct/cancel |
| J3 | GET versões de fatores; POST/GET cálculos; GET cálculo; POST recalculate do snapshot |
| J4 | GET dashboard/summary e dashboard/outliers por calculationId |
| J5 | POST /reports com calculationId e formato, resposta binária síncrona |

Catálogo fixo electricity/diesel/gasoline/lpg: kWh/MWh, L, L, kg. Eletricidade Escopo 2, demais Escopo 1. Fonte pode ser desativada, sem apagar origem. Não há endpoints de publicar fator, aprovar, fechar, auditoria separada ou jobs de relatório.

## Escrita, revisões e cálculo

Escritas usam Idempotency-Key (janela 24h); mesma chave/payload retorna anterior sem nova revisão; payload diferente →409. Duplicidade unidade/fonte/competência também →409. Correção/cancelamento exigem baseRevision e motivo; base desatualizada →409. Consulta da idempotência precede validação da base para permitir retry.

Correção altera quantidade/unidade compatível, sem trocar a identificação. Cancelamento cria revisão cancelled; não há DELETE físico ou reativação. Chave permanece reservada; UI informa a limitação antes de confirmar.

POST /calculations cria snapshot do corrente: entradas, configuração esperada, conversões, fatores/metodologia e resultados. Publicação A/B é procedimento da equipe. Recálculo de calculationId usa snapshot original, não fatores correntes; retorna snapshot recomposto e difference=0. Não exige hash.

JSON/banco usam strings decimais, Decimal no domínio. Valor original e exato separados da exibição 3 casas ROUND_HALF_EVEN. Não converter strings com Number/parseFloat para calcular na interface.

## Exemplo sintético

POST /facilities/{facilityId}/records, com token e Idempotency-Key:

```json
{"sourceId":"src-electricity","period":"2026-10","quantity":"1","unit":"MWh"}
```

O sourceId é vínculo da unidade ao catálogo, não texto arbitrário. Correção:

```json
{"baseRevision":1,"quantity":"1.2","unit":"MWh","justification":"Conferência da conta"}
```

Com fator sintético 0.5 kgCO2e/kWh, resultados são 500 e 600. Os números servem só para teste. Cancelar usa baseRevision e justification, preservando revisões.

## Leitura, mediana e relatórios

Listas por cursor: padrão 20, máximo 100 e nextCursor. Totais/mediana usam todo o snapshot, não a página. Filtros de competência/unidade são restritos ao tenant. Unidade incompleta não vira zero.

Ranking somente de unidades ativas/completas com ≥1 fonte esperada. ≥3 unidades e mediana>0: abs((valor−mediana)/mediana×100)>50 alerta. Igual a 50 não alerta. Retornar status insufficient_sample/zero_median sem classificar. É alerta operacional absoluto, sem normalização por área.

POST /reports recebe calculationId e format pdf/csv. Ambos usam o mesmo snapshot; retornam arquivo diretamente, sem Cloud Storage/job/downloadUrl obrigatório. Incluir revisões, fatores, metodologia, autores, datas e pendências; não exigir período fechado. J5 obrigatória até 05/12, fora das 20 issues do CP2.

## Erros e verificação

401 token ausente/inválido/expirado; 403 sem tenant/papel admitido; 404 recurso ausente/de outra empresa; 409 duplicidade/revisão-base/chave; 422 dados incompatíveis. Testar zero, ausência, negativo, unidades, reenvio e acesso indevido. Firestore direto deny-all; Admin SDK depende da autorização na API.

Validar contrato com Redocly CLI e validar exemplos/referências contra os schemas. Quando a aplicação for implementada, executar testes de contrato reais com emuladores. As rotas e schemas do alvo não significam que os testes do backend já passaram.
