# Visão do Produto e Projeto: CarbonTrace

AI Lab Makers · Maker Foundation · G5

## 1 CENÁRIO ATUAL DO CLIENTE E DO NEGÓCIO

### 1.1 Introdução ao Negócio e Contexto

A CarbonTrace é uma proposta de plataforma web para redes de varejo e franquias registrarem consumos mensais por unidade, calcularem emissões de gases de efeito estufa e consolidarem o inventário da rede. O MVP cobre Escopos 1 e 2. O projeto é desenvolvido pelo Grupo 05 no AI Lab Makers, no ciclo Maker Foundation.

O segmento inicial compreende redes com 20 a 300 unidades, com equipe de ESG ou controladoria na matriz e responsáveis pela coleta nas lojas. Supermercados, atacarejos regionais, farmácias, redes de alimentação e lojas de conveniência são setores a investigar. Esse recorte define o público pretendido; a capacidade para atender 300 unidades ainda depende de testes.

O processo considerado começa com o registro de eletricidade, combustíveis e refrigerantes na unidade. A matriz revisa os dados, calcula as emissões e publica o fechamento mensal. Quando um valor precisa ser corrigido, a equipe deve conseguir consultar tanto a revisão atual quanto o número publicado anteriormente. Ainda não há, nos materiais do projeto, um cliente contratado ou resultados de entrevistas que comprovem a frequência desses problemas.

### 1.2 Identificação da Oportunidade ou Problema

O problema investigado é a dificuldade de conferir a origem e reproduzir o cálculo dos números de um inventário consolidado a partir de planilhas de várias unidades. A documentação inicial identifica seis hipóteses para validação com responsáveis pela coleta e pelo fechamento:

1. Valores são registrados em unidades de medida diferentes, como kWh e MWh, sem conversão controlada.
2. Fatores de emissão são substituídos sem preservar a versão usada no fechamento anterior.
3. Correções sobrescrevem o dado original e não registram autor, data e motivo.
4. A soma das unidades diverge do total da rede por cópia manual ou arredondamento em etapas diferentes.
5. Os registros não identificam separadamente quem informou e quem aprovou os dados.
6. A resposta a uma solicitação de auditoria depende de localizar arquivos e reconstruir cálculos manualmente.

A oportunidade consiste em reunir coleta, revisão, cálculo e fechamento em um fluxo com referências persistentes entre o dado informado e o resultado publicado. A redução de retrabalho deverá ser medida em um piloto; não há uma linha de base que permita estimar economia de horas ou de custos neste momento.

A Figura 1 organiza as causas propostas nas categorias de método, máquina, mão de obra, materiais, medida e meio ambiente. O diagrama representa somente hipóteses do projeto, não resultados de uma pesquisa concluída.

![Diagrama de Ishikawa: causas da baixa confiança no inventário consolidado](assets/Diagrama_Ishikawa.png)

*Figura 1. Diagrama de Ishikawa da CarbonTrace, organizado pelos 6M.*

### 1.3 Desafios do Projeto

O desafio técnico principal é manter a reprodução dos cálculos após correções e novas publicações de fatores. Cada resultado deve registrar as revisões de entrada, a conversão de unidades, o fator, o GWP e a metodologia utilizados. Publicar uma versão nova não pode alterar resultados já fechados.

A consolidação exige aritmética decimal e uma regra única de arredondamento. O total deve ser calculado com valores não arredondados; eventuais diferenças entre a soma dos valores exibidos e o total exibido precisam ser identificadas. Correções retroativas devem atualizar uma nova revisão do consolidado sem duplicar lançamentos nem modificar a versão anterior.

A operação depende de pessoas com responsabilidades distintas. O gerente da unidade precisa informar os dados sem conhecer os detalhes do cálculo de carbono. A controladoria precisa revisar pendências e aprovar os lançamentos. O backlog exige que o aprovador seja diferente do autor do lançamento, portanto o piloto precisa contar com pelo menos duas pessoas para completar esse fluxo.

O prazo termina em 05/12/2026 e o orçamento de nuvem tem teto de US$ 50. Para limitar o trabalho, o MVP usa entrada manual e não inclui OCR, integrações com concessionárias ou ERP. A seleção de fatores, suas condições de uso e a ficha metodológica precisam ser verificadas antes da homologação. O documento não comprova experiência prévia da equipe nas tecnologias escolhidas nem substitui os testes do motor de cálculo.

### 1.4 Segmentação de Clientes

A segmentação proposta prioriza redes que operam refrigeração, frota ou geradores próprios e concentram o fechamento em uma equipe pequena. A presença dessas fontes permite avaliar o cálculo de combustíveis e refrigerantes, além da eletricidade comprada.

| Perfil | Atividade no processo | Necessidade atendida | Limite no MVP |
| --- | --- | --- | --- |
| Gerente de ESG da matriz | Define unidades, fontes e fatores; acompanha o inventário | Consultar consolidados e autorizar reaberturas com motivo registrado | Não recebe certificação do inventário nem cálculo de Escopo 3 |
| Analista de controladoria | Revisa lançamentos, aprova dados e fecha a competência | Conferir pendências, consultar recibos e exportar PDF e CSV | A decomposição automática da variação entre revisões é Should |
| Gerente ou franqueado da unidade | Informa os dados mensais das fontes ativas | Usar um formulário com validação de valor e unidade de medida | Acesso restrito à unidade autorizada; coleta manual |
| Auditor ou verificador | Examina os resultados publicados | Conferir arquivos exportados, versões e recibos de cálculo | Acesso temporário dentro da plataforma é Could |

A hipótese comercial é que a diretoria financeira ou de sustentabilidade participe da compra, com influência de auditoria, consultoria ou franqueadora. Decisor, orçamento disponível e disposição de pagar ainda precisam ser confirmados nas entrevistas.

## 2 SOLUÇÃO PROPOSTA

### 2.1 Objetivos do Produto

O objetivo é permitir o fechamento mensal de um inventário de Escopos 1 e 2 em que cada resultado possa ser relacionado às entradas e recalculado com as versões utilizadas na publicação. O produto deve identificar pendências antes do fechamento, preservar correções como revisões e manter a soma das unidades consistente com o consolidado.

As metas abaixo são critérios de validação previstos, não resultados alcançados. Os cenários e limites por história estão no [backlog MoSCoW](backlog/MoSCoW.md).

| Dimensão | Meta | Marco previsto |
| --- | --- | --- |
| Reprodutibilidade | Diferença zero em 100 recálculos com as mesmas entradas e versões | Demo Day |
| Consistência | Soma exata das unidades igual ao total em 1.000 cenários de teste | Checkpoint 2 |
| Validação da coleta | Sinalizar pelo menos 19 de 20 erros de unidade identificáveis na massa de teste | Checkpoint 2 |
| Usabilidade | Cada um de 3 participantes registra 4 fontes em até 5 minutos | Checkpoint 3 |
| Rastreabilidade | Consultar a origem de cada um de 10 resultados em até 60 segundos e 3 cliques | Demo Day |
| Desempenho | p95 de leitura até 800 ms e de escrita até 1,5 s; cálculo de uma unidade com 4 fontes até 1,5 s no p95 | Checkpoint 4 |
| Qualidade do código | Cobertura de pelo menos 80% no domínio e 60% global; nenhuma vulnerabilidade crítica ou alta sem exceção documentada | Checkpoint 4 |
| Interface web | Lighthouse de pelo menos 90, com páginas e categorias de avaliação registradas no plano de testes | Checkpoint 4 |
| Autorização | Cobrir 100% das rotas protegidas e aprovar os testes de isolamento por cliente e unidade | Checkpoint 2 |
| Custo de nuvem | Acumulado até US$ 5 no CP2, US$ 12 no CP3 e US$ 50 no ciclo | Checkpoints 2 a 4 |

Para desempenho, o backlog propõe 100 operações após 10 de aquecimento, com 100 unidades cadastradas e 1 usuário simultâneo, em ambiente identificado. O resultado desse teste não comprova capacidade para 300 unidades com acessos concorrentes.

### 2.2 Características da Solução

**Fontes e limites do inventário**

O Escopo 1 inclui combustíveis de frota própria, combustíveis estacionários, como geradores, GLP e gás natural, e gases refrigerantes. O Escopo 2 inclui eletricidade comprada pela abordagem por localização. A ficha metodológica deverá definir os fatores, as conversões, o tratamento de CO2 biogênico e a versão de GWP usada em cada conjunto de cálculo.

As cinco jornadas obrigatórias são:

| Jornada | Comportamento previsto | Histórias do backlog |
| --- | --- | --- |
| Cadastrar unidades e fontes | Registrar código, nome, vínculo própria ou franqueada, área e fontes ativas; rejeitar código duplicado no mesmo cliente | CT-M01 e CT-M02 |
| Registrar dados mensais | Validar valor e unidade de medida; registrar competência, autor e instante; impedir que a mesma requisição gere lançamentos duplicados | CT-M03 |
| Calcular emissões | Converter unidades e aplicar fatores e metodologia versionados, preservando as entradas de cada execução | CT-M04 e CT-M05 |
| Consolidar o dashboard | Exibir totais por competência, unidade e escopo, identificar pendências e consultar a versão de fechamento selecionada | CT-M06, CT-M07 e CT-M08 |
| Exportar PDF e CSV | Gerar arquivos de um período fechado com referências aos recibos, versões e verificação de integridade | CT-M09 e CT-M10 |

**Versionamento, fechamento e auditoria**

Fatores publicados e revisões de lançamentos não são editados. Uma correção cria uma versão com referência à anterior e motivo registrado. Os períodos têm quatro estados: aberto, em revisão, fechado e reaberto. O fechamento exige dados completos e aprovados; a reabertura depende do perfil ESG. Um novo fechamento preserva a consulta à publicação anterior.

Cada recibo contém entradas, conversões, fórmula, resultado e versões de fator, GWP e metodologia. O hash SHA-256 permite comparar a integridade do conteúdo. Na exportação, o PDF informa o hash do CSV final, e um manifesto separado lista os hashes dos dois arquivos. O hash não comprova a veracidade do consumo informado e não equivale a uma certificação.

**Priorização**

O [backlog MoSCoW](backlog/MoSCoW.md) contém 27 histórias: 10 Must, 4 Should, 3 Could e 10 Won't. A implementação deve respeitar essa divisão:

| Prioridade | Conteúdo |
| --- | --- |
| Must | Cinco jornadas básicas, autorização por cliente e unidade, fatores imutáveis, revisões, fechamento e recibos |
| Should | Ponte de variação entre revisões, ranking por intensidade, outliers, qualidade e plausibilidade dos dados e importação de unidades por CSV |
| Could | Acesso temporário do auditor, filtros por vínculo e abordagem de consolidação, escolha de AR5 ou AR6 por período |
| Won't | Cálculo de Escopo 3, OCR de faturas, integração com concessionárias, certificação de terceira parte, créditos de carbono, Escopo 2 baseado em mercado, integração com ERP, anexos de documentos, aplicativo nativo e benchmark entre clientes |

O cadastro do vínculo própria ou franqueada é Must; os filtros e o registro da abordagem de consolidação são Could. Uma indicação de possível categoria 14 será apenas informativa, caso CT-C02 seja entregue. O MVP não calcula emissões de Escopo 3.

Se CT-S02 for entregue, as comparações usarão kgCO2e por m², mediana da rede e escore z modificado com limiar absoluto maior que 3,5. CT-S03 acrescentará rótulos de qualidade e justificativas de desvios históricos. Os tratamentos para área inválida, histórico insuficiente e MAD zero estão especificados no backlog. Esses recursos não são condição para concluir as cinco jornadas básicas.

**Dados pessoais e operação do piloto**

A proposta limita os dados pessoais a nome, e-mail corporativo, papel e vínculo com unidade. Os eventos de auditoria devem usar um identificador opaco, com a correspondência para a pessoa mantida separadamente. Logs de aplicação não devem conter dados pessoais.

A política proposta prevê retenção dos dados de identificação durante o contrato e por até 90 dias após seu término, seguida de eliminação ou anonimização. Papéis contratuais, bases legais e procedimentos para solicitações de titulares dependem de validação jurídica antes de uso com dados reais. A demonstração utiliza somente dados sintéticos; eventuais registros de entrevistas devem ter consentimento e não conter nome, CPF ou telefone. Essas são condições planejadas, não uma declaração de conformidade já verificada.

### 2.3 Tecnologias a Serem Utilizadas

