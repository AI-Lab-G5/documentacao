# CarbonTrace: documentação do MVP

Grupo 05 · Revisão de escopo de **23/10/2026**, com planejamento alinhado em **25/10/2026**. O CRUD de consumos é a base, preservando cinco jornadas: unidades/fontes, coleta, cálculo versionado, dashboard da rede e PDF/CSV rastreável.

**Este repositório descreve o comportamento-alvo, não comprova implementação.** A OpenAPI 0.2.0 é planejada. A API e o frontend de [CarbonTrace](https://github.com/AI-Lab-G5/CarbonTrace) precisam ser adaptados e testados. Esta revisão não modifica código, configuração ou contrato gerado naquele repositório.

## Leitura recomendada

- [ADR-008: decisão formal e cortes](docs/adr/ADR-008-recorte-crud.md).
- [Visão do produto](docs/visao-de-produto.md).
- [Backlog MoSCoW e correspondência dos IDs](docs/backlog/MoSCoW.md).
- [Revisão datada da S3 e consolidação das issues](docs/backlog/revisao-2026-10-25-s3.md).
- [Matriz de rastreabilidade planejada](docs/backlog/rastreabilidade.md): casos e resultados ainda pendentes.
- [Validação do problema e demonstração](docs/validacao/validacao-problema-e-defensibilidade.md).
- [Índice das decisões arquiteturais](docs/adr/README.md).
- [Guia da API](docs/api/README.md) e [OpenAPI 0.2.0](docs/api/openapi.yaml).
- [Definition of Done](docs/definition-of-done.md) e [template de PR](.github/pull_request_template.md).

## Recorte aprovado

Um perfil funcional, **funcionário responsável pela coleta**, com login individual e acesso às unidades da própria empresa. ESG/controladoria continuam como público comprador, sem perfis separados. Contas e fatores são preparados pela equipe, sem telas de administração.

Fontes: eletricidade, diesel, gasolina e GLP. Correção cria revisão; exclusão significa cancelamento lógico. Preservam-se fatores/cálculos imutáveis, conversões delimitadas, precisão decimal, ranking/mediana e exportações consistentes.

Fora do MVP: aprovação segregada, fechamento/reabertura, quatro perfis, auditor convidado, refrigerantes, intensidade por área, auditoria criptográfica, Escopo 3, OCR, integrações, certificação e créditos de carbono.

## Marcos e evidências

| Marco | Entrega planejada |
| --- | --- |
| [Sprint 2: 19 a 25/10](https://github.com/AI-Lab-G5/CarbonTrace/milestone/2) | J1/J2 e base local; 5 issues: #5, #7, #9, #11 e #13 |
| [Sprint 3: 26/10 a 01/11](https://github.com/AI-Lab-G5/CarbonTrace/milestone/3) | J3 e aceite de J1-J3; 5 issues: #15, #18, #19, #21 e #24 |
| Demo Day: 05/12/2026 | Cinco jornadas homologadas, incluindo J5 PDF/CSV |

Repriorização de 25/10/2026 conforme o Guia para os POs: o CP2 cobre especificamente J1-J3, em 10 issues ativas (uma por integrante em cada sprint). J4/J5 continuam no MVP para as semanas 5 a 8; não bloqueiam este gate. Issues substituídas são encerradas como consolidadas/adiadas e retiradas das milestones, sem contar como entrega. Publicar documentos não encerra implementação nem aprova gate. Informar o tutor sobre a revisão; exigências técnicas do caderno permanecem.

| Área / responsável | Sprint 2, até 25/10 | Sprint 3, até 01/11 |
| --- | --- | --- |
| PO / @Thomas4ugust0 | [#5](https://github.com/AI-Lab-G5/CarbonTrace/issues/5) | [#15](https://github.com/AI-Lab-G5/CarbonTrace/issues/15) |
| Tech Lead / @pontesluis | [#7](https://github.com/AI-Lab-G5/CarbonTrace/issues/7) | [#18](https://github.com/AI-Lab-G5/CarbonTrace/issues/18) |
| Frontend / @heitormontt | [#9](https://github.com/AI-Lab-G5/CarbonTrace/issues/9) | [#19](https://github.com/AI-Lab-G5/CarbonTrace/issues/19) |
| Backend / @felipecduque7 | [#11](https://github.com/AI-Lab-G5/CarbonTrace/issues/11) | [#21](https://github.com/AI-Lab-G5/CarbonTrace/issues/21) |
| DevOps + QA / @Bipelipe | [#13](https://github.com/AI-Lab-G5/CarbonTrace/issues/13) | [#24](https://github.com/AI-Lab-G5/CarbonTrace/issues/24) |

Sprint 2 e Sprint 3 da equipe correspondem à S3 e à S4 do guia. A revisão da S3 foi registrada em 25/10; a revisão da S4 ainda deverá ocorrer entre 26/10 e 01/11. Pendências de 25/10 devem ser registradas e replanejadas, sem conclusão retroativa.

GCP + Firebase continuam como base. US$ 3-8 é estimativa, não gasto medido nem garantia. Limites acumulados: CP2 US$ 5; CP3 US$ 12; ciclo US$ 50. A US$ 40, revisar/desligar recursos não críticos. Registrar gasto real, data e evidência.

O [workflow existente](.github/workflows/ci.yml) tem etapas TODO; não representa segurança e testes completos. Workflow e CODEOWNERS não são modificados nesta revisão.

## Glossário

- **CT-Mxx:** ID histórico de história originalmente Must; prioridade vigente está no backlog, inclusive para itens adiados.
- **J1-J5:** jornadas, não perfis.
- **Tenant:** empresa isolada das demais, determinada pelo token.
- **Revisão:** nova entrada preservando a anterior após correção/cancelamento.
- **Snapshot:** conjunto imutável de entradas, configurações, fatores e resultados para reprodução/exportação.
- **EP/BVA:** particionamento de equivalência e análise de valores-limite.
