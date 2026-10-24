## Resumo
<!-- Descreva brevemente o que foi feito. -->

## Referência
**Issue / História:** #[Número_da_Issue]
**Critério de aceite atendido:** <!-- Escreva ou copie o critério específico -->

## Tipo de Mudança
- [ ] Nova funcionalidade (Feature)
- [ ] Correção de erro (Bugfix)
- [ ] Refatoração ou ajuste técnico
- [ ] Documentação

## Como testar
<!-- Descreva o passo a passo para o revisor rodar localmente e confirmar o funcionamento -->

## Impacto Arquitetural
- [ ] Novo recurso no GCP? (Se sim, cite o ADR de aprovação: ____ )
- [ ] O contrato OpenAPI (`docs/api/openapi.yaml`) foi modificado?

## Validação de Segurança
- [ ] Nenhum dado pessoal foi exposto nos *logs* de aplicação.
- [ ] Nenhum segredo/senha *hardcoded* (TruffleHog passou).

## UI/UX (Se aplicável)
<!-- Anexe prints das telas de Sucesso, Carregando, Vazio e Erro -->

## Checklist DoD
- [ ] PR revisado e CI verde.
- [ ] Teste novo ligado ao critério de aceite.
- [ ] Nenhum segredo no código.
- [ ] Telas com estados (Carregando, Vazio, Erro).
- [ ] Logs limpos (sem dados pessoais).
- [ ] Nenhum recurso novo GCP sem ADR/Tech Lead.
- [ ] Uso exclusivo de decimais exatos (sem float/double em cálculos/fatores).
- [ ] Teste de isolamento de *tenants* efetuado.