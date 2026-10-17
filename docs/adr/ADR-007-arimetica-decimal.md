# ADR-007: Aritmética decimal e política de arredondamento

## Status
Proposto (16/10/2026) - Autor: Tech Lead (Grupo 05)

## Contexto
Valores de emissões dependem de conversões de unidades complexas e múltiplas multiplicações de fatores (ex: conversão de litros de combustível para GJ e, depois, para toneladas de CO2e). O uso de tipos IEEE 754 (*float/double*) introduz dízimas infinitas que corrompem as somas consolidadas da rede num cenário B2B.

## Decisão
Aplica-se o uso estrito e exclusivo do tipo `decimal.Decimal` em toda a lógica de domínio do Python. A política de arredondamento será o modo **`ROUND_HALF_EVEN`** (*Banker's Rounding*, "meio para o par"), ideal para mitigar vieses de arredondamento cumulativo em somatórios estatísticos grandes de várias unidades.
**Regra temporal:** O arredondamento só é efetuado uma única vez no resultado final, ou quando este é exibido ao utilizador. Intermediações matemáticas mantêm a escala de alta precisão (ex: 12 a 28 casas decimais). Na base de dados e no contrato JSON (API), será usado formato de *string* decimal (`"X.YY"`), com o ponto como separador, mantendo o formato *pt-BR* com vírgulas exclusivamente para renderização na interface visual do utilizador (*frontend*).

Alternativas descartadas:
- **Arredondamento em passos intermediários (`ROUND_HALF_UP`):** Rejeitada. Resulta em desvios mensuráveis (*drift*) na soma anual de milhares de transações de pequenas franquias.

## Consequências
- **Fica mais fácil:** Garantir que o valor total do grupo empresarial (Dashboard) coincide no último cêntimo/centésimo com a soma matemática dos sub-relatórios individuais.
- **Fica mais difícil:** O controlo do código exigirá ferramentas automatizadas (ex: *hooks* e verificadores customizados) para sinalizar erro de CI caso um construtor de ponto flutuante `float()` seja encontrado acidentalmente.
- **Riscos e mitigação:** Erro no processamento manual via *scripts*. Mitigado com um CI/CD que não permite fusão (*merge*) onde tipos nativos de flutuantes existam nas *pipelines* de cálculo.
- **Gatilho de revisão:** Escala do sistema exigir suporte de precisão para unidades microcósmicas além do normal de mercado (acima de 28 casas decimais).