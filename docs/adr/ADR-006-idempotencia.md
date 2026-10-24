# ADR-006 — Idempotência e revisões

## Status
Revisado em 23/10/2026; [ADR-008](ADR-008-recorte-crud.md) substitui IDs derivados de SHA-256 e manifesto criptográfico.

## Decisão
Chave de negócio do lançamento: tenant + unidade + fonte + competência. Duplicação retorna 409 e orienta corrigir o existente. A chave permanece reservada após cancelamento. Fonte/unidade/competência não mudam na correção.

Escritas usam Idempotency-Key: repetir chave/payload normalizado retorna recurso original sem nova revisão/evento; chave com outro payload retorna 409. Escopo da chave inclui tenant, usuário, operação e recurso; registrar resposta/ID por 24h. Comparar campos normalizados, sem exigir hash criptográfico/canonicalização RFC.

Correção/cancelamento informam revisão-base e motivo. Transação verifica revisão corrente e incrementa a revisão; base desatualizada retorna 409. Idempotência deve ser consultada antes da checagem da base para permitir repetição da mesma operação já concluída.

Snapshots têm IDs opacos e referências explícitas, não derivadas do conteúdo. Mesmo reenvio de criação de cálculo é idempotente; outra execução pode criar novo snapshot. Dashboard nunca soma snapshots diferentes: seleciona um único conjunto de revisões/resultados.

Recalcular snapshot não muda entradas/versões e não cria revisão de consumo; retorna resultado recomposto e diferença. Não recriar histórico com fatores correntes.

## Validação
Simular reenvios, payload diferente com mesma chave, disputa de revisão-base, duplicidade concorrente e cancelamento. Provar que rede instável não gera dupla emissão. O controle é transacional na API, sem infraestrutura criptográfica.
