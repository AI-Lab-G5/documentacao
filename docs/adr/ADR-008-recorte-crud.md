# ADR-008: CRUD com requisitos originais preservados

## Status

Decisão de produto aprovada pelo responsável em 23/10/2026; justificativas ampliadas nesta revisão editorial. Implementação pendente nas issues; documentação não comprova aceite técnico ou aprovação do tutor. Revisa parcialmente ADR-001 a ADR-007 e mantém a stack.

## Contexto

A proposta original pede coleta por unidade, fatores versionados, consolidação e relatórios rastreáveis. A visão evoluiu para quatro perfis, aprovação segregada, fechamento/reabertura e infraestrutura de auditoria, aumentando telas, permissões, estados e testes para uma equipe com prazo curto.

Reduzir tudo a CRUD puro resolveria a coleta, mas retiraria versionamento, comparação da rede e exportações. O projeto deixaria de demonstrar recalcular com o fator original e comparar unidades com a mediana. A redução precisa atingir a operação, mantendo os requisitos que dão sentido à proposta.

O usuário operacional é o funcionário que coleta dados; ESG/controladoria podem continuar como comprador/interessados sem exigir perfis diferentes no software. CRUD, precisão e rastreabilidade precisam formar um fluxo completo, sem se confundir com certificação ou inventário homologado.

## Decisão

Manter cinco jornadas: J1 unidades/fontes; J2 coleta; J3 cálculo versionado/reprodução; J4 dashboard/ranking/mediana; J5 PDF/CSV rastreável. Um perfil funcionario, login individual e acesso às unidades da própria empresa. Equipe provisiona contas e prepara fatores, sem gestão pela interface.

Fontes: eletricidade, diesel, gasolina e GLP. Conversão kWh/MWh, demais L/L/kg. Decimal, strings e exibição 3 casas ROUND_HALF_EVEN. Correção cria revisão; cancelamento lógico preserva origem e retira do corrente. Snapshots guardam entradas, fontes esperadas, conversões, fatores/metodologia e autor/data; A permanece após B ou correção.

Comparação absoluta por kgCO2e, com unidades ativas/completas e ≥1 fonte esperada. Alerta abs(desvio)>50% com ≥3 completas e mediana>0; fora disso, aviso sem classificação. Não alegar eficiência ou teste estatístico.

PDF/CSV síncronos do mesmo snapshot, com revisões/fatores/UID/datas, sem fechamento, jobs, Storage obrigatório, assinatura ou manifesto. Fatores não conferidos continuam preliminares. Metodologia fixa explicita os limites dos fatores agregados.

Alternativas descartadas:
- **Manter integralmente quatro perfis e fechamento:** aumenta o trabalho de fluxos e autorização sem ser indispensável à coleta e à cena original.
- **CRUD sem versões, rede ou relatórios:** entrega algo menor, mas não preserva as cinco jornadas nem a demonstração solicitada.
- **Apagar histórico para simplificar:** impede reproduzir o anterior e explicar correções; cancelar logicamente resolve a retirada do corrente.
- **Manter infraestrutura criptográfica como obrigatória:** confunde a trilha mínima com uma solução de auditoria mais ampla; referências e snapshots bastam ao recorte.
- **Trocar a stack:** consome capacidade sem resolver a origem da complexidade.

## Consequências

- **Fica mais fácil:** Reduzir telas, perfis e transições, reaproveitando código e mantendo a proposta de coleta/cálculo/rede.
- **Fica mais difícil:** O projeto ainda precisa implementar histórico, fatores/snapshots, comparação e PDF/CSV; não é apenas um formulário e não se garante prazo sem avaliar capacidade.
- **Riscos e mitigação:** Documentos podem reintroduzir funções adiadas ou afirmar que o alvo está implementado. Usar backlog/IDs, contrato-alvo e matriz, mantendo resultados pendentes até testes/PR/CI reais.
- **Gatilho de revisão:** Novo requisito de perfil/fonte/metodologia, evidência de adoção incompatível ou capacidade/custo insuficientes. Registrar decisão antes de ampliar o escopo.

## Cortes e correspondência

Fora do MVP: quatro perfis/gestão de permissões; aprovação; fechamento/reabertura; auditor convidado; refrigerantes; intensidade/MAD; escolha GWP; hashes/assinatura/infraestrutura separada de auditoria. Excluem-se também Escopo 3, OCR, concessionárias, ERP, anexos, certificação, créditos, app nativo e benchmark entre clientes.

CT-M06 fica adiada e reservada. Comparação básica sai da parte avançada de CT-S02 e passa a CT-M12; cancelamento é CT-M11. IDs equivalentes permanecem, sem reutilização silenciosa. Ver [backlog](../backlog/MoSCoW.md). Textos anteriores continuam no histórico Git.

O Ishikawa permanece contexto, sem obrigar tratar todas as causas. A visão de produto e a validação mantêm a estrutura original e a informação de pesquisa; trechos de aprovação/fechamento são contexto do cliente ou itens adiados, não funcionalidades restabelecidas.

## Entregas e verificação

Repriorização aprovada em 25/10/2026 conforme o Guia para os POs: Sprint 2 (19 a 25/10), J1/J2 e base local; Sprint 3 (26/10 a 01/11), J3 e provas/aceite de J1-J3. São 10 issues ativas, 5 por sprint e uma por integrante. J4/J5 continuam Must do MVP para as semanas 5 a 8; não são dependências do CP2. As antigas issues são encerradas administrativamente como consolidadas/adiadas, retiradas das milestones e ligadas às substitutas, sem representar implementação concluída. Datas, responsáveis das ativas e Sprint 1 são preservados.

EP/BVA e matriz pertencem ao QA; PO registra refinamentos S3/S4 e confere aceite/evidências. Backend implementa domínio/tokens, TL coordena/revisa e DevOps configura ambiente/CI e produz prints de custos/calculadora. Cada critério entregue tem teste; 100 recálculos, 1.000 agregações e 19/20 erros deixam de ser contagens obrigatórias. Revisão concorrente, cancelamento com chave reservada e janela de 24 horas ficam para evolução quando necessários, sem excluir os objetivos do MVP. Esta é repriorização de entrega, não retorno aos quatro perfis ou remoção das cinco jornadas.

[OpenAPI 0.2.0](../api/openapi.yaml) é contrato-alvo, distinto do código/contrato gerado atual de CarbonTrace. Esta revisão modifica documentacao, não implementa aquele produto.

Manter exigências do caderno: segurança/deny-all, EP/BVA, contratos, evidências, custos e marcos de operação/qualidade/rollback. Apresentar revisão ao tutor; não dispensar gate por decisão unilateral de produto. Métricas exploratórias de pesquisa/defensibilidade são identificadas como propostas, sem resultados inventados ou novas funcionalidades.

GCP/Firebase mantidos. US$3 a US$8 é estimativa; CP2 ≤US$5, CP3 ≤US$12, ciclo ≤US$50 e ação a US$40 exigem gasto real comprovado. Workflow e CODEOWNERS permanecem inalterados.
