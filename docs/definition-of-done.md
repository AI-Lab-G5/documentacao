# Definition of Done: CarbonTrace

Atualizado em 25/10/2026 · [ADR-008](adr/ADR-008-recorte-crud.md). Aplicar o tipo de evidência à entrega; não confundir planejamento, documentação, implementação e homologação.

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
- [ ] UI com carregando, vazio, erro e sucesso; teclado, rótulos e status com texto e ícone além da cor. Confirmação de cancelamento somente quando essa função for implementada.
- [ ] Cada rota protegida valida token Firebase, tenant e papel funcionario; testar sem token, expirado, papel errado e empresa errada. Exceções públicas planejadas: /healthz e /readyz. Firestore direto deny-all, com testes para anônimo e autenticado.
- [ ] Decimal/strings para domínio, ROUND_HALF_EVEN e nenhuma soma de páginas como total da rede.
- [ ] No CP2, preservar entrada original, autoria/datas, fator/versão/valor e resultado; B não altera A e o cálculo original se reproduz. Revisões concorrentes, cancelamento, janela de 24h e snapshots completos da rede aplicam-se às entregas posteriores, sem apagar histórico existente.
- [ ] Fatores versionados/preliminares corretamente identificados e recálculo histórico consistente.
- [ ] Queries paginadas/controladas; sem listeners amplos sem justificativa.
- [ ] J5, quando implementada, exporta PDF/CSV do mesmo snapshot e documenta origem/pendências.
- [ ] Metas do caderno verificadas nos marcos aplicáveis; alterações dependem do tutor.

## Evidências específicas do CP2

- [ ] J1-J3 com UI/API/emuladores reais; checkout limpo e Compose sem ajustes manuais, persistência após recarga e caso de erro amigável.
- [ ] Backend/TL documentam o teto concreto antes dos testes de fronteira; QA testa abaixo/no/acima, zero, negativo, unidade e mês, com resultados conferidos à mão.
- [ ] QA publica /docs/rastreabilidade.md e /docs/relatorio-testes-v1.md em CarbonTrace, com totais executados/aprovados/reprovados/pulados, cobertura e BUG-xxx. Bugs em /docs/bugs/BUG-xxx.md, vinculados às issues responsáveis.
- [ ] CI da aplicação executa lint/build/testes e bloqueia merge com falha; PR revisado por outra pessoa. Pipeline completo de produção/deploy pertence aos marcos posteriores.
- [ ] PO registra revisões datadas S3/S4, confere a matriz, testa pessoalmente e escreve demo de 5 a 10 linhas incluindo erro.
- [ ] DevOps produz prints da calculadora e do faturamento, com premissas/data; PO confere gasto acumulado até US$ 5. Excesso exige motivo/ações em /docs/finops e apresentação ao tutor.
- [ ] J4/J5 e metas posteriores não impedem o aceite do núcleo; funcionalidade documentalmente descrita não equivale a funcionalidade entregue.

## N/A e aceite

Marcar item não aplicável com justificativa N/A e revisão explícita. N/A não substitui falha ou teste pulado. Nesta revisão documental não se exige fingir implementação/teste de quatro papéis, aprovação ou fechamento, que foram adiados.

A revisão do produto foi aprovada pelo responsável; o gate depende de evidências e do processo com o tutor. O CI existente possui TODOs e não prova sozinho segurança ou testes da aplicação.
