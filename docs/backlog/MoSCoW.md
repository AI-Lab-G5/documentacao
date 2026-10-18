# Backlog MoSCoW - CarbonTrace

AI Lab Makers · Maker Foundation · G5

Este backlog detalha a [visão do produto](../visao-de-produto.md), especialmente as seções 2.1 e 2.2, considerando também o texto de visão anexado e o diagrama de Ishikawa. O recorte é o MVP de inventário mensal de Escopos 1 e 2 para redes de varejo e franquias. Os anexos são fontes de contexto do produto.

## Como usar

- **Must:** obrigatório para entregar o MVP e suas cinco jornadas básicas.
- **Should:** importante, mas pode ser adiado sem impedir a entrega básica.
- **Could:** opcional, executado somente após Must e Should, se houver capacidade.
- **Won't:** decisão explícita de não implementar neste MVP; não significa exclusão permanente.

Cada história possui ID estável e critérios de aceite numerados. Todos os critérios de uma história incluída precisam passar. Para Won't, os critérios verificam a exclusão, não autorizam implementar a funcionalidade desejada.

**Convenções de teste e decisões propostas neste backlog:** usar dados sintéticos; os números de fatores nos exemplos são fictícios e não servem para inventários reais. Adotar decimal exato no cálculo, kgCO2e como unidade de comparação e arredondamento apenas na apresentação. A regra proposta para exibição é 3 casas decimais, com empate arredondado para cima. Testes de p95 usam 100 operações após 10 de aquecimento, na mesma configuração de ambiente documentada, com 100 unidades cadastradas e 1 usuário simultâneo; p95 é o 95º tempo da amostra ordenada. Isso é uma base de homologação, não uma promessa de capacidade para 300 unidades concorrentes.

Limites e exemplos operacionais não definidos na visão - política de justificativa, tratamento de MAD zero, prazo de acesso temporário e limite de importação - são propostas para tornar o backlog testável. A tolerância de referência de 0,1% também permanece proposta, conforme a visão, e deve constar da ficha metodológica antes da homologação. Não representam funcionalidades já implementadas.

## Must

### CT-M01 - Cadastrar unidades e fontes (jornada 1)

**História:** Como gerente de ESG da matriz, quero cadastrar unidades e suas fontes de emissão, para organizar a coleta mensal da rede.

**Critérios de aceite:**
1. Cadastrar 1 unidade com código, nome, vínculo própria/franqueada, área em m² maior que zero e 3 fontes ativas em até 3 minutos, medidos do início do formulário à confirmação, por cada um de 3 participantes.
2. Disponibilizar as 4 famílias de fontes previstas: eletricidade comprada por localização, combustíveis de frota própria, combustíveis estacionários e refrigerantes; eletricidade pertence ao Escopo 2 e as outras 3 ao Escopo 1.
3. Repetir o código dentro do mesmo cliente retorna HTTP 409 e mantém exatamente 1 unidade com esse código; o mesmo código em outro cliente não constitui duplicidade.

### CT-M02 - Controlar acesso e isolamento

**História:** Como gerente de ESG da matriz, quero restringir o acesso por cliente e unidade, para proteger os dados da minha rede.

**Critérios de aceite:**
1. Em 100% das rotas protegidas, uma requisição sem autenticação retorna HTTP 401; tentativas de acessar recursos de outro cliente retornam 403 ou 404 e zero dados desse cliente.
2. Um gerente de unidade acessa apenas sua unidade; em uma base com 2 clientes e 2 unidades por cliente, todas as tentativas de ler ou alterar as outras 3 unidades são negadas, inclusive pelo acesso direto ao Firestore.
3. ESG e controladoria acessam as unidades do próprio cliente; apenas ESG pode cadastrar unidades, publicar fatores e reabrir períodos. Cada operação negada deixa zero alterações persistidas.

### CT-M03 - Registrar dados mensais (jornada 2)

**História:** Como gerente de unidade, quero registrar os consumos mensais em um formulário guiado, para informar dados válidos sem depender de planilhas.

**Critérios de aceite:**
1. Cada um de 3 participantes registra 4 fontes em até 5 minutos, do início do formulário à confirmação do último registro.
2. Valores negativos, campos obrigatórios ausentes e unidades incompatíveis são bloqueados na interface e na API, com zero lançamentos persistidos. Zero é aceito quando informado explicitamente; ausência nunca é convertida em zero.
3. Em um conjunto de 20 erros de unidade identificáveis pela informação fornecida, pelo menos 19 são sinalizados antes da gravação. O teste não presume detectar digitação equivocada quando o usuário declara uma unidade válida.
4. Todo lançamento salvo contém cliente, unidade, fonte, competência YYYY-MM, valor, unidade de medida, identificador opaco do autor e data/hora do registro; reenviar a mesma requisição com a mesma chave de idempotência mantém 1 lançamento.

### CT-M04 - Publicar fatores imutáveis

**História:** Como gerente de ESG da matriz, quero publicar versões de fatores de emissão, para preservar a origem e a validade dos cálculos.

**Critérios de aceite:**
1. Cada versão publicada possui ID, fonte documental, data de publicação, início e fim de vigência, unidade do fator, gases aplicáveis e versão de GWP; a ausência de qualquer campo obrigatório impede a publicação.
2. Para combustão, registrar CO2 fóssil, CO2 biogênico, CH4 e N2O com valores ou indicação explícita de não aplicabilidade; para refrigerantes, registrar o gás e o GWP aplicável. Zero campos aplicáveis podem ficar sem definição.
3. Tentar editar ou excluir uma versão publicada é rejeitado e preserva 100% de seu conteúdo. Corrigi-la cria um novo ID com referência à versão anterior e motivo não vazio.
4. Publicar a versão B não altera nenhum resultado já calculado com A. Uma entrada sem fator compatível com fonte, unidade e competência fica sem resultado e apresenta erro identificando a incompatibilidade.

### CT-M05 - Calcular e reproduzir emissões (jornada 3)

**História:** Como analista de controladoria, quero calcular emissões com versões fixas de dados, fatores e metodologia, para reproduzir os números de um período.

**Critérios de aceite:**
1. Com fator sintético de 0,5 kgCO2e/kWh, entradas de 1 MWh e 1.000 kWh resultam, cada uma, em exatamente 500 kgCO2e. Conversões incompatíveis geram zero resultados válidos.
2. Executar 100 recálculos com as mesmas revisões de entrada, fator e metodologia produz diferença numérica zero e o mesmo hash de entradas; publicar um novo fator entre execuções não altera esse resultado.
3. Todo resultado referencia as versões de entrada, fator, GWP e metodologia e seu recibo; CO2 biogênico é apresentado separadamente, conforme a ficha metodológica, sem inclusão silenciosa no total fóssil.
4. Os 10 casos de referência da ficha metodológica, com as mesmas premissas da ferramenta do Programa Brasileiro GHG Protocol, têm erro relativo de no máximo 0,1%; quando a referência é zero, o resultado deve ser exatamente zero. Registrar a versão da ferramenta e os resultados esperados no conjunto de testes.
5. O cálculo de 1 unidade com 4 fontes apresenta p95 de até 1,5 segundo nas condições de homologação descritas acima.

### CT-M06 - Revisar e fechar períodos

**História:** Como analista de controladoria, quero revisar e fechar a competência mensal, para identificar quais dados foram aprovados e publicados.

**Critérios de aceite:**
1. Permitir as transições aberto → em revisão → fechado → reaberto → em revisão; todas as demais transições são rejeitadas sem alteração do estado.
2. A entrada em revisão exige 1 lançamento válido por fonte ativa na competência em cada unidade incluída; havendo 1 pendência, a operação é bloqueada e lista a unidade e a fonte faltantes.
3. O fechamento exige aprovação de 100% dos lançamentos incluídos e cálculo sem erros. Cada aprovação registra aprovador e instante; a pessoa que informou o lançamento não pode aprová-lo.
4. Fechar cria 1 versão imutável do consolidado com os IDs das revisões usadas. Tentativas de lançar ou alterar dados em período em revisão ou fechado são rejeitadas; somente aberto ou reaberto aceita novas revisões.

### CT-M07 - Corrigir sem perder o histórico

**História:** Como gerente de ESG da matriz, quero reabrir um período e registrar correções como novas revisões, para corrigir números mantendo o que foi reportado anteriormente.

**Critérios de aceite:**
1. Reabrir exige usuário ESG e motivo com pelo menos 1 caractere não branco; cada reabertura registra autor, instante e versão fechada anterior.
2. Corrigir um valor de 100 para 120 cria exatamente 2 revisões consultáveis, ligadas entre si, com competência, instante de registro, autor e motivo; a primeira continua contendo 100.
3. Após novo fechamento, consultar a versão anterior retorna 100 e a nova retorna 120 no exemplo de fator unitário. Uma consulta pela data de publicação retorna a última versão fechada até aquele instante, ou informa zero versões publicadas se nenhuma existia.
4. Uma correção altera os agregados apenas da nova revisão; repetir seu processamento 2 vezes não duplica o valor consolidado.

