# ADR-006: Idempotência e identificação de cálculos por conteúdo

## Status
Proposto (16/10/2026) - Autor: Tech Lead (Grupo 05)

## Contexto
O registo manual de consumo em múltiplos *tenants* acarreta problemas de duplicação por falhas de rede. Além disso, quando uma unidade envia uma correção de dados de um mês, o sistema necessita de recalcular sem perder o histórico do cálculo errado que outrora esteve em vigor.

## Decisão
Vamos utilizar identificadores de cálculo derivados do seu conteúdo. A identificação gerada será baseada no algoritmo **SHA-256** calculando o *hash* da serialização canónica (JSON formatado) das entradas: revisões das atividades, versão exata da metodologia/fatores e a política de arredondamento. O utilizador e a data do processo ficarão de fora do *hash*.
Para todas as operações de escrita (`POST`), é exigido o uso do cabeçalho `Idempotency-Key` com uma janela de validade (24 horas). Correções retroativas de dados originam uma nova revisão na fonte e, logo, produzem um cálculo com um novo *hash*. O cálculo antigo é preservado e marcado como "substituído_por" apontando para o novo identificador.

Alternativas descartadas:
- **IDs sequenciais puros e sobreposição de dados:** Rejeitada. Impossibilita o rasto das correções de dados e viola a rastreabilidade do cálculo.

## Consequências
- **Fica mais fácil:** Evitar a repetição do processamento se for enviado um pedido duplo acidental. O mesmo *hash* retorna `200 OK` do cálculo pré-existente sem gravar novamente.
- **Fica mais difícil:** Criar a serialização canónica padrão em Python que ordene perfeitamente chaves JSON para o cálculo correto do *hash* (requer rigor no RFC 8785).
- **Riscos e mitigação:** Exaustão e bloqueio na verificação de idempotência em simultâneo. Mitigado usando transações condicionais rápidas do Firestore.
- **Gatilho de revisão:** Mudança profunda na forma como um modelo de dados de uma atividade é composto, exigindo migração dos *hashes*.