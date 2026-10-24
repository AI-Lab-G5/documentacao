# ADR-007 — Decimal e arredondamento

## Status
Revisado em 23/10/2026; compatível com [ADR-008](ADR-008-recorte-crud.md). A regra única substitui o ROUND_HALF_UP antes proposto no backlog.

## Decisão
Quantidades, fatores, conversões e emissões usam decimal.Decimal no domínio, criado de strings. JSON e Firestore preservam strings decimais sem notação científica. Frontend não usa Number/parseFloat para calcular ou normalizar valores exatos; contadores e posições de UI podem ser números.

Precisão de trabalho dimensionada para os operandos e a soma, de pelo menos 28 dígitos significativos, com validação para não aceitar dados que excedam a precisão suportada silenciosamente. Evitar arredondamento intermediário por contexto insuficiente. Sem NaN/infinito/negativo para consumo. Quantidade explicitamente zero é válida.

Conversão permitida: MWh×1000→kWh; kWh, L e kg mantêm unidade-base da fonte compatível. Calcular/somar exato antes de apresentar; saída de exibição em 3 casas usando ROUND_HALF_EVEN. Guardar valor não arredondado no snapshot. Diferença entre soma das parcelas exibidas e total exibido deve aparecer.

Exemplo sintético: 0.1234+0.2344=0.3578; exibir parcelas 0.123 e 0.234, total 0.358 e diferença 0.001. Empate 1.2345→1.234; 1.2355→1.236.

## Consequências
Política/metodologia identificadas no snapshot e constantes no recálculo. Classificar alerta >50% antes de arredondar. Strings são contrato, não homologação ambiental. Testar fronteiras, soma e conversão; não proibir floats indiscriminadamente em campos que não são valores de domínio.
