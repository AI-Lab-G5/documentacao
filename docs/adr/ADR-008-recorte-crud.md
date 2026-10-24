# ADR-008 — CRUD com requisitos originais preservados

## Status

Decisão de produto aprovada pelo responsável do projeto em 23/10/2026. Implementação pendente nas issues; alteração documental não significa aceite técnico nem aprovação do tutor. Revisa parcialmente ADR-001 a ADR-007; não altera a stack.

## Contexto e decisão

A visão anterior ampliou o MVP para quatro perfis, aprovação segregada, fechamento/reabertura e infraestrutura de auditoria. O recorte inicial de CRUD puro retiraria requisitos centrais da proposta. A decisão é preservar cinco jornadas com operação simplificada.

Usuário único: funcionário responsável pela coleta, login individual, mesmas permissões nas unidades da própria empresa. ESG/controladoria são cliente/interessados, não perfis da aplicação. Provisionamento de contas e fatores pela equipe.

Preservar: J1 unidades/fontes; J2 coleta validada; J3 fatores versionados/cálculo/reprodução; J4 dashboard/ranking/mediana; J5 PDF/CSV rastreável. Fontes: eletricidade, diesel, gasolina, GLP. Conversão kWh/MWh, demais em L/L/kg. Decimal, strings e ROUND_HALF_EVEN em 3 casas.

Correção cria revisão imutável; cancelamento lógico preserva origem e exclui dos totais correntes. Unidade/fonte são desativadas. Não há DELETE físico. Snapshots guardam entradas, configuração esperada, conversões, fatores, metodologia, resultados, autor/data. B ou correção não modifica snapshot A.

Ranking absoluto kgCO2e; somente unidades ativas/completas com ≥1 fonte esperada. Desvio absoluto >50% da mediana com ≥3 completas e mediana >0; fora dessas condições, avisar sem classificar. Não usar intensidade por área ou teste estatístico.

PDF/CSV síncronos do mesmo snapshot, com revisões/fatores/UID/datas, sem fechamento, jobs, Storage obrigatório ou manifesto. Fatores não conferidos continuam preliminares.

## Cortes explícitos

Fora do MVP: quatro perfis/gestão de permissões; aprovação; fechamento/reabertura; auditor convidado; refrigerantes; intensidade por área/MAD; escolha de GWP; hashes, assinatura ou infraestrutura separada de auditoria. Excluem-se também Escopo 3, OCR, concessionárias, ERP, anexos, certificação, créditos, app nativo e benchmark entre clientes.

Metodologia fixa registra premissas dos fatores agregados; não prometer inventário completo, homologação metodológica ou diferenciação comercial sem validação. Ishikawa permanece diagnóstico histórico, sem obrigar tratar todas as causas.

## Correspondência e efeitos

IDs equivalentes são mantidos com critérios revisados. CT-M06 fica adiada e reservada. CT-S02 avançada fica fora, com comparação básica obrigatória em CT-M12 (novo). CT-M11 (novo) trata cancelamento. Ver [backlog](../backlog/MoSCoW.md).

Esta decisão prevalece sobre trechos substituídos dos ADRs: quatro papéis em ADR-002, fechamento/Storage em ADR-003, hashes/publicação na API em ADR-005/006 e regras de teste ligadas a aprovação em ADR-004. Conteúdo anterior permanece no histórico Git.

## Entregas e estado real

Sprint 2 (19–25/10): J1/J2 e fundação do cálculo. Sprint 3 (26/10–01/11): J3/J4, revisões/cancelamentos e gate CP2. J5 é Must até 05/12, mas não foi acrescentada às 20 issues CP2. Preservar responsáveis, estados e milestones; não encerrar issues automaticamente.

[OpenAPI 0.2.0](../api/openapi.yaml) é contrato-alvo, diferente do código/contrato gerado atual no repositório CarbonTrace. Esta revisão altera somente documentacao; não implementa produto ou altera código/configuração daquele repositório.

Manter exigências técnicas do caderno: segurança/deny-all, EP/BVA, contratos, evidências e custos no CP2; operação, qualidade, observabilidade e rollback nos marcos seguintes. Apresentar a revisão ao tutor; exigências externas não são dispensadas.

## Consequências e validação

Reduzem-se telas, papéis e transições de negócio, mantendo as cenas da demo. Permanecem trabalho de histórico, snapshots e PDF/CSV: não se garante prazo apenas pelo corte. Confirmar capacidade e bloqueios semanalmente.

Aceite da decisão: documentação/contrato coerentes, IDs rastreáveis, critérios e issues sem exigências removidas, cinco jornadas cobertas no MVP e J1–J4 no CP2. Evidência de implementação virá de testes/PR/CI reais.

GCP/Firebase mantidos; US$3–8 é estimativa, não medição. Registrar custo real (CP2 ≤US$5, CP3 ≤US$12, ciclo ≤US$50), com ação a US$40. Workflow e CODEOWNERS existentes permanecem inalterados.