### CT-M08 - Consolidar o dashboard básico (jornada 4)

**História:** Como gerente de ESG da matriz, quero consultar os totais mensais por unidade e escopo, para acompanhar o inventário consolidado da rede.

**Critérios de aceite:**
1. Em 1.000 cenários gerados com semente registrada, incluindo frações decimais e revisões, a soma exata das unidades é igual ao total da rede, com diferença zero. Usar apenas 1 revisão selecionada de cada lançamento.
2. O dashboard distingue pelo menos competência, versão do fechamento, Escopo 1 e Escopo 2; escolher uma competência ou versão altera todos os totais da tela para o mesmo recorte, sem misturar versões.
3. Para 100 unidades, a consulta do dashboard apresenta p95 de até 800 ms. Dados incompletos aparecem como pendência, sem serem convertidos em emissão zero.
4. O total é arredondado depois da soma exata. Se a soma dos valores individuais exibidos diferir do total exibido, apresentar a diferença de arredondamento explicitamente, permitindo reconciliar os valores com diferença zero.

### CT-M09 - Rastrear recibos de cálculo

**História:** Como analista de controladoria, quero abrir o recibo de cada resultado, para explicar sua origem durante uma auditoria.

**Critérios de aceite:**
1. Em 10 resultados sorteados de uma base sintética, rastrear cada resultado até suas entradas em até 60 segundos e no máximo 3 cliques a partir do dashboard.
2. Em 100% dos recibos, exibir valores e unidades de entrada, conversões, fórmula, resultado, IDs de revisões e versões de fator, GWP e metodologia, além do hash SHA-256 das entradas.
3. Uma especificação versionada define ordenação, codificação UTF-8 e representação decimal da entrada canônica; recalcular o hash do mesmo conteúdo retorna os mesmos 64 caracteres hexadecimais. Alterar 1 valor da entrada produz hash diferente no caso de teste.

### CT-M10 - Exportar PDF e CSV (jornada 5)

**História:** Como analista de controladoria, quero exportar um período fechado em PDF e CSV com sua trilha de auditoria, para compartilhar números verificáveis.

**Critérios de aceite:**
1. Para 1 competência com 100 unidades e 4 fontes por unidade, gerar cada formato em até 10 segundos; os 2 arquivos identificam cliente, competência, versão do fechamento, fatores e metodologia.
2. Cada linha detalhada do CSV identifica o recibo correspondente; 100% dos totais do PDF podem ser reconciliados com as linhas do CSV e seus recibos, com diferença zero antes do arredondamento de apresentação.
3. O PDF exibe o SHA-256 dos bytes do CSV final e ambos identificam o mesmo pacote. Um manifesto separado lista os hashes do PDF e do CSV, evitando um hash autorreferente. Alterar 1 byte de qualquer arquivo faz a verificação desse arquivo falhar.
4. Exportar um período sem versão fechada é bloqueado. Exportar novamente uma versão anterior preserva os mesmos valores e referências, mesmo após um novo fechamento.

## Should

### CT-S01 - Explicar a variação entre revisões

**História:** Como analista de controladoria, quero decompor a diferença entre dois fechamentos, para explicar por que o inventário mudou.

**Critérios de aceite:**
1. Apresentar 3 parcelas: efeito de dados, de fator e de metodologia; sua soma é exatamente igual ao total novo menos o antigo.
2. Aplicar a ordem fixa dados → fator → metodologia, exibindo essa convenção. No exemplo de metodologia multiplicativa inalterada, consumo 100 → 120 e fator 2 → 3 geram efeito de dados 40, efeito de fator 120, efeito de metodologia 0 e variação total 160.

### CT-S02 - Comparar intensidades e detectar outliers

**História:** Como gerente de ESG da matriz, quero comparar unidades por intensidade e mediana da rede, para investigar diferenças sem penalizar unidades maiores.

**Critérios de aceite:**
1. Calcular intensidade em kgCO2e/m²; 100 kgCO2e em 10 m² e 200 kgCO2e em 20 m² resultam em 10 kgCO2e/m² e empatam no ranking. Área ausente ou igual a zero exclui a unidade do ranking e gera aviso.
2. Calcular mediana e MAD das intensidades do recorte; com quantidade par, usar a média dos 2 valores centrais. Aplicar z modificado = 0,6745 × (intensidade − mediana) / MAD e alertar quando o valor absoluto for maior que 3,5; exatamente 3,5 não alerta.
3. Com menos de 3 unidades válidas ou MAD igual a zero, exibir “base insuficiente para outliers”, sem dividir por zero. Cada alerta apresenta intensidade, mediana, escore e limiar.

### CT-S03 - Identificar a qualidade e a plausibilidade dos dados

**História:** Como gerente de unidade, quero identificar a qualidade do dado e justificar desvios históricos, para ajudar a matriz a avaliar sua confiabilidade.

**Critérios de aceite:**
1. Cada lançamento recebe exatamente 1 dos 3 rótulos: medido, estimado ou padrão; estimado e padrão exigem justificativa com pelo menos 1 caractere não branco.
2. Comparar a entrada aos últimos 6 meses anteriores com dados da mesma unidade e fonte, convertidos à mesma unidade canônica. Havendo pelo menos 3 meses e MAD maior que zero, exigir justificativa quando o valor absoluto do z modificado superar 3,5; usar a fórmula de CT-S02.
3. Com menos de 3 meses ou MAD zero, mostrar “histórico insuficiente”, permitir registro com as validações básicas e não afirmar ausência de anomalia. Dashboard e exportações mostram a contagem de cada rótulo e o percentual de lançamentos estimados sobre o total válido; com zero lançamentos, mostram “sem dados”.

### CT-S04 - Importar unidades por CSV

**História:** Como gerente de ESG da matriz, quero importar o cadastro de unidades por CSV, para reduzir o trabalho de configuração inicial.

**Critérios de aceite:**
1. Um arquivo com 100 unidades válidas cria exatamente 100 cadastros seguindo as mesmas validações de CT-M01; arquivos com mais de 300 linhas de dados são rejeitados antes da gravação.
2. Em arquivo com 1 código duplicado ou área inválida, informar linha e campo do erro e criar zero unidades, permitindo correção e reenvio sem importação parcial.

## Could

### CT-C01 - Conceder consulta temporária ao auditor

**História:** Como auditor, quero acesso temporário somente leitura, para conferir o inventário sem alterar os registros.

**Critérios de aceite:**
1. Um acesso concedido por ESG por 7 dias permite consultar apenas o cliente autorizado; no instante de expiração e depois dele, todas as consultas são negadas.
2. Todas as tentativas de criar, alterar ou excluir dados com esse perfil retornam 403 e zero alterações. A revogação por ESG impede a próxima requisição, mesmo antes dos 7 dias.

### CT-C02 - Rotular vínculo e abordagem de consolidação

**História:** Como gerente de ESG da matriz, quero filtrar unidades por vínculo e registrar a abordagem de consolidação, para contextualizar os limites do inventário.

**Critérios de aceite:**
1. Registrar exatamente 1 das 3 abordagens: controle operacional, controle financeiro ou participação societária; permitir filtrar os 2 vínculos já cadastrados, própria e franqueada.
2. Em uma base com 2 unidades próprias e 3 franqueadas, cada filtro retorna respectivamente 2 e 3 unidades. Um rótulo de possível categoria 14 é apenas informativo, com aviso “Escopo 3 não calculado no MVP”, e adiciona zero emissões de Escopo 3 aos totais.

### CT-C03 - Escolher a versão de GWP por período

**História:** Como gerente de ESG da matriz, quero selecionar AR5 ou AR6 para um período aberto, para comparar resultados com versões explícitas de GWP.

**Critérios de aceite:**
1. Oferecer 2 opções, AR5 e AR6, somente quando existir conjunto compatível de fatores. Um período fechado rejeita troca direta e preserva 100% dos recibos existentes.
2. Para 1 kg de gás sintético com GWP de teste 10 na opção A e 12 na B, obter respectivamente 10 e 12 kgCO2e; os recibos identificam a opção usada. O MVP básico usa 1 versão fixa documentada, mesmo se esta história for adiada.

## Won't - fora deste MVP

As histórias abaixo registram necessidades reconhecidas e deliberadamente excluídas para proteger prazo, orçamento e foco em rastreabilidade. Seus critérios são verificações negativas do escopo entregue.

### CT-W01 - Inventário de Escopo 3

**História:** Como gerente de ESG da matriz, quero calcular as emissões da cadeia de valor, para ampliar a cobertura do inventário.

**Decisão:** excluir cálculo de Escopo 3, inclusive categoria 14; CT-C02 permite apenas contexto e filtros.

**Critério de aceite da exclusão:**
1. Tentar cadastrar um lançamento de Escopo 3 é rejeitado sem persistência; dashboard e exportações contêm zero totais calculados de Escopo 3 e indicam que esse escopo não foi inventariado, sem apresentá-lo como emissão zero.

### CT-W02 - OCR de faturas

**História:** Como gerente de unidade, quero extrair consumos automaticamente de imagens de faturas, para reduzir a digitação.

