# ADR-007: Aritmética decimal e política de arredondamento

## Status

Registro original: Proposto (16/10/2026) - Autor: Tech Lead (Grupo 05).
Revisado em 23/10/2026, compatível com [ADR-008](ADR-008-recorte-crud.md). A regra única substitui o ROUND_HALF_UP anteriormente proposto no backlog.

## Contexto

Quantidades e fatores decimais precisam ser convertidos, multiplicados e somados em várias unidades. Ponto flutuante binário pode representar esses valores aproximadamente; arredondar cada etapa também pode fazer o total da rede divergir da soma exata.

O usuário precisa distinguir o número exato usado no cálculo da apresentação com poucas casas. O recorte simples não elimina esse problema: mesmo duas parcelas podem exibir soma diferente do total arredondado. Uma única política evita que API, interface e exportação expliquem números diferentes.

## Decisão

Usar **decimal.Decimal** no domínio, construído de strings. Banco e JSON transportam **strings decimais sem notação científica**. A interface não usa Number/parseFloat para normalizar ou calcular quantidades/fatores/emissões; contadores e posições podem continuar números.

O contexto de precisão será dimensionado para operandos e soma, com pelo menos 28 dígitos significativos; validar o que ultrapassar a precisão suportada, sem arredondamento intermediário silencioso. Rejeitar NaN/infinito/negativo de consumo; aceitar zero explícito.

Converter somente MWh×1000→kWh; demais unidades são kWh/L/kg compatíveis com a fonte. Somar valores exatos antes de arredondar. **ROUND_HALF_EVEN**, em 3 casas na exibição, reduz a escolha sistemática do mesmo lado nos empates e mantém a regra uniforme. A política e a metodologia ficam identificadas no snapshot.

Preservar o exato e mostrar diferença de apresentação: 0.1234+0.2344=0.3578; parcelas 0.123 e 0.234 somam 0.357, mas total exibe 0.358, diferença 0.001. Empates: 1.2345→1.234; 1.2355→1.236. Classificar >50% da mediana antes do arredondamento visual.

Alternativas descartadas:
- **Float/double no domínio:** aproximação binária pode comprometer a igualdade exigida de recálculo/agregação.
- **Arredondar intermediários ou usar a soma das parcelas exibidas como total:** perde precisão e esconde o valor efetivamente calculado.
- **ROUND_HALF_UP em um documento e HALF_EVEN em outro:** políticas diferentes criam inconsistência; o projeto escolhe uma só, sem afirmar que todo uso de HALF_UP produz erro por si mesmo.
- **Inteiros com escala fixa em todos os campos:** exigem convenções adicionais para fatores e conversões; strings/Decimal mantêm o contrato já escolhido.

Aplicação no CP2 (25/10/2026): Backend implementa Decimal/conversões em #11/#21; TL revisa em #7/#18; QA prepara e executa os limites/resultados esperados em #24. TL/Backend documentam o teto concreto da quantidade por fonte/unidade antes da BVA, com PO conferindo a regra. Totais/mediana/exportações permanecem para J4/J5, sem postergar a precisão do núcleo.

## Consequências

- **Fica mais fácil:** Reproduzir cálculos, testar igualdade exata e explicar diferenças de arredondamento sem alterar os dados históricos.
- **Fica mais difícil:** Validar strings, configurar precisão suficiente e impedir conversão acidental para número binário em clientes/scripts.
- **Riscos e mitigação:** Contexto decimal insuficiente ou política diferente na exportação. Testar conversões, empates, frações e somas; reutilizar snapshot/política. Não proibir float indiscriminadamente em campos que não são valores do domínio.
- **Gatilho de revisão:** Entrada/metodologia que exija outra precisão ou apresentação. Mudança deve ser versionada e não alterar a interpretação de snapshots anteriores.
