# Definition of Done — CarbonTrace

23/10/2026 · [ADR-008](adr/ADR-008-recorte-crud.md). Aplicar o tipo de evidência à entrega; não confundir planejamento, documentação, implementação e homologação.

## Toda entrega

- [ ] Issue/critério de aceite identificados, sem declarar funções removidas como obrigatórias.
- [ ] PR revisado por outra pessoa e verificações reais executadas; CI/resultado vinculados.
- [ ] Documentação e matriz requisito → aceite → teste → resultado atualizadas.
- [ ] Sem segredos ou dados pessoais em logs; não alegar ferramenta de varredura executada se houver apenas TODO no CI.
- [ ] Nenhum recurso novo de nuvem sem justificativa/ADR; custo acompanhado.
- [ ] Falhas, testes pulados e dependências explicitados; nenhum caso não executado marcado como aprovado.

## Alteração somente documental

- [ ] Visão, backlog/IDs, ADRs, exemplos e contrato-alvo coerentes.
- [ ] OpenAPI lintada e referências/exemplos validados quando alterados.
- [ ] Links internos e cinco jornadas conferidos; itens adiados explicitamente rotulados.
- [ ] Estado planejado separado do implementado; mudança documental não encerra automaticamente issue de software.
- [ ] Workflow/CODEOWNERS e código do CarbonTrace preservados nesta revisão de escopo.

## Implementação, quando aplicável

- [ ] Testes relevantes ligados ao aceite, incluindo contrato e EP/BVA; ambiente, versão e comandos reproduzíveis.
- [ ] UI com carregando, vazio, erro e sucesso; confirmação de cancelamento e aviso de limites.
- [ ] API autoriza tenant/papel funcionario em cada rota; testes de token e acesso indevido; Firestore direto deny-all.
- [ ] Decimal/strings para domínio, ROUND_HALF_EVEN e nenhuma soma de páginas como total da rede.
- [ ] Revisões/snapshots preservados; correção/cancelamento idempotentes e origem consultável.
- [ ] Fatores versionados/preliminares corretamente identificados e recálculo histórico consistente.
- [ ] Queries paginadas/controladas; sem listeners amplos sem justificativa.
- [ ] J5, quando implementada, exporta PDF/CSV do mesmo snapshot e documenta origem/pendências.
- [ ] Metas do caderno verificadas nos marcos aplicáveis; alterações dependem do tutor.

## N/A e aceite

Marcar item não aplicável com justificativa N/A e revisão explícita. N/A não substitui falha ou teste pulado. Nesta revisão documental não se exige fingir implementação/teste de quatro papéis, aprovação ou fechamento, que foram adiados.

A revisão do produto foi aprovada pelo responsável; o gate depende de evidências e do processo com o tutor. O CI existente possui TODOs e não prova sozinho segurança ou testes da aplicação.
