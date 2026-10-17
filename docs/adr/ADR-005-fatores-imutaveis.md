# ADR-005: Versionamento imutável de fatores e metodologia

## Status
Proposto (16/10/2026) - Autor: Tech Lead (Grupo 05)

## Contexto
Empresas sofrem com a instabilidade de planilhas. Para evitar *greenwashing*, o cálculo do período "X" auditado deve permanecer estático. Metodologias (regras de cálculo) e fatores (valores GWP) mudam com o tempo e requerem um rasto histórico inquebrável.

## Decisão
Vamos aplicar um **padrão de versionamento e imutabilidade estrita**. Versões publicadas de fatores ou metodologias não possuem operações de `UPDATE` ou `DELETE` na API. Qualquer correção resulta na publicação de uma nova versão (ex: `v1.2` substitui a anterior, que passa ao estado *obsoleto* mas permanece na base de dados para cálculo retroativo de registos que a utilizaram).
O conteúdo terá um *hash* armazenado para atestar que os dados não foram modificados no banco de forma ilegal. A fixação é automática: cada cálculo regista qual foi a versão exata do fator aplicada. A substituição desta versão num período já fechado exige privilégios administrativos, originando obrigatoriamente um evento na trilha de auditoria.

Alternativas descartadas:
- **Atualização na mesma entidade com coluna de "data de validade":** Rejeitada. Quebra o princípio da imutabilidade do modelo *append-only* no Firestore e compromete auditorias.

## Consequências
- **Fica mais fácil:** Comprovar a entidades auditoras a origem matemática de cada cálculo emitido num PDF do ano anterior.
- **Fica mais difícil:** A estrutura da base de dados cresce continuamente, uma vez que nenhum registo obsoleto é apagado fisicamente.
- **Riscos e mitigação:** Crescimento dos custos de leitura por indexação de várias versões. Mitigado indexando preferencialmente as versões "ativas/padrão".
- **Gatilho de revisão:** Se o limite de armazenamento ou as consultas de catalogação ficarem demasiado lentas devido ao volume histórico.