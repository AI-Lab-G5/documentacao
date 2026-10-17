# Documentação da API - CarbonTrace

Este diretório contém o contrato oficial da API (`openapi.yaml`) e o guia de referência rápida para os programadores.

## Resumo de Jornadas, Operações e Papéis

| Jornada | Operações (Endpoints) | Papéis permitidos |
| :--- | :--- | :--- |
| **Transversal** | `GET /health`, `GET /me` | Público (health), Todos (me) |
| **J1** | `/facilities`, `/sources`, `/catalogs` | `admin_esg` (escrita), Todos (leitura) |
| **J2** | `/records`, `/correct`, `/approve` | `admin_esg`, `operador_unidade` (inserir), `analista_controladoria` (aprovar) |
| **J3** | `/methodologies`, `/factor-versions`, `/calculations` | `admin_esg` (publicar), `analista_controladoria` (calcular), `auditor` (ler) |
| **J4** | `/dashboard/summary`, `/outliers` | `admin_esg`, `analista_controladoria` |
| **J5** | `/reports`, `/download`, `/audit-events` | `admin_esg`, `auditor` |

## Decisões de Contrato e Suposições
- **Idempotência:** O cabeçalho `Idempotency-Key` atua apenas na intenção de requisição (evita duplicados na rede). O hash de conteúdo do cálculo dita a idempotência de negócio: mesmos inputs = mesmo `contentHash` (`200 OK` devolvendo o cálculo antigo).
- **Sem exclusões:** O método DELETE não é suportado; atualizações de estado mudam para `isActive: false` (J1) ou criam novas revisões retroativas (J2).
- **Dados consolidados:** Apenas os registos mensais no estado `approved` entram no processamento de cálculos (J3).

## Como validar o contrato localmente
Sempre que alterar o ficheiro `openapi.yaml`, corra o linter rigoroso do Redocly para garantir que a formatação e as referências (`$ref`) estão corretas:
```bash
npx @redocly/cli lint docs/api/openapi.yaml