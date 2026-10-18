# Definition of Done (DoD) - CarbonTrace

Esta lista define as condições obrigatórias para que qualquer tarefa, história ou Pull Request (PR) seja considerada concluída e pronta para mesclar na *branch* principal.

### Itens Obrigatórios (Caderno)
- [ ] **PR revisado por alguém diferente do autor e CI verde**
  - *Como verificar:* Interface do GitHub na página do PR bloqueará o *merge* caso as regras de proteção de *branch* não tenham sido satisfeitas.
- [ ] **teste novo ligado a um critério de aceite**
  - *Como verificar:* O revisor lê o corpo do teste (ex: `test_calculo_idempotente()`) e confirma se mapeia um critério escrito no ticket Jira/GitHub Issue referenciado.
- [ ] **nenhum segredo (senha, chave) no código**
  - *Como verificar:* O job de CI `secrets-scan` (ex: TruffleHog/Gitleaks) não deteta chaves do GCP ou Firebase. O revisor faz duplo *check*.
- [ ] **telas com estados de carregando, vazio e erro**
  - *Como verificar:* Autor anexa os três *prints* (ou GIFs) da interface (React) no descritivo do PR.
- [ ] **logs sem dados pessoais**
  - *Como verificar:* O revisor confirma que nenhum campo identificável (nome, e-mail) foi incluído nas chamadas `logger.info()` no código.
- [ ] **nenhum recurso novo no GCP sem ADR ou aprovação do Tech Lead**
  - *Como verificar:* O revisor avalia se o código instancia novos serviços ou se as infraestruturas (ex: Cloud Storage) já estão catalogadas num ADR vigente.

### Itens Específicos do Projeto (Arquitetura)
- [ ] **Contrato OpenAPI atualizado no mesmo PR**
  - *Como verificar:* Se a PR toca num *endpoint*, o ficheiro `docs/api/openapi.yaml` foi atualizado e o lint do Redocly passa no CI.
- [ ] **Nenhum uso de `float` ou `double` para quantidades, fatores ou emissões**
  - *Como verificar:* CI (lint estático) ou revisor procuram por conversões literais (`float()`, tipos de TS `number`) que quebrem o ADR-007.
- [ ] **Isolamento de tenants testado**
  - *Como verificar:* Novos *endpoints* têm um teste unitário/integração validando rejeição/falta de dados caso o `tenantId` do JWT difira dos dados alojados no Firestore (ADR-002).
- [ ] **Gravação de trilha de auditoria para ações de mutação**
  - *Como verificar:* Operações de `POST` e `PATCH` despacham um evento correspondente na coleção/tabela de `audit_events`.

### Exceções
Se um item desta lista não for aplicável ao contexto da tarefa (ex: "telas com estados" numa PR apenas de backend), o autor assinala o *checkbox*, mas adiciona a justificação "N/A: [Motivo]" à frente, cabendo ao revisor concordar ou rejeitar a exceção. Referenciado em: `README.md` e `.github/pull_request_template.md`.