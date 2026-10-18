# ADR-002: Autenticação e autorização

## Status
Proposto (16/10/2026) - Autor: Tech Lead (Grupo 05)

## Contexto
O CarbonTrace é uma aplicação *multi-tenant* focada em empresas B2B. O sistema não permite registo público; os utilizadores são convidados. Para efeitos de auditoria e RGPD/LGPD, são guardados dados mínimos (nome, e-mail corporativo, cargo). O isolamento dos dados entre os clientes (*tenants*) é o pilar da segurança da plataforma. O orçamento estrito (US$ 3–8) inviabiliza soluções empresariais dispendiosas.

## Decisão
Vamos usar o **Firebase Authentication** padrão (modo *single project*) gerindo o *multi-tenancy* através de **Custom Claims**. O token JWT de cada utilizador conterá o `tenantId` e o `role` (papel). A API validará o token em cada pedido e extrairá o `tenantId` exclusivamente deste token, nunca do corpo ou via de acesso (*path*).

Os dados pessoais mínimos serão armazenados no Firestore numa coleção separada e encriptada (criptografia em repouso do GCP), retidos apenas enquanto o contrato durar. A trilha de auditoria referenciará apenas o ID único (UID) do autor.

Papéis de RBAC estabelecidos: `admin_esg`, `analista_controladoria`, `operador_unidade`, `auditor`.

Alternativas descartadas:
- **Google Cloud Identity Platform (Multi-tenancy nativo):** Rejeitada. Acarreta custos fixos e complexidade adicional que ultrapassam o orçamento de US$ 3–8.

## Consequências
- **Fica mais fácil:** O emulador do Firebase Auth permite testes locais fluentes, sem custos ou criação de projetos na nuvem.
- **Fica mais difícil:** A revogação ou alteração imediata de privilégios, visto que os *custom claims* propagam-se apenas quando o token é renovado (até 1 hora).
- **Riscos e mitigação:** Exposição de dados de um *tenant* a outro. Mitigado forçando a injeção do `tenantId` no contexto da requisição na camada de *middleware* da API, antes da lógica de negócio.
- **Gatilho de revisão:** Se a plataforma precisar de suportar SSO (Single Sign-On) federado (SAML/OIDC) por cliente corporativo no futuro.