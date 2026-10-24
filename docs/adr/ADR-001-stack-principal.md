# ADR-001 — Stack principal

## Status
Revisado em 23/10/2026 pelo [ADR-008](ADR-008-recorte-crud.md). Decisão técnica prevista, não evidência de deploy.

## Decisão
Manter Python/FastAPI e Decimal, React/TypeScript/Vite, Firebase Authentication/Firestore, Cloud Run e Firebase Hosting. Docker Compose/emuladores no ambiente local, GitHub Actions no CI. Evitar troca de tecnologias e novos serviços para esta revisão.

O recorte elimina jobs/Storage obrigatórios para relatórios; PDF/CSV são gerados sob demanda a partir de snapshots. Bibliotecas serão escolhidas na implementação sem alterar o contrato do produto.

## Justificativa e consequências
Reaproveitar a base existente e precisão decimal do domínio. Não afirmar que outras linguagens são incapazes, que a equipe tem experiência não demonstrada ou que custos/cold start estão comprovados.

US$3–8 continua estimativa; teto do ciclo US$50 e limites por checkpoint exigem medição. A documentação não muda configuração nem código de CarbonTrace. Alterar stack exige novo ADR; texto anterior permanece no histórico Git.
