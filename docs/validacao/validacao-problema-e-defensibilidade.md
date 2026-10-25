# Validação do problema e hipótese de defensibilidade

CarbonTrace · Grupo 05 · AI Lab Makers

Revisão editorial de 23/10/2026. Estrutura, perguntas, tabelas, responsabilidades e protocolo originais recuperados. Ajustes funcionais seguem o [ADR-008](../adr/ADR-008-recorte-crud.md): um funcionário, CRUD com histórico, snapshots e cinco jornadas; sem aprovação/fechamento na aplicação.

**Status: hipótese não validada.** Este documento contém um roteiro de entrevista e evidências secundárias públicas. Por enquanto não realizamos nenhuma entrevista para validá-lo, portanto, estamos utilizando apenas fontes públicas como base, o que não elimina a necessidade de validação.

## 1. Objetivo e aplicação

Investigar se a coleta e a conferência dos consumos e resultados dos Escopos 1 e 2 realmente geram retrabalho recorrente, quanto esse retrabalho custa e se existe orçamento para resolvê-lo. Priorizar responsáveis por ESG/controladoria de redes com 20 a 300 unidades e funcionários que coletam dados. Estes últimos são o único perfil da aplicação e podem operar as unidades da própria empresa. Realizar inicialmente pelo menos 2 entrevistas, preferencialmente em organizações distintas, como investigação exploratória. Essa amostra não permite generalizar para todo o mercado.

As perguntas sobre aprovação e fechamento investigam o processo atual do cliente; não reintroduzem esses fluxos no MVP. A demo cobre somente eletricidade, diesel, gasolina e GLP e não comprova capacidade para toda a faixa de 20 a 300 unidades.

## 2. Roteiro de entrevista: 10 perguntas

### P01. Frequência do fechamento

Nos últimos 12 meses, quantos fechamentos de inventário de emissões vocês fizeram e quantos tiveram atraso, correção ou dificuldade para conferir a origem de um número?

**Registrar:** número de fechamentos e de fechamentos afetados, inclusive zero; distinguir fechamento mensal, anual e ausência de inventário. Se a pessoa acompanhou menos de 12 meses, registrar a janela efetivamente conhecida.

### P02. Frequência e gravidade do problema

Qual foi o último caso de dado incorreto ou não rastreável, quantas unidades foram afetadas e quantas vezes um caso parecido ocorreu nos últimos três fechamentos?

**Registrar:** tipo de erro, unidades afetadas, ocorrências e consequência concreta, como dias de atraso ou reenvio de relatório. Se não houver caso, registrar ausência de ocorrência, sem pedir que a pessoa imagine um.

### P03. Solução atual

Como o dado sai da unidade e chega ao relatório aprovado hoje, quais ferramentas são usadas e como vocês guardam o valor anterior quando há uma correção?

**Registrar:** etapas, ferramentas, papéis de quem informa e aprova e existência de histórico. Não pedir nomes, credenciais ou documentos internos.

### P04. Esforço para resolver

No último fechamento concluído, quantas pessoas participaram e quantas horas de trabalho foram gastas na coleta, conferência e correção dos dados?

**Registrar:** horas-pessoa por etapa, com estimativa mínima e máxima quando não houver medição; separar o trabalho normal do retrabalho. Não somar horas de pessoas já incluídas no total informado.

### P05. Custo atual

Quanto a organização gastou no último mês ou ano com ferramentas, consultoria e retrabalho desse processo, em reais, e qual parte desse valor você consegue informar como custo agregado?

**Registrar:** valores ou faixas, período, componentes incluídos e base da estimativa. Aceitar “não informado”; não pedir remuneração pessoal e não converter horas em reais sem um custo agregado fornecido pela pessoa.

### P06. Pressão e consequência

Nos últimos 12 meses, quantas solicitações de clientes, bancos, franqueadora ou auditoria exigiram explicar um número de emissões e qual foi o tempo de resposta ou a consequência de não responder?

**Registrar:** quantidade, categoria de solicitante, tempo e consequência observada; sem nomes de instituições. Separar exigência contratual, solicitação voluntária e percepção de obrigação legal.

### P07. Tentativas anteriores

O que vocês já tentaram mudar ou contratar para resolver esse problema, quanto custou essa tentativa e por que continuaram ou deixaram de usar a solução?

**Registrar:** alternativa usada, custo quando conhecido, resultado e motivo de adoção ou abandono; “nunca tentamos” é uma resposta válida. Distinguir experiência direta de opinião sobre uma ferramenta nunca utilizada.

### P08. Decisão e orçamento

Quais funções avaliam, aprovam o orçamento e autorizam a compra de uma ferramenta desse tipo, de qual orçamento sairia o pagamento e quanto tempo levou uma contratação semelhante?

**Registrar:** funções, área orçamentária e prazo em dias ou meses; não registrar nomes. Identificar se a pessoa responde como usuária, influenciadora ou aprovadora e se conhece o processo ou está estimando.

### P09. Critério de adoção

Que resultado mensurável e quais condições mínimas fariam vocês testar uma nova ferramenta, considerando horas de retrabalho, tempo para rastrear um número e esforço para começar a usar?

**Registrar:** limiar esperado, duração aceitável do teste e impedimentos concretos. Se a adoção depender de OCR, ERP, anexos ou outro item Won't, registrar a incompatibilidade com o MVP, sem prometer sua implementação.

### P10. Disposição de pagar

Considerando somente uma aplicação web com um perfil de funcionário, coleta manual por unidade, correção/cancelamento com histórico, cálculo versionado de eletricidade/diesel/gasolina/GLP, dashboard/ranking/mediana e exportação PDF/CSV, sem aprovação ou fechamento, qual valor total mensal em reais vocês considerariam pagar, para qual faixa de unidades, e acima de qual valor a contratação deixaria de fazer sentido?

**Registrar:** valor mensal total, teto, faixa de unidades e eventual custo de implantação aceito; zero e “não sei” são respostas válidas. Registrar se existe orçamento disponível e autoridade para confirmar o valor. A resposta é uma declaração hipotética, não receita, compromisso ou preço validado.

## 3. Evidências públicas e hipóteses a investigar

Os registros abaixo recuperam as evidências secundárias e os limites de inferência documentados antes da revisão de escopo. Seguem como **hipótese não validada com usuários**. A restauração editorial não constitui nova verificação de vigência normativa, oferta comercial ou representatividade; conferir a fonte/data antes de usá-los como fundamento atual.

| ID e fonte | Evidência secundária | Hipótese e limite da inferência | Perguntas relacionadas |
| --- | --- | --- | --- |
| F01. [GHG Protocol, Corporate Standard](https://ghgprotocol.org/corporate-standard) | O padrão orienta a preparação de inventários corporativos e busca melhorar consistência e transparência do relato. | **Hipótese não validada:** responsáveis pelo fechamento precisam de versões e referências para reproduzir os números. A orientação metodológica não mede frequência de erros, custo ou intenção de compra. | P01, P02, P03, P06 |
| F02. [CDP, Charting the Change: Disclosure Data Dashboard, 14/01/2025](https://www.cdp.net/en/insights/disclosure-data-dashboard-2024) | O relatório sobre 2024 situa varejo entre os setores com maior aumento de divulgação e informa mais de 12.500 empresas respondentes ao questionário para pequenas e médias empresas. | **Hipótese não validada:** solicitações de informação ambiental podem gerar trabalho recorrente no segmento. A amostra é internacional e não representa especificamente redes brasileiras de 20 a 300 unidades; não revela custo nem disposição de pagar pela CarbonTrace. | P01, P04, P05, P06 |
| F03. [Resolução CVM 244, de 29/05/2026](https://conteudo.cvm.gov.br/export/sites/cvm/legislacao/resolucoes/anexos/200/resol244.pdf) e [explicação oficial da CVM](https://www.gov.br/cvm/pt-br/assuntos/noticias/2026/cvm-altera-resolucao-193-para-revogar-obrigatoriedade-da-divulgacao-de-informacoes-financeiras-relacionadas-a-sustentabilidade) | A CVM revogou a obrigatoriedade de divulgação antes prevista na Resolução 193 e alterou o regime de adoção voluntária. | **Hipótese não validada:** a demanda comercial pode depender mais de pedidos de clientes, bancos e franqueadoras do que daquela obrigatoriedade. A norma não estabelece obrigação universal de inventário mensal para varejistas; a pesquisa não presume que todos os entrevistados estejam sujeitos à CVM. | P06, P08 |
| F04. [IBM Envizi, Emissions Management](https://www.ibm.com/products/envizi/emissions-management) | A oferta pública inclui cálculos de Escopos 1, 2 e 3, biblioteca de fatores, API, opção em Excel e cálculos transparentes para apoiar auditoria. | **Hipótese não validada:** pode haver espaço para um fluxo mais restrito e adequado à coleta nas unidades. A descrição é do fornecedor, não uma avaliação independente nem prova de preço, facilidade de uso ou deficiência do concorrente. | P03, P07, P09, P10 |

A análise competitiva acima avalia a oferta pública, sem fabricar reclamações, depoimentos ou alegações de superioridade. F04 é evidência contrária à ideia de que cálculo e auditabilidade, isoladamente, sejam diferenciais exclusivos.

## 4. Hipótese de defensibilidade

**Status: hipótese não validada.**

A vantagem que pretendemos construir é conhecer os erros recorrentes da coleta, correção e conferência de emissões no varejo e já ter verificações prontas para eles. Isso pode reduzir o trabalho necessário para atender uma nova rede com problemas semelhantes.

Por exemplo: uma unidade informa MWh e outra informa kWh; uma correção altera um mês já calculado; um fator novo passa a valer, mas o snapshot e o relatório anteriores precisam continuar reproduzíveis. Para cada situação, a equipe deve registrar a regra, criar um exemplo sintético com resultado esperado e automatizar sua verificação. Esse conjunto será ampliado a partir das entrevistas e, futuramente, dos pilotos, sem guardar dados pessoais ou documentos internos dos clientes.

**Por que um concorrente não nos copia amanhã?** Nossa hipótese é que copiar a interface e o cálculo seja possível, mas reunir e validar o conhecimento dos erros de fechamento das redes de varejo leve tempo, permitindo à CarbonTrace atender novos casos com menos retrabalho.

Hoje, essa vantagem ainda não existe de forma comprovada. Os casos já descritos no backlog são o ponto de partida. Para sustentar a hipótese, precisamos mostrar que a equipe consegue transformar um problema observado em uma verificação reutilizável. Se os mesmos casos puderem ser atendidos com esforço equivalente por outra solução, não haverá evidência de vantagem nessa dimensão.

### 4.1 O que construir

Criar um catálogo de casos de coleta, correção e cálculo, inicialmente limitado às funcionalidades Must do recorte aprovado. Cada caso deve conter:

| Campo | Conteúdo obrigatório |
| --- | --- |
| ID | Código único, como VAL-01 |
| Origem | Pergunta e código da entrevista consentida, ou referência pública e seção; não registrar a identidade da pessoa |
| Problema | Descrição objetiva do erro e da etapa em que ocorre |
| Entrada sintética | Valores, unidades, competência e versões necessárias para reproduzir a situação |
| Resultado esperado | Número exato ou rejeição esperada, incluindo o que deve permanecer inalterado |
| Teste automatizado | Referência ao teste e à história Must correspondente |
| Revisão | Registro de conferência do resultado esperado por alguém diferente de quem implementou o teste |

Um problema relatado em entrevista só entra no catálogo após consentimento e sem dados identificáveis. Quando a origem for documental, marcar o caso como **hipótese não validada com usuários**. Não contar mudanças de valores de um mesmo exemplo como novos tipos de problema.

### 4.2 Métrica da hipótese

**Métrica: tempo para incorporar um novo caso de coleta, correção ou cálculo.** Contar as horas úteis entre a entrega de um caso completo à equipe, com entradas e resultado esperado revisado, e a inclusão de um teste automatizado aprovado, com eventual correção da aplicação e execução dos testes existentes sem novas falhas.

**Meta proposta para o Demo Day:** incorporar 2 casos adicionais, cada um em até 8 horas úteis de trabalho da equipe, dentro de no máximo 2 dias úteis após o recebimento. Somar as horas de todas as pessoas envolvidas; não confundir esforço com prazo decorrido. Registrar os horários, o esforço, a versão do código e o resultado da execução. Casos que já funcionem também exigem um teste que demonstre esse comportamento.

Os 2 casos devem tratar condições ainda não cobertas pelo catálogo, dentro do Must. DevOps + QA prepara os casos e o Tech Lead confere os resultados esperados antes de entregá-los a Backend e Frontend. Quem preparar ou revisar o caso não deve ter implementado a funcionalidade avaliada; se houver esse conflito, o Tech Lead deve redistribuir a revisão para outro integrante tecnicamente apto e registrar a escolha. Se os implementadores já conheciam o caso ou se ele apenas troca os números de um teste existente, ele não atende ao exercício. Se o problema exigir uma funcionalidade Won't, o PO registra a incompatibilidade e solicita outro caso antes de iniciar a medição.

A meta é uma proposta de avaliação, não um resultado alcançado. Sua descrição original é preservada como avaliação exploratória de defensibilidade, sem se tornar gate adicional do CP2 ou exigir recurso fora do recorte. A equipe precisa confirmar capacidade e registrar execução ou não execução; a restauração editorial de 23/10 não reatribuiu tarefas; a priorização de 25/10 consolida 10 issues ativas de J1-J3, sem antecipar esta métrica exploratória. Atingi-la demonstra capacidade inicial de incorporar novos problemas. Para demonstrar vantagem competitiva depois do MVP, será necessário comparar o esforço com uma alternativa capaz de executar os mesmos casos e acompanhar se o catálogo reduz o esforço nos pilotos seguintes.

### 4.3 Protocolo de demonstração

No Demo Day, vale mostrar uma correção que preserva o relatório antigo, um recálculo que produz o mesmo resultado e a evidência de que 2 casos novos foram incorporados dentro da meta. A incorporação dos casos acontece antes do evento; não será necessário programar durante a apresentação.

#### Antes do Demo Day

As responsabilidades abaixo seguem as funções do grupo dos seguintes integrantes do grupo:

| Função | Integrante |
| --- | --- |
| Product Owner (PO) | [Thomas Augusto Amorim de Araujo](https://github.com/Thomas4ugust0) |
| Tech Lead | [Luis Davi Pontes da Silva](https://github.com/pontesluis) |
| Frontend | [Heitor Gomes Monteiro](https://github.com/heitormontt) |
| Backend | [Felipe Couto Duque](https://github.com/felipecduque7) |
| DevOps + QA | [Felipe Moreira da Silva Vieira](https://github.com/Bipelipe) |

| Responsável | Ação | Entrega verificável |
| --- | --- | --- |
| Product Owner (PO) | Realizar as 2 entrevistas previstas, obter consentimento, selecionar problemas e confirmar que estão no Must | Síntese sem identificação pessoal e origem dos problemas; se não houver entrevistas, declarar zero e usar apenas hipóteses documentais |
| Tech Lead | Conferir as regras de cálculo e os resultados esperados de D01 a D07; revisar as alterações de Backend e Frontend e resolver dúvidas técnicas | Casos revisados, decisões técnicas registradas e revisão das alterações, respeitando a separação entre autor e revisor |
| Backend | Implementar os testes de cálculo, validação na API, fatores, revisões e consolidação de D01 a D07; corrigir o comportamento quando necessário | Testes automatizados e massas sintéticas com versão do código identificada |
| Frontend | Preparar formulários, mensagens, correção/cancelamento, consulta de snapshots e origem dos cálculos | Fluxo D04 navegável, versões anterior/atual visíveis; medir a meta exploratória de origem em 60 segundos e 3 cliques |
| DevOps + QA | Preparar os 2 casos adicionais e obter a revisão do Tech Lead antes de entregá-los aos implementadores, reservando os 2 dias úteis de execução até 04/12/2026 | Entradas, resultados esperados, origem, revisão e momento de entrega de cada caso |
| Backend e Frontend | Incorporar os casos adicionais nas respectivas camadas; registrar o esforço individual e executar os testes aplicáveis | Correções e testes necessários, horas-pessoa e prazo decorrido por caso |
| DevOps + QA | Preparar o ambiente e a execução no CI; executar a suíte completa e o fluxo de interface; guardar resultados e falhas | Ambiente reproduzível, versão implantada, relatório de testes e evidências dos tempos e cliques |
| Product Owner (PO), com DevOps + QA | Consolidar o esforço de todos os envolvidos e comparar os resultados com os critérios definidos | Quadro de aprovação ou reprovação dos casos e da meta de esforço e prazo |

Os fatores abaixo são fictícios e servem somente para testar o sistema. Em cada cenário, usar dados isolados. Um funcionário opera as unidades da própria empresa, sem aprovação, fechamento ou reabertura; CT-M06 está adiada. Snapshots/revisões preservam o anterior e fatores são preparados pela equipe. Usar outra empresa para testar isolamento.

| Teste | O que fazer | Resultado obrigatório |
| --- | --- | --- |
| D01. Conversão | Calcular separadamente 1 MWh e 1.000 kWh com fator de 0,5 kgCO2e/kWh | Cada cálculo retorna exatamente 500 kgCO2e |
| D02. Entrada inválida | Enviar um consumo de eletricidade em litros | Rejeitar a entrada, explicar a incompatibilidade e persistir zero lançamentos válidos |
| D03. Fator novo | Salvar snapshot de 100 kWh com A=0,5; equipe prepara B=0,6; recalcular o snapshot original com A | O anterior continua em 50 kgCO2e e referenciando A; repetir com entradas/fator originais e diferença zero |
| D04. Correção retroativa | Salvar 100 kWh com A=0,5; corrigir com motivo para 120 kWh; calcular novo snapshot mantendo A; cancelar e calcular o corrente | Anterior 50 kgCO2e, novo 60, corrente exclui cancelado; revisões/snapshots preservam entrada, autor, data, motivo e fator |
| D05. Consolidação | Consolidar 2 unidades com resultados exatos de 0,1234 e 0,2344 kgCO2e | Total exato de 0,3578 kgCO2e; na apresentação com 3 casas, total de 0,358 e soma das parcelas exibidas de 0,357, com diferença de arredondamento de 0,001 identificada |
| D06. Ranking e mediana | Comparar unidades ativas/completas com totais 100/100/200; testar ±50%, mediana zero, menos de 3 e pendências | Mediana 100 e alerta somente em 200; igualdade a 50% não alerta; insuficiência/zero sem classificação; incompletas fora da comparação |
| D07. PDF/CSV | Exportar o mesmo snapshot A após B e correção/cancelamento | Mesma origem e valores A nos dois formatos, com revisões/fatores/metodologia/UID/datas e indicação de preliminaridade/pendências |

D01 a D07 verificam o comportamento técnico; quantidade de execuções não substitui cobertura por critério, EP/BVA, contratos e segurança. As metas anteriores de 100 recálculos, 1.000 agregações e 19/20 erros deixam de ser gate obrigatório nesta priorização. A avaliação metodológica original de 10 casos de referência/tolerância de 0,1% permanece proposta adicional a confirmar para fontes compatíveis, sem declarar inventário homologado ou ampliar o catálogo. D01 a D03 pertencem ao núcleo do CP2 J1-J3, junto dos testes de teto/mês/unidade e demais critérios. D04 (correção/cancelamento avançados), D05/D06 (consolidação/mediana) e D07 (exportação) seguem no MVP nas fases posteriores, fora das 10 issues ativas. Registrar verificações executadas e pendentes, sem tratar proposta como resultado.

#### Durante o Demo Day

1. **Product Owner (PO) apresenta o catálogo:** mostrar os 7 cenários técnicos e, se executados, os 2 casos adicionais da métrica, distinguindo origem documental de problema relatado em entrevista. Não apresentar casos sintéticos como relatos reais.
2. **Frontend demonstra D04, com apoio de Backend:** abrir snapshot antigo, corrigir com motivo e mostrar 50 e 60 kgCO2e nas respectivas versões; cancelar e conferir o corrente sem apagar anteriores. Abrir origem pelo dashboard e medir a meta exploratória de 60 segundos/3 cliques. Demonstrar D06 e exportar A em PDF/CSV (D07), sem aprovação/fechamento.
3. **DevOps + QA executa os testes:** exibir o resultado de D01 a D07 e, se executados, dos 2 casos adicionais na versão identificada. Mostrar também as evidências de reprodução e consistência dos cenários definidos, sem presumir contagem fixa como aprovação. Backend explica os resultados de cálculo e o Tech Lead esclarece as decisões técnicas.
4. **Product Owner (PO) apresenta a métrica, conferida por DevOps + QA:** informar, para cada caso adicional, origem, esforço em horas-pessoa, prazo decorrido, teste criado e resultado. Comparar os valores obtidos aos limites de 8 horas úteis de esforço e 2 dias úteis de prazo.
5. **DevOps + QA disponibiliza as evidências:** reunir massas sintéticas, testes, instruções de execução, versão do código e relatório de resultados para reprodução por outra pessoa. O Tech Lead confere as instruções técnicas e o PO verifica que o catálogo não identifica os entrevistados.

#### Como interpretar o resultado

| Resultado observado | Conclusão permitida |
| --- | --- |
| Os 7 cenários técnicos passam | O comportamento técnico demonstrado funciona nesses cenários; isso não comprova defensibilidade |
| Os 2 casos adicionais passam, dentro dos limites de esforço e prazo, sem novas falhas na suíte | A equipe demonstrou a capacidade de incorporar casos dentro da meta proposta |
| Algum caso adicional excede o limite ou introduz uma falha | A meta não foi atingida; registrar a causa e o resultado real antes de corrigir e repetir |
| Todos os casos têm origem apenas documental | A demonstração é técnica; a relevância dos problemas para os usuários continua não validada |
| Não há comparação equivalente com outra solução | Não afirmar que a CarbonTrace é mais rápida de adaptar ou difícil de copiar |

## 5. Conclusão

A proposta é construir conhecimento reutilizável sobre erros de coleta, correção e cálculo no varejo e transformá-lo em verificações que reduzam o trabalho de atender novos casos. Preservam-se as 2 entrevistas consentidas, os cenários técnicos e a descrição da métrica exploratória de 2 casos adicionais/8 horas/2 dias úteis, com execução ou pendência declarada. Até o Demo Day, apresentar evidências das cinco jornadas e registrar separadamente a avaliação de aprendizado, sem ampliar o recorte funcional ou contar J5 como entregue no CP2. Demanda, disposição de pagar e vantagem sobre concorrentes continuam como **hipótese não validada** enquanto não houver evidência específica.
