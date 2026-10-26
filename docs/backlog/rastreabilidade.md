# Matriz de rastreabilidade planejada

Repriorização de 25/10/2026 conforme o Guia para os POs. [Backlog](MoSCoW.md) · [ADR-008](../adr/ADR-008-recorte-crud.md).

Este arquivo é um índice de planejamento do produto, não prova de execução. A matriz de aceite exigida no CP2 será montada pelo QA em **/docs/rastreabilidade.md no repositório CarbonTrace**, com uma ligação verificável para cada critério individual: requisito -> critério -> CT-xx/teste -> resultado -> versão/PR/CI. O PO confere, não substitui o QA na elaboração técnica.

São 10 issues ativas, uma por integrante em cada sprint. Evidências de software continuam pendentes até a implementação e execução reais; o registro documental da revisão S3 está disponível.

| Área / requisito CP2 | Implementação / evidência | Issues ativas | Resultado |
| --- | --- | --- | --- |
| J1 cadastro de unidades e fontes | API/UI reais, validações e persistência | #7, #9, #11, #24 | Pendente |
| J2 lançamento mensal validado | Zero, negativo, teto, unidade, mês, tipos e formatos | #7, #9, #11, #24 | Pendente |
| J3 motor com fatores versionados | Decimal, exemplo à mão, versão/valor de fator e consulta do resultado | #7, #18, #19, #21, #24 | Pendente |
| Login/tenant/papel e deny-all | Backend implementa; QA testa quatro negações por rota e acesso direto | #11, #13, #18, #21, #24 | Pendente |
| Interface acessível e estados | Teclado, rótulos, status com texto e ícone além da cor, erro/vazio/carregando e recarga | #9, #19, #24 | Pendente |
| Ambiente e CI | Compose/configuração/seed automáticos, sem ajustes manuais; lint/build/testes e bloqueio de merge | #7, #13, #18 | Pendente |
| EP/BVA, matriz e relatório | Cada critério com caso executável/resultado; total, cobertura e BUG-xxx | #13, #24; PO confere #15 | Pendente |
| Refinamento e aceite do PO | Revisões S3/S4, critérios numerados e demo de 5 a 10 linhas | #5, #15 | S3 registrada; confirmação das regras, S4, demo e aceite pendentes |
| FinOps | DevOps produz calculadora/prints do faturamento; PO confere <= US$ 5 | #13, #24; PO confere #5/#15 | Pendente |

J4/J5, métricas avançadas de agregação/mediana, PDF/CSV e detalhes de revisão/cancelamento continuam no MVP para as semanas 5 a 8, sem dependência no gate CP2. As issues substituídas ficam fechadas como consolidadas/adiadas, não como implementação entregue. Bug Reports podem ser arquivos /docs/bugs/BUG-xxx.md, vinculados ao relatório e à issue responsável, preservando no máximo 10 issues ativas.

Consultar a [revisão datada da S3](revisao-2026-10-25-s3.md) para responsáveis, links e consolidações. Os caminhos /docs/rastreabilidade.md, /docs/relatorio-testes-v1.md e /docs/bugs/BUG-xxx.md acima são entregas da aplicação ainda pendentes de produção/execução pelo QA, não arquivos de resultado criados por esta revisão.