| Componente | Tecnologia prevista | Uso no projeto |
| --- | --- | --- |
| Autenticação | Firebase Authentication | Identificar usuários antes de autorizar operações |
| Persistência | Cloud Firestore | Armazenar unidades, lançamentos, versões, recibos e consolidados com separação por cliente |
| API | Cloud Run, no Google Cloud | Executar validações, cálculos, fechamento e exportações |
| Aplicação web | Firebase Hosting | Publicar a interface de coleta, revisão e consulta |
| Ambiente local | Docker Compose e emuladores Firebase | Reproduzir API, frontend e serviços de desenvolvimento com dados sintéticos |
| Contrato da API | OpenAPI | Documentar operações, entradas, respostas e erros |
| Entrega e acesso à nuvem | CI e Workload Identity Federation | Executar verificações e autenticar o processo de implantação |

A linguagem da API, o framework do frontend, as bibliotecas de cálculo decimal e de PDF e o provedor de CI ainda precisam ser definidos em registros de decisão de arquitetura (ADRs). A escolha deve considerar precisão decimal, execução nos serviços previstos e capacidade de manutenção pela equipe.

O acesso deve ser autorizado na API e nas regras do Firestore, com negação por padrão e permissões explícitas. A proposta inclui TLS em trânsito, criptografia em repouso gerenciada pelo provedor e testes de acesso entre clientes e unidades. Consultas paginadas e consolidados pré-calculados devem limitar leituras repetidas; a atualização desses consolidados precisa preservar a revisão do fechamento e tolerar reprocessamento sem duplicação.

### 2.4 Pesquisa de Mercado e Análise Competitiva

A pesquisa de mercado ainda é preliminar. Os materiais disponíveis identificam alternativas, mas não registram entrevistas concluídas, preços coletados ou testes comparativos. A tabela define o que deverá ser verificado; não atribui limitações a fornecedores sem evidência.

| Alternativa a investigar | Questões da comparação | Evidência a registrar |
| --- | --- | --- |
| Planilhas usadas pelas redes | Como são controladas versões, aprovações, conversões e correções? | Exemplo anonimizado do fluxo e tempo gasto no fechamento |
| Ferramenta do Programa Brasileiro GHG Protocol | Quais cálculos e fatores podem servir de referência para o recorte adotado? | Versão utilizada, premissas e 10 casos de cálculo comparáveis |
| Consultorias de inventário | Como organizam a coleta mensal e a entrega de evidências ao cliente? | Descrição do serviço, frequência e proposta comercial quando disponível |
| Plataformas corporativas, como IBM Envizi, Watershed, Persefoni, Sweep e Greenly | Como tratam coleta por unidade, revisões, reprodução de fechamentos e exportação de evidências? | Documentação oficial ou demonstração, com data e limitações observadas |

A hipótese de diferenciação da CarbonTrace é oferecer um fluxo restrito a Escopos 1 e 2 que associe coleta por unidade, fechamento mensal e reprodução de versões anteriores. A hipótese será avaliada com pelo menos duas entrevistas com profissionais do segmento. Devem ser registrados o processo atual, os erros encontrados, o esforço para rastrear um número, os responsáveis pela compra e a disposição de testar a solução. Se forem usadas evidências secundárias no Checkpoint 1, elas precisam ser identificadas como tal, sem serem apresentadas como entrevistas.

### 2.5 Análise de Viabilidade

**Viabilidade técnica**

O MVP dispensa integrações externas de coleta e concentra o trabalho em formulários, autorização, cálculo e versionamento. Isso reduz dependências de concessionárias e ERPs, mas não elimina a necessidade de validar fatores e regras de cálculo. A viabilidade será demonstrada por jornadas completas, testes de isolamento e comparação com casos de referência.

A homologação prevê 10 casos de referência com tolerância proposta de 0,1%, a confirmar na ficha metodológica, e resultado exatamente zero quando a referência for zero. A reprodução de uma mesma versão deve ter diferença zero, independentemente dessa tolerância de comparação externa. A revisão dos fatores por duas pessoas e a conferência das condições de redistribuição são necessárias antes de publicar o conjunto de fatores.

**Prazo e entregas**

