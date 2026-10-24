# ADR-002 — Autenticação e autorização

## Status
Revisado em 23/10/2026; [ADR-008](ADR-008-recorte-crud.md) substitui os quatro papéis anteriores.

## Decisão
Firebase Authentication com login individual; um único papel admitido: funcionario. Provisionamento pela equipe, sem cadastro público ou tela de usuários. Claims assinadas: tenant_id e papel=funcionario. Resposta /me usa tenantId/role por convenção do contrato.

A API verifica token e deriva tenant exclusivamente da identidade, nunca de path/query/body. Funcionário acessa todas as unidades do próprio tenant; não há restrição individual por unidade. Tenant ausente/papel não admitido →403; token ausente/inválido/expirado →401; recurso de outro tenant →404 sem revelar existência.

Firestore direto deny-all para todos os clientes. Admin SDK ignora Rules, portanto a API precisa de autorização em cada rota, inclusive relatórios, históricos e snapshots. Tokens/cabeçalhos debug não substituem a autenticação no modo demonstrado e na nuvem.

## Validação
Testar duas empresas, contas individuais, IDs de outra empresa, troca de parâmetros e papel não admitido. Contas do mesmo tenant têm permissões iguais; não testar bloqueio entre suas unidades como se esse comportamento ainda fosse obrigatório.

Em desenvolvimento validar ID tokens do Auth Emulator; fora dele, tokens Firebase válidos. Dados pessoais mínimos, UID nas trilhas, logs sem nome/e-mail. Retenção/base legal com dados reais exige validação própria; não declarar conformidade jurídica já verificada.
