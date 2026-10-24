# ADR-002: Autenticação e autorização

## Status

Registro original: Proposto (16/10/2026) - Autor: Tech Lead (Grupo 05).
Revisado em 23/10/2026 pelo [ADR-008](ADR-008-recorte-crud.md), substituindo a matriz anterior de quatro perfis.

## Contexto

O CarbonTrace atende empresas B2B e precisa identificar quem informou ou corrigiu consumos e impedir acesso aos dados de outra empresa. Esse isolamento continua obrigatório mesmo no piloto. Uma conta compartilhada impediria atribuir autoria individual; retirar autenticação não é uma forma aceitável de reduzir o escopo.

A proposta anterior incluía gestor ESG, controladoria, operador de unidade e auditor. Aprovações, permissões diferentes e acesso temporário criavam telas, transições e combinações de teste além da coleta pretendida. O recorte aprovado concentra o trabalho em um funcionário que opera as unidades da própria empresa.

## Decisão

Manter **Firebase Authentication**, contas individuais provisionadas pela equipe e **Custom Claims** assinadas: `tenant_id` e `papel=funcionario`. A API valida o token em cada rota protegida e deriva o tenant da identidade, nunca de um tenant informado em path, query ou body. Por convenção do contrato, `/me` devolve `tenantId` e `role`.

Todos os funcionários têm as mesmas funcionalidades nas unidades do próprio tenant. Não há cadastro público, tela de gestão de usuários, restrição individual por unidade nem perfil separado de auditor. Token ausente/inválido/expirado retorna 401; ausência de tenant ou papel admitido retorna 403; recurso de outra empresa retorna 404 sem revelar existência.

Firestore direto permanece **deny-all** para clientes anônimos/autenticados. A API usa Admin SDK, que não é protegido pelas Rules; por isso ela precisa autorizar registros, históricos, cálculos, dashboard e relatórios. No ambiente demonstrado, validar tokens do Auth Emulator; cabeçalhos debug não substituem autenticação. Fora dele, validar tokens Firebase apropriados.

Dados pessoais mínimos ficam separados da trilha de revisões/snapshots, que usa UID opaco; logs não devem conter nome/e-mail. Políticas de retenção/base legal com dados reais exigem validação própria, não uma declaração de conformidade já alcançada.

Alternativas descartadas:
- **Quatro perfis e controle por unidade:** adiados porque pertenciam aos fluxos de aprovação/auditoria retirados; um perfil reduz combinações sem perder isolamento entre empresas.
- **Conta única compartilhada ou tenant enviado pelo navegador:** rejeitados por perda de autoria e possibilidade de ampliar acesso indevidamente.
- **Multi-tenancy nativo/SSO empresarial:** não necessário ao piloto; avaliar complexidade, recursos e custo somente se surgirem requisitos reais, sem pressupor preço vigente.

## Consequências

- **Fica mais fácil:** Executar o CRUD com um conjunto único de permissões e reproduzir testes locais com contas individuais.
- **Fica mais difícil:** Provisionamento e alteração de vínculo continuam administrativos; mudanças de claims exigem renovação/validação do token.
- **Riscos e mitigação:** Vazamento entre tenants ou confiança indevida nas Rules. Centralizar o contexto de identidade, negar por padrão e testar tokens/IDs externos em todas as rotas.
- **Gatilho de revisão:** Necessidade comprovada de aprovação segregada, permissão por unidade, gestão de contas na interface ou SSO. Exige novo planejamento, não extensão silenciosa do papel atual.