| Marco | Data em 2026 | Entrega prevista |
| --- | --- | --- |
| Checkpoint 1 | 18/10 | Visão, Ishikawa, backlog com aceite mensurável, 2 entrevistas ou evidências secundárias identificadas, hipótese de diferenciação, ADRs, OpenAPI v0.1, Definition of Done, ambiente local, CI com lint e build, dados de teste reproduzíveis, `.env.example`, alertas de orçamento, teste de isolamento escrito, ficha metodológica e fatores v1 |
| Checkpoint 2 | 01/11 | De 3 a 5 jornadas completas no ambiente local, regras do Firestore com negação por padrão e testes, testes de partição de equivalência e valores-limite, matriz de rastreabilidade e custo acumulado até US$ 5 |
| Checkpoint 3 | 15/11 | Implantação no Firebase Hosting e Cloud Run, verificações de disponibilidade, logs JSON, Workload Identity Federation e custo acumulado até US$ 12 |
| Checkpoint 4 | 29/11 | Verificações de CI que bloqueiem entregas reprovadas, cobertura e Lighthouse nas metas, reversão de implantação ensaiada e relatório de custos |
| Demo Day | 05/12 | Homologação das cinco jornadas obrigatórias e apresentação das evidências |

Should e Could só devem ser assumidos após avaliar a capacidade restante. A comparação de unidades com a mediana, prevista na ideia inicial da demonstração, depende da entrega de CT-S02. O recálculo de um período com o fator original permanece obrigatório.

**Viabilidade financeira**

A estimativa inicial de infraestrutura registrada no projeto é de aproximadamente US$ 8, com teto de US$ 50 para o ciclo. Esse valor ainda não foi acompanhado de uma memória de cálculo por serviço e volume de uso. Não deve ser tratado como cotação vigente, custo de produção ou custo total do projeto: trabalho da equipe, suporte e eventual validação especializada não estão quantificados.

O acompanhamento deve usar os gastos acumulados dos serviços e os limites de cada checkpoint. Ao atingir US$ 40, a equipe deverá revisar instâncias e rotinas agendadas, quando existentes, para conter o consumo antes do teto. Consultas e exportações precisam ser observadas nos testes para identificar leituras excessivas no Firestore.

**Viabilidade de mercado e riscos**

A viabilidade comercial depende de confirmar a frequência do problema e o interesse em adotar a coleta proposta. Ainda não há base para estimar receita, retorno financeiro ou prazo de aquisição de clientes.

| Risco | Tratamento previsto |
| --- | --- |
| Problema pouco frequente ou sem orçamento para solução | Validar processo, esforço e decisão de compra em entrevistas |
| Fator inadequado, desatualizado ou sem permissão de uso | Registrar fonte, publicação e vigência; revisar conteúdo e condições de uso antes da publicação |
| Divergência de cálculo ou consolidação | Usar decimal, conversões explícitas, casos de referência e 1.000 cenários de consistência |
| Perda de histórico em correções | Preservar revisões e testar consultas a fechamentos anteriores |
| Acesso indevido entre clientes | Testar API e Firestore com clientes e unidades distintos |
| Prazo ou orçamento excedido | Priorizar Must, revisar MoSCoW semanalmente e acompanhar limites de custo |
| Relatório interpretado como certificação | Identificar metodologia e versões e informar no PDF que não constitui certificação de terceira parte |

### 2.6 Impacto da Solução

Para a unidade, o benefício esperado é concluir o registro de quatro fontes em até cinco minutos, com retorno imediato sobre campos ausentes, valores negativos e unidades incompatíveis. O teste com três participantes deverá verificar se o formulário atende essa meta sem assistência durante a execução.

Para a controladoria, o resultado esperado é consultar pendências antes de fechar a competência e corrigir lançamentos sem perder a publicação anterior. A diferença zero entre soma das unidades e total da rede será verificada nos testes de consistência. Uma eventual redução do tempo total de fechamento só poderá ser calculada depois de medir o processo atual e compará-lo a um piloto equivalente.

Para ESG e auditoria, o benefício esperado é localizar a origem de um resultado em até sessenta segundos e três cliques, além de reproduzir o cálculo com as mesmas versões. PDF, CSV e manifesto permitirão conferir os resultados publicados e detectar alterações nos arquivos. A qualidade do inventário continuará dependendo da qualidade dos dados informados e da adequação dos fatores escolhidos.

A demonstração deverá mostrar uma correção preservando o fechamento anterior e cem recálculos com diferença zero usando as mesmas entradas e versões. Se o ranking por intensidade for entregue, também deverá mostrar a comparação de unidades com a mediana da própria rede. O projeto não promete redução de emissões, certificação, economia financeira ou conformidade jurídica comprovada apenas pela adoção da plataforma.
