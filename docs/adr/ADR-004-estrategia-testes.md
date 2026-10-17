# ADR-004: Estratégia de testes

## Status
Proposto (16/10/2026) - Autor: Tech Lead (Grupo 05)

## Contexto
A defensibilidade do produto baseia-se na confiança dos números para auditorias de carbono. A equipa necessita de garantir isolamento de *tenants*, consistência em recálculos (o mesmo fator deve produzir o mesmo resultado anos depois) e a estabilidade do contrato OpenAPI.

## Decisão
Vamos adotar uma estratégia de testes baseada em três pilares, integrados no CI do GitHub Actions:
1. **Testes Unitários e de Propriedade:** Usando a biblioteca Hypothesis (Python) para injetar combinações de conversão de unidades e operações decimais, garantindo que não ocorrem perdas de precisão ou inversões.
2. **Testes de Contrato:** Validação automática do código contra o ficheiro OpenAPI v0.1 para confirmar a conformidade de *endpoints*, tipos e rotas.
3. **Testes de Ouro (*Golden Tests*) e Integração:** Usando os emuladores do Firebase. Incluirão testes com dados estáticos que garantem o desafio de engenharia: recalcular um período histórico com a versão do fator original devolverá exatamente o mesmo resultado numérico *byte a byte*.

Todos os novos testes devem estar explicitamente ligados a um critério de aceitação do Definition of Done (DoD).

Alternativas descartadas:
- **Testes E2E complexos com Selenium/Cypress em CI:** Rejeitada. Aumentaria o tempo de *build* e manutenção na fase de MVP para uma equipa académica, dando prioridade à integridade da API backend.

## Consequências
- **Fica mais fácil:** Detetar regressões no cálculo exato das emissões antes do código chegar à *branch main*.
- **Fica mais difícil:** Manter a suite de testes de ouro quando uma nova metodologia legal altera retroativamente o modelo estrutural.
- **Riscos e mitigação:** Falsos positivos nos testes de isolamento de *tenants*. Mitigado através da obrigatoriedade de simulação rigorosa de diferentes tokens JWT (utilizadores A e B) nos testes de integração do emulador.
- **Gatilho de revisão:** Queda na cobertura de testes dos módulos de cálculo central abaixo dos 90%.