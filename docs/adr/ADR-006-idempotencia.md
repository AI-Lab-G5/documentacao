# ADR-006: Idempotência e identificação de lançamentos e cálculos

## Status

Registro original: Proposto (16/10/2026) - Autor: Tech Lead (Grupo 05).
Revisado em 23/10/2026 pelo [ADR-008](ADR-008-recorte-crud.md). Identificadores derivados de SHA-256 deixam de ser obrigatórios; preservam-se idempotência e histórico.

## Contexto

Falhas de rede ou cliques repetidos podem reenviar a mesma coleta. Se cada tentativa criar outro lançamento, os totais da rede ficam incorretos. Edição concorrente também pode apagar uma correção quando a segunda pessoa grava sobre uma versão desatualizada.

O plano anterior usava hash canônico do conteúdo para identificar cálculos. Essa técnica adiciona regras de serialização, canonicalização e migração que não são necessárias para comprovar a reprodução com referências explícitas e snapshots imutáveis. A prevenção de duplicação deve existir independentemente do uso de hashes.

## Decisão

Usar chave de negócio **tenant + unidade + fonte + competência**, com um lançamento lógico. Nova entrada duplicada retorna 409 e orienta corrigir o existente; a chave permanece reservada após cancelamento. Identificação não muda na correção.

Escritas usam **Idempotency-Key**, com janela de 24 horas e escopo tenant/usuário/operação/recurso. Mesma chave e payload normalizado retornam resposta/ID anteriores sem nova revisão; payload diferente com mesma chave retorna 409. Comparar campos normalizados e armazenar o vínculo da operação, sem exigir hash/canonicalização RFC.

Correção/cancelamento exigem motivo e revisão-base. Transação verifica a corrente, cria nova revisão e atualiza seu ponteiro; base desatualizada retorna 409. Verificar idempotência antes da base para que o retry de uma operação já concluída seja aceito sem recriar revisão.

Cálculos usam IDs opacos e referências explícitas às revisões/fatores. Retry da criação é idempotente; uma execução diferente pode criar outro snapshot. Dashboard seleciona apenas um snapshot, sem somar versões. Recálculo recompõe o original, não cria revisão de consumo nem troca fatores.

Alternativas descartadas:
- **Confiar só em botão desabilitado:** não cobre retry de rede ou requisição direta à API.
- **Novo documento a cada POST:** duplica consumo e pode alterar agregados sem mudança real.
- **Sobrescrever a revisão corrente:** elimina a origem e não trata disputa entre edições.
- **Hash obrigatório como identidade:** adiado porque o controle transacional e as referências atendem o recorte sem essa complexidade adicional.

Repriorização de 25/10/2026: para o CP2, manter a prevenção simples de duplicatas por tenant/unidade/fonte/competência e preservar entradas/resultados originais. A janela de Idempotency-Key de 24h, reserva após cancelamento e transações de revisão-base abaixo são desenho para evolução posterior, a reavaliar quando implementados, não critérios extras de #11/#21. Retentar uma coleta não deve gerar dois consumos; isso não exige agora todo o mecanismo de operações.

## Consequências

- **Fica mais fácil:** Repetir operações com segurança e explicar correções, sem definir manifesto ou IDs criptográficos.
- **Fica mais difícil:** Implementar janela/escopo das chaves e transações, armazenar respostas e tratar conflitos na interface.
- **Riscos e mitigação:** Reuso de chave entre empresas, comparação inconsistente ou corrida concorrente. Incluir tenant/usuário/recurso no escopo, normalizar campos e testar retries, payload diferente e base desatualizada.
- **Gatilho de revisão:** Operações assíncronas, novas formas de importação ou necessidade comprovada de deduplicação por conteúdo entre sessões; avaliar sem romper IDs e histórico existentes.
