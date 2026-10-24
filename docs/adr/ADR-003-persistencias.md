# ADR-003 — Persistência

## Status
Revisado em 23/10/2026 pelo [ADR-008](ADR-008-recorte-crud.md). Substitui fechamento obrigatório, materialização periódica e Storage obrigatório.

## Decisão
Firestore com dados por tenant, mantendo isolamento na API. Referências/valores decimais como strings. Fatores publicados, revisões de consumos e snapshots de cálculo são imutáveis; ponteiros de revisão corrente e metadados de unidade/fonte podem ser atualizados em transação.

Lançamento lógico identificado por unidade/fonte/competência. Correção/cancelamento cria revisão com número, referência anterior, UID, instante e motivo. Revisão-base evita perda concorrente. Não marcar a revisão antiga por atualização destrutiva; seu encadeamento e ponteiro corrente identificam a vigente. Cancelamento não libera a chave para outro lançamento nem oferece reativação neste MVP.

Desativar unidade/fonte não apaga histórico ou cancela entradas. Snapshots contêm configuração esperada de fontes, revisões, conversões, fatores, metodologia, valores exatos, autor/data. Selecionar anterior não consulta estado corrente. Somente um conjunto de revisões entra em cada total.

PDF/CSV gerados síncronos do mesmo snapshot, sem armazenar arquivos de forma permanente. Sem serviço separado de auditoria: revisões/snapshots ligados fornecem a trilha mínima.

## Consequências e controles
Consultas paginadas e filtradas, sem stream irrestrito ou listeners amplos sem justificativa. Agregação decimal na API, a partir do recorte completo, não apenas de uma página da interface. Total/mediana armazenados no snapshot evitam recomputação por página.

A implementação define índices e limites operacionais e registra leituras/custos. Testar transações, revisões, cancelamentos, isolamento e ausência de duplicação. No Firestore cliente, deny-all; a imutabilidade via Admin SDK é responsabilidade testada da API.
