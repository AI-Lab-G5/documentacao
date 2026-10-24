# ADR-005 — Fatores imutáveis e reprodução

## Status
Revisado em 23/10/2026; [ADR-008](ADR-008-recorte-crud.md) remove publicação pela aplicação, hashes e fluxo de fechamento.

## Decisão
Equipe prepara conjuntos A/B imutáveis e metodologia fixa identificada. A aplicação só lê versões e fatores. Sem endpoint de publicar/editar/excluir fator e sem escolha de GWP. Fator agregado documenta referência, fonte, escopo, unidade, vigência, premissas e status preliminary/verified; sem inferir conferência a partir da presença de um link.

Nova execução seleciona versão compatível mais recente vigente na competência; não haver versão gera 422. Conjuntos inválidos são rejeitados no procedimento de carga da equipe. Apenas eletricidade, diesel, gasolina e GLP compõem o catálogo.

Snapshot preserva referências e valores dos fatores, entradas/revisões e metodologia. Recalcular A usa A mesmo após B, correção ou cancelamento. Para atualizar o corrente, criar novo snapshot, sem modificar o anterior. Não exigir hash para provar isso; usar referências imutáveis, resultados e testes.

## Limites
GWP/tratamento ambiental são premissas fixas do fator agregado e metodologia, sem motor por gás. Distinguir estimativa reduzida de inventário homologado. Usar dados sintéticos identificados nos testes; apresentar fatores reais como verificados somente após conferência de fonte/condições de uso.

Risco: editar dados com Admin SDK. Mitigar por procedimento de carga somente-inserção, validação e testes de imutabilidade; Rules deny-all sozinhas não protegem Admin SDK.
