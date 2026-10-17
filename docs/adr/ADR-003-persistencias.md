# ADR-003: Persistência

## Status
Proposto (16/10/2026) - Autor: Tech Lead (Grupo 05)

## Contexto
A base de dados necessita suportar isolamento *multi-tenant*, integridade de auditoria e garantir exatidão absoluta nos valores dos fatores de emissão e resultados. Contudo, o Firestore (sugerido por defeito) não possui um tipo numérico exato, operando apenas com pontos flutuantes ou inteiros de 64 bits.

## Decisão
Vamos usar o **Firestore** particionado logicamente pelo `tenantId` nos documentos raiz. Coleções críticas (fatores, registos de atividade, cálculos e eventos de auditoria) operarão no modelo *append-only* (apenas inserção). As regras de segurança impedirão atualizações (`update`) ou exclusões (`delete`).

Para contornar a limitação de tipos do Firestore, **todos os decimais exatos serão armazenados como *strings* canónicas** (ex: `"145.678"`, sem notação científica). A aplicação converterá estas *strings* para o tipo nativo `decimal` em memória antes de qualquer agregação ou operação matemática. Os agregados do *dashboard* serão materializados periodicamente a partir dos cálculos originais reconstruíveis. Ficheiros (PDF/CSV) ficarão no Cloud Storage com URLs assinados de curta duração.

Alternativas descartadas:
- **Cloud SQL (PostgreSQL):** Rejeitada. Suporta nativamente o tipo `NUMERIC`, ideal para o caso, mas o custo da instância mais pequena excede imediatamente os US$ 8 mensais.
- **Firestore armazenando valores como inteiros escalados:** Rejeitada. Obriga a aplicação a gerir casas decimais variáveis de acordo com o fator, aumentando o risco de erro face à conversão automática do tipo `decimal` em Python a partir de *strings*.

## Consequências
- **Fica mais fácil:** Manter o custo da base de dados próximo de zero (escalão gratuito do Firestore).
- **Fica mais difícil:** Fazer agregações nativas (`SUM()` no Firestore) via queries diretas. Tudo o que necessite de soma exata terá de ser processado em memória na API ou através de transações que guardam o total precalculado como *string*.
- **Riscos e mitigação:** Corrupção silenciosa de decimais em escritas manuais. Mitigado proibindo escrita direta; a API é o único meio e aplicará a formatação padrão da *string*.
- **Gatilho de revisão:** Se a dimensão dos dados de agregação tornar as consultas diárias e o processamento em memória pesados ou dispendiosos.