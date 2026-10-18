# Documentação - CarbonTrace

Repositório central de documentação e especificações do projeto **CarbonTrace**, plataforma de gestão de emissões de GEE (Gases de Efeito Estufa - Escopos 1 e 2) voltada para redes de varejo e franquias.

---

## Estrutura da Documentação

A documentação do projeto está estruturada nas seguintes pastas e arquivos com os caminhos atualizados:

### 1. Produto e Negócio
- [Visão do Produto](docs/visao-de-produto.md): Contexto de negócio, personas, análise de causa raiz (Diagrama de Ishikawa), jornadas do utilizador e requisitos.
- [Validação do Problema e Defensibilidade](docs/validacao/validacao-problema-e-defensibilidade.md): Hipóteses de validação, dados de mercado (CDP) e estratégia de defensibilidade.

### 2. Backlog e Qualidade
- [Backlog MoSCoW e Critérios de Aceite](docs/backlog/MoSCoW.md): Requisitos priorizados pelo método MoSCoW com histórias de utilizador e critérios de aceite mensuráveis.
- [Definition of Done (DoD)](docs/definition-of-done.md): Lista de verificação e condições obrigatórias para que qualquer tarefa ou Pull Request seja considerada concluída.

### 3. Arquitetura e Decisões Técnicas
- [Registos de Decisão de Arquitetura (ADR)](docs/adr/README.md): Índice das decisões arquiteturais adotadas:
  - [ADR-001: Stack principal](docs/adr/ADR-001-stack-principal.md)
  - [ADR-002: Autenticação e autorização](docs/adr/ADR-002-autenticacao.md)
  - [ADR-003: Persistência](docs/adr/ADR-003-persistencias.md)
  - [ADR-004: Estratégia de testes](docs/adr/ADR-004-estrategia-testes.md)
  - [ADR-005: Versionamento imutável de fatores](docs/adr/ADR-005-fatores-imutaveis.md)
  - [ADR-006: Idempotência e identificação](docs/adr/ADR-006-idempotencia.md)
  - [ADR-007: Aritmética decimal](docs/adr/ADR-007-arimetica-decimal.md)

### 4. API e Contratos
- [Documentação da API](docs/api/README.md): Visão geral dos contratos, resumo das jornadas (J1 a J5), perfis de acesso (RBAC) e instruções de validação local.
- [Contrato OpenAPI](docs/api/openapi.yaml): Especificação OpenAPI 3.1.0 com todos os endpoints, esquemas de dados e exemplos de requisição/resposta.

### 5. Configuração e Governança
- [Pipeline de CI](.github/workflows/ci.yml): Automação no GitHub Actions para verificações de segredos, formatação (*lint*), testes e *build*.
- [Template de Pull Request](.github/pull_request_template.md): Modelo padronizado para submissão e revisão de alterações.
- [CODEOWNERS](.github/CODEOWNERS): Definição de responsabilidades e revisores do repositório.

---

## Glossário de Termos Úteis

- **ADR (Architecture Decision Record):** Registo curto de uma decisão técnica documentando o contexto, o que foi decidido e as suas consequências (ex: "Usaremos Firestore porque...").
- **OpenAPI:** O contrato da API. É a "ementa do backend para o frontend", listando todos os endpoints, o que cada um recebe e o que devolve, sem conter código de implementação.
- **Definition of Done (DoD):** A lista de verificação com as condições obrigatórias para que qualquer tarefa ou Pull Request seja considerada pronta e finalizada.
- **Critério de aceite mensurável:** Regra que dita quando uma história de utilizador está concluída, exigindo um número ou resultado testável (ex: mau: "ser rápido"; bom: "responder em até 800 ms").
- **Docker Compose:** Um comando (`docker compose up`) que levanta o sistema inteiro (API, frontend e base de dados) no computador de qualquer integrante da equipa, garantindo que o ambiente local é igual para todos.
- **Emulador Firebase:** Versão local e gratuita do Firebase (Auth e Firestore) utilizada para testar a aplicação sem consumir o orçamento da nuvem.
- **CI (Continuous Integration):** Verificações automáticas que correm no GitHub a cada Pull Request (PR). Se o código tiver erros de formatação (*lint*) ou os testes falharem, o PR fica "vermelho" e o *merge* é bloqueado.
- **Tenant:** Cada empresa cliente (rede ou franquia) que utiliza o sistema. Os dados operam em isolamento absoluto: os dados da empresa A nunca podem aparecer ou interferir com os da empresa B.

---
