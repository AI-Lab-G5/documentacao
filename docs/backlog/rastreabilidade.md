# Matriz de rastreabilidade planejada

23/10/2026 · [Backlog](MoSCoW.md) · [ADR-008](../adr/ADR-008-recorte-crud.md).

Esta matriz liga o alvo de produto ao planejamento. **Todos os resultados estão pendentes** nesta revisão documental. IDs de casos abaixo são planejados; não são nomes de testes já existentes. Durante a implementação, adicionar caminho do teste, resultado observado, versão, PR e CI reais.

Issues são de [AI-Lab-G5/CarbonTrace](https://github.com/AI-Lab-G5/CarbonTrace/issues), não deste repositório. J1-J4 no CP2; J5 obrigatória no MVP posterior.

| Requisito / aceite | Caso planejado | Issues CP2 | Resultado / PR / CI |
| --- | --- | --- | --- |
| CT-M01.1-4: unidade/fonte, código, desativação e paginação | T01 cadastro/duplicidade/catálogo/limites | #7, #9, #11, #14 | Pendente |
| CT-M02.1-4: token, papel único, tenant e deny-all | T02 matriz de autorização de todas as rotas | #7, #13, #17, #23 | Pendente |
| CT-M03.1-4: validação, idempotência e erros de unidade | T03 zero/ausente/negativo/unidade/reenvio; EP/BVA | #6, #10, #12, #14, #24 | Pendente |
| CT-M04.1-4: fatores A/B e compatibilidade | T04 imutabilidade/vigência/preliminar | #8, #21, #24 | Pendente |
| CT-M05.1-5: conversão, precisão e reprodução | D01/D03, 100 recálculos, empate HALF_EVEN | #8, #19, #21, #24 | Pendente |
| CT-M07.1-4: revisão sem aprovação | D04 correção, base concorrente e retry | #12, #19, #21, #24 | Pendente |
| CT-M08.1-4: totais correntes/snapshot e paginação | D05 + 1.000 cenários com semente | #20, #22, #24 | Pendente |
| CT-M09.1-3: origem de cinco resultados | T09 detalhe/revisão/fator/autor/datas | #19, #21, #24 | Pendente |
| CT-M10.1-4: PDF/CSV do mesmo snapshot | D07 exportação A depois de B/correção | Planejamento posterior obrigatório, fora das 20 issues | Pendente, não implementado |
| CT-M11.1-3: cancelamento sem apagamento | D04 cancelamento/total/histórico/retry | #12, #19, #22, #24 | Pendente |
| CT-M12.1-4: ranking/mediana e >50% | D06, ±50%, zero, <3 e incompletas | #20, #22, #24 | Pendente |
| CP2: execução local, contrato, EP/BVA, relatório e custos | T13 checkout limpo; relatório/custos comprovados | #5, #6, #13, #14, #15, #16, #18, #23, #24 | Pendente |

CT-M06 e demais funcionalidades adiadas não serão marcadas como aceitas por ausência de implementação. Metas dos checkpoints posteriores permanecem no caderno; planejar suas evidências nos marcos correspondentes.
