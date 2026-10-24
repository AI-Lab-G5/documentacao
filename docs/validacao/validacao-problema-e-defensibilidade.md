# Validação do problema e da proposta — CarbonTrace

23/10/2026 · [Visão](../visao-de-produto.md) · [ADR-008](../adr/ADR-008-recorte-crud.md).
**Hipóteses ainda não validadas com usuários.** Esta revisão não adiciona entrevistas, resultados comerciais ou provas de superioridade.

## Público e perguntas

Cliente/interessado: ESG/controladoria de redes de varejo/franquias. Usuário da aplicação: funcionário que coleta consumos das unidades da empresa. Um perfil operacional não impede entrevistar compradores e usuários diferentes.

Realizar pelo menos duas entrevistas consentidas, registrando síntese sem nome/CPF/telefone/documentos internos. Se não houver entrevistas, declarar zero e rotular fontes secundárias como tal.

1. Como ocorre a coleta mensal hoje? Quem informa e quem utiliza os números?
2. Que fontes/unidades de medida são usadas, e que erros reais foram encontrados?
3. Como corrigem um consumo sem perder o valor anterior?
4. Como identificam o fator aplicado e reproduzem um relatório antigo?
5. Como comparam unidades e interpretam diferenças de tamanho/atividade?
6. Quanto tempo gastam para registrar, localizar e conferir um número?
7. Um funcionário operando as unidades com login individual é suficiente para um piloto?
8. O que impediria trocar a planilha pelo CRUD proposto? Se depender de recurso excluído, registrar incompatibilidade sem prometer entrega.
9. Quem decide, qual orçamento existe e haveria interesse em testar? Não converter resposta hipotética em receita validada.

Comparação de coleta: testar 3 participantes com 4 fontes em ≤5 minutos cada, sem assistência. Se houver comparação com planilha, usar tarefa e dados equivalentes, registrar tempo/erros/ajuda. Não inferir vantagem sem comparação.

## Fontes secundárias e limites

Referências históricas para investigar, não prova de demanda ou da validade dos fatores:
- [GHG Protocol — Corporate Standard](https://ghgprotocol.org/corporate-standard).
- [CDP — Disclosure Data Dashboard 2024](https://www.cdp.net/en/insights/disclosure-data-dashboard-2024).
- [IBM Envizi — Emissions Management](https://www.ibm.com/products/envizi/emissions-management).

Não reproduzir afirmações legais ou preços sem verificação atual específica. Fatores ambientais e condições de redistribuição precisam de conferência própria. O recorte não afirma adequação universal ou certificação.

## Hipótese de valor e defensibilidade

Hipótese: coleta padronizada, correções rastreáveis e reprodução de versões podem reduzir retrabalho frente ao processo atual. CRUD/cálculo/auditabilidade não são diferenciais exclusivos por si só. Catalogar erros observados e seus testes pode gerar aprendizado reutilizável, mas não comprova barreira competitiva.

Cada caso registra ID, origem consentida/documental, entrada sintética, resultado esperado revisado, história, teste, versão e resultado real. Não contar apenas troca de números como novo problema. As antigas metas de incorporar dois casos inéditos em 8 horas foram adiadas para não ampliar o compromisso do MVP; não são critério do caderno nem requisito da proposta original.

## Demonstração técnica

Dados abaixo são sintéticos, não fatores oficiais. Demo usa um funcionário, ≥3 unidades completas da mesma empresa e outra empresa para provar isolamento. Sem aprovar, fechar ou reabrir períodos.

| Caso | Ação | Resultado esperado |
| --- | --- | --- |
| D01 — Conversão | 1 MWh e 1.000 kWh, fator A=0,5 kgCO2e/kWh | Ambos 500 kgCO2e |
| D02 — Inválido | Eletricidade em litros, negativo, ausente | Erro; zero gravações/resultados válidos; zero explícito aceito |
| D03 — Versão original | Salvar 100 kWh×A=0,5; preparar B=0,6; recalcular snapshot A 100 vezes | 50 kgCO2e, diferença zero; A preservada |
| D04 — Correção/cancelamento | Corrigir 100→120 com motivo, calcular novo snapshot, cancelar e calcular corrente | Antigo 50, novo 60, corrente exclui cancelado; todas as revisões consultáveis |
| D05 — Soma | Unidades com 0,1234 e 0,2344 | Exato 0,3578; exibição 0,358, parcelas 0,357 e diferença 0,001 |
| D06 — Mediana | Unidades completas 100/100/200 | Mediana 100; ranking 200/100/100; somente 200 alerta |
| D07 — Exportação J5 | PDF/CSV do snapshot A após D04/B | Mesma origem/valores A, revisões/fatores/datas identificados |

Também testar ±50% exatos sem alerta, >50% com alerta, mediana zero, <3 unidades e pendências; 1.000 cenários de agregação com semente; 20 erros identificáveis com meta ≥19/20; contratos/EP-BVA e segurança conforme backlog.

D01–D06 pertencem ao CP2 J1–J4. D07 é Must do MVP até 05/12, fora das 20 issues atuais. Não declarar J5 entregue antes da implementação.

## Responsabilidades e evidências

| Função | Entrega |
| --- | --- |
| Thomas — PO | Revisar escopo, obter entrevistas/rotular hipóteses, definir aceite e registrar custos |
| Luis — Tech Lead | Conferir conversões/fatores, referências, resultados esperados e revisar contratos/PRs |
| Heitor — Frontend | Coleta, correção/cancelamento, consulta da origem e dashboard |
| Felipe Duque — Backend | Revisões, cálculo, snapshots, agregados e dados para exportação posterior |
| Felipe Vieira — DevOps + QA | Ambiente, EP/BVA, contratos, segurança, execução e relatório de resultados |

Relatório registra ambiente, versão, comandos, resultado esperado/observado, status aprovado/reprovado/não executado, PR/CI e limitação. Matriz liga requisito → aceite → teste → resultado. Usar referências reais, não resultados inventados.

No Demo Day: explicar dor/hipótese; executar coleta/correção/cálculo; demonstrar A após B; comparar mediana; exportar A em PDF/CSV; mostrar isolamento, testes e custos. Os testes demonstram comportamento nos cenários executados, não demanda, qualidade ambiental geral, conformidade legal ou vantagem sobre concorrentes.