**Decisão:** excluir OCR; a coleta do MVP é manual e guiada.

**Critério de aceite da exclusão:**
1. O fluxo de coleta oferece zero ações de extração por OCR e o contrato da API expõe zero operações de reconhecimento de faturas.

### CT-W03 - Integração com concessionárias

**História:** Como gerente de unidade, quero sincronizar consumos com concessionárias, para evitar lançamentos manuais recorrentes.

**Decisão:** excluir conectores com concessionárias.

**Critério de aceite da exclusão:**
1. O MVP oferece zero conectores e zero campos para credenciais de concessionárias; executar a coleta de 1 competência dispara zero chamadas a serviços de concessionárias.

### CT-W04 - Certificação de terceira parte

**História:** Como gerente de ESG da matriz, quero obter certificação externa do inventário pela plataforma, para apresentar uma avaliação independente aos clientes.

**Decisão:** excluir certificação; exportar evidências não equivale a certificar.

**Critério de aceite da exclusão:**
1. O MVP emite zero certificados e cada PDF exportado contém 1 aviso de que o relatório não constitui certificação de terceira parte.

### CT-W05 - Créditos de carbono

**História:** Como gerente de ESG da matriz, quero comprar e aplicar créditos de carbono, para acompanhar compensações da rede.

**Decisão:** excluir compra, venda, aposentadoria e compensação por créditos.

**Critério de aceite da exclusão:**
1. A interface e a API oferecem zero operações de créditos; os totais de emissão calculados sofrem zero deduções por compensações.

### CT-W06 - Escopo 2 por escolha de compra

**História:** Como gerente de ESG da matriz, quero calcular Escopo 2 por instrumentos de compra de energia, para avaliar emissões pela abordagem baseada em mercado.

**Decisão:** entregar somente a abordagem por localização.

**Critério de aceite da exclusão:**
1. Em 100% dos resultados de Escopo 2, o método é identificado como localização; solicitar cálculo baseado em mercado retorna erro de método não suportado e zero resultados calculados.

### CT-W07 - Integração com ERP

**História:** Como analista de controladoria, quero importar consumos diretamente do ERP, para aproveitar dados já registrados pela empresa.

**Decisão:** excluir conectores com ERP; CT-S04 cobre somente cadastro de unidades por CSV.

**Critério de aceite da exclusão:**
1. A versão entregue possui zero conectores de ERP e zero rotinas de sincronização com ERP habilitadas.

### CT-W08 - Anexar documentos de origem

**História:** Como gerente de unidade, quero anexar comprovantes aos lançamentos, para reunir documentos de apoio à auditoria.

**Decisão:** excluir upload e armazenamento de anexos; a trilha cobre dados e versões registrados.

**Critério de aceite da exclusão:**
1. O formulário de lançamento oferece zero campos de upload e a API expõe zero operações de anexar comprovantes. CT-S04, se entregue, processa o CSV de unidades sem disponibilizá-lo como anexo de um lançamento.

### CT-W09 - Aplicativo nativo

**História:** Como gerente de unidade, quero usar um aplicativo nativo no celular, para realizar a coleta em um aplicativo instalado.

**Decisão:** entregar aplicação web, sem aplicativos nativos.

**Critério de aceite da exclusão:**
1. O conjunto de entregáveis do MVP contém zero pacotes Android e iOS e zero publicações em lojas de aplicativos.

### CT-W10 - Benchmark entre clientes

**História:** Como gerente de ESG da matriz, quero comparar minha rede com outras empresas, para avaliar meu desempenho relativo no setor.

**Decisão:** excluir comparação entre clientes; as comparações de CT-S02 ficam dentro da própria rede.

**Critério de aceite da exclusão:**
1. Em uma base com 2 clientes, o dashboard, o ranking e as exportações de cada um usam zero registros do outro, inclusive na mediana; existem zero consultas de benchmark entre clientes na API pública.

## Dependências e corte de entrega

| Entrega | Dependências |
| --- | --- |
| Coleta (M03) | Cadastro (M01), autorização (M02) |
| Cálculo (M05) | Coleta (M03), fatores (M04) |
| Fechamento e correção (M06-M07) | Cálculo (M05), autorização (M02) |
| Dashboard e recibos (M08-M09) | Cálculo e versões de fechamento (M05-M07) |
| Exportações (M10) | Fechamento (M06), recibos (M09) |
| Ponte de variação (S01) | Correções (M07) |
| Intensidade e qualidade (S02-S03) | Cadastro, coleta e dashboard (M01, M03, M08) |
| Auditor temporário (C01) | Autorização e recibos (M02, M09) |