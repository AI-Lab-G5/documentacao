# CarbonTrace — documentação do MVP

Grupo 05 · Revisão de escopo de **23/10/2026**. O CRUD de consumos é a base, preservando cinco jornadas: unidades/fontes, coleta, cálculo versionado, dashboard da rede e PDF/CSV rastreável.

**Este repositório descreve o comportamento-alvo, não comprova implementação.** A OpenAPI 0.2.0 é planejada. A API e o frontend de [CarbonTrace](https://github.com/AI-Lab-G5/CarbonTrace) precisam ser adaptados e testados. Esta revisão não modifica código, configuração ou contrato gerado naquele repositório.

## Leitura recomendada

- [ADR-008 — decisão formal e cortes](docs/adr/ADR-008-recorte-crud.md).
- [Visão do produto](docs/visao-de-produto.md).
- [Backlog MoSCoW e correspondência dos IDs](docs/backlog/MoSCoW.md).
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
| [Sprint 2 — 19 a 25/10](https://github.com/AI-Lab-G5/CarbonTrace/milestone/2) | J1/J2 integradas e base do cálculo; issues #5–#14 |
| [Sprint 3 — 26/10 a 01/11](https://github.com/AI-Lab-G5/CarbonTrace/milestone/3) | J3/J4, correção/cancelamento e evidências do CP2; issues #15–#24 |
| Demo Day — 05/12/2026 | Cinco jornadas homologadas, incluindo J5 PDF/CSV |

J5 é obrigatória no MVP, mas não foi adicionada às 20 issues do CP2. Publicar documentos não encerra implementação nem aprova gate. Informar o tutor sobre a revisão; exigências técnicas do caderno permanecem.

GCP + Firebase continuam como base. US$ 3–8 é estimativa, não gasto medido nem garantia. Limites acumulados: CP2 US$ 5; CP3 US$ 12; ciclo US$ 50. A US$ 40, revisar/desligar recursos não críticos. Registrar gasto real, data e evidência.

O [workflow existente](.github/workflows/ci.yml) tem etapas TODO; não representa segurança e testes completos. Workflow e CODEOWNERS não são modificados nesta revisão.

## Glossário

- **CT-Mxx:** ID histórico de história originalmente Must; prioridade vigente está no backlog, inclusive para itens adiados.
- **J1–J5:** jornadas, não perfis.
- **Tenant:** empresa isolada das demais, determinada pelo token.
- **Revisão:** nova entrada preservando a anterior após correção/cancelamento.
- **Snapshot:** conjunto imutável de entradas, configurações, fatores e resultados para reprodução/exportação.
- **EP/BVA:** particionamento de equivalência e análise de valores-limite.
