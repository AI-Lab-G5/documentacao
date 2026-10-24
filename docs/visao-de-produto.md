# Visão do Produto e Projeto: CarbonTrace

AI Lab Makers · Maker Foundation · G5

Revisão editorial de 23/10/2026. Estrutura original preservada; conteúdo alinhado ao [ADR-008](adr/ADR-008-recorte-crud.md): CRUD de consumos com cinco jornadas, um perfil de funcionário e histórico mínimo. Este documento descreve o alvo planejado, não comprova implementação.

## 1 CENÁRIO ATUAL DO CLIENTE E DO NEGÓCIO

### 1.1 Introdução ao Negócio e Contexto

A CarbonTrace é uma proposta de plataforma web para redes de varejo e franquias registrarem consumos mensais por unidade, calcularem emissões de gases de efeito estufa e consolidarem os resultados da rede. O CRUD de consumos é a base: cadastrar, consultar, corrigir e cancelar dados com histórico mínimo. O recorte cobre eletricidade e três combustíveis dos Escopos 1 e 2, sem afirmar inventário completo homologado. O projeto é desenvolvido pelo Grupo 05 no AI Lab Makers, no ciclo Maker Foundation.

O segmento inicial compreende redes com 20 a 300 unidades, com equipe de ESG ou controladoria na matriz e responsáveis pela coleta nas lojas. Supermercados, atacarejos regionais, farmácias, redes de alimentação e lojas de conveniência são setores a investigar. Esse recorte define o público pretendido; a capacidade para atender 300 unidades ainda depende de testes.

O processo considerado começa com o registro de eletricidade, diesel, gasolina e GLP. Um funcionário com login individual opera as unidades da própria empresa, registra os consumos e consulta cálculos, dashboard, ranking e exportações. Corrigir cria revisão; cancelar preserva a origem e retira o lançamento dos totais correntes. Cálculos anteriores continuam consultáveis como snapshots imutáveis. Aprovação segregada, fechamento/reabertura e refrigerantes foram retirados do MVP. Ainda não há, nos materiais do projeto, um cliente contratado ou resultados de entrevistas que comprovem a frequência desses problemas.

### 1.2 Identificação da Oportunidade ou Problema

O problema investigado é a dificuldade de conferir a origem e reproduzir o cálculo dos números de um inventário consolidado a partir de planilhas de várias unidades. A documentação inicial identifica seis hipóteses para validação com responsáveis pela coleta e pelo fechamento:

1. Valores são registrados em unidades de medida diferentes, como kWh e MWh, sem conversão controlada.
2. Fatores de emissão são substituídos sem preservar a versão usada no fechamento anterior.
3. Correções sobrescrevem o dado original e não registram autor, data e motivo.
4. A soma das unidades diverge do total da rede por cópia manual ou arredondamento em etapas diferentes.
5. Os registros não identificam separadamente quem informou e quem aprovou os dados.
6. A resposta a uma solicitação de auditoria depende de localizar arquivos e reconstruir cálculos manualmente.

A oportunidade consiste em reunir coleta, correção, cálculo e consulta em um fluxo com referências persistentes entre o dado informado e o resultado salvo. A hipótese de separação entre quem informa e quem aprova, citada acima, continua como contexto do processo do cliente, mas não gera aprovação nem quatro perfis na aplicação. A redução de retrabalho deverá ser medida em um piloto; não há uma linha de base que permita estimar economia de horas ou de custos neste momento.

A Figura 1 organiza as causas propostas nas categorias de método, máquina, mão de obra, materiais, medida e meio ambiente. O diagrama representa somente hipóteses do projeto, não resultados de uma pesquisa concluída.

![Diagrama de Ishikawa: causas da baixa confiança no inventário consolidado](../assets/Diagrama_Ishikawa.png)

*Figura 1. Diagrama de Ishikawa da CarbonTrace, organizado pelos 6M.*

O diagrama permanece diagnóstico histórico. Nem todas as causas serão tratadas neste MVP: aprovação, fechamento formal, comparação por área e plausibilidade histórica automática estão fora do recorte obrigatório.

### 1.3 Desafios do Projeto

O desafio técnico principal é manter a reprodução dos cálculos após correções e novas versões de fatores. Cada snapshot deve registrar as revisões de entrada, a configuração das fontes esperadas, a conversão, o fator/versão, a metodologia fixa, o autor, a data e os resultados. Premissas de GWP e gases já incorporados ao fator agregado ficam documentadas na metodologia, sem motor por gás ou seleção AR5/AR6. Disponibilizar B não pode alterar um cálculo salvo com A.

A consolidação exige aritmética decimal e uma regra única de arredondamento. O total deve ser calculado com valores não arredondados; eventuais diferenças entre a soma dos valores exibidos e o total exibido precisam ser identificadas. Correções retroativas devem atualizar uma nova revisão do consolidado sem duplicar lançamentos nem modificar a versão anterior.

A operação é centrada em um funcionário responsável pela coleta, sem exigir conhecimento especializado em carbono para preencher o formulário. Cada pessoa usa sua conta, com as mesmas permissões nas unidades da própria empresa. ESG/controladoria continuam como interessados nos resultados, mas não há aprovador distinto, fluxo de fechamento ou interface de auditor. Simplificar perfis não elimina autenticação, isolamento por empresa ou registro de autoria.

O prazo termina em 05/12/2026 e o orçamento de nuvem tem teto de US$ 50. Para limitar o trabalho, o MVP usa entrada manual e não inclui OCR, integrações com concessionárias ou ERP. A seleção de fatores, suas condições de uso e a ficha metodológica precisam ser verificadas antes da homologação. O documento não comprova experiência prévia da equipe nas tecnologias escolhidas nem substitui os testes do motor de cálculo.

### 1.4 Segmentação de Clientes

A segmentação proposta prioriza redes que concentram a coleta em uma equipe pequena e usam eletricidade, frota ou GLP. A presença dessas fontes permite avaliar os consumos cobertos pelo catálogo reduzido. O segmento comercial de 20 a 300 unidades é uma hipótese; o piloto utiliza poucas unidades e não comprova capacidade para toda essa faixa.

| Perfil | Atividade no processo | Necessidade atendida | Limite no MVP |
| --- | --- | --- | --- |
| Gerente de ESG da matriz | Interessado no processo e nos resultados da rede | Consultar consolidados e compreender a origem dos números | Não constitui perfil separado na aplicação; sem reabertura, certificação ou Escopo 3 |
| Analista de controladoria | Interessado na conferência de pendências e relatórios | Consultar revisões e resultados e utilizar PDF/CSV | Sem perfil de aprovação, fechamento ou ponte de variação |
| Funcionário responsável pela coleta | Único perfil da aplicação; opera as unidades da própria empresa | Cadastrar unidades/fontes, coletar, corrigir, cancelar e consultar dados | Login individual; mesmas permissões na empresa; coleta manual |
| Auditor ou verificador | Destinatário externo de resultados, quando necessário ao cliente | Examinar arquivos e referências das revisões e fatores utilizados | Sem conta/perfil temporário ou interface própria no MVP |

A hipótese comercial é que a diretoria financeira ou de sustentabilidade participe da compra, com influência de auditoria, consultoria ou franqueadora. Decisor, orçamento disponível e disposição de pagar ainda precisam ser confirmados nas entrevistas.

## 2 SOLUÇÃO PROPOSTA

### 2.1 Objetivos do Produto

O objetivo é permitir a coleta mensal guiada e o cálculo simplificado de consumos dos Escopos 1 e 2, com resultado relacionado às entradas e reproduzível pelas versões utilizadas. O produto deve identificar pendências, preservar correções/cancelamentos e manter a soma das unidades consistente com o consolidado. Não exige aprovação ou fechamento de período.

As metas abaixo são critérios de validação previstos, não resultados alcançados. Os cenários e limites por história estão no [backlog MoSCoW](backlog/MoSCoW.md). A meta exploratória de rastrear 10 resultados em 60 segundos/3 cliques preserva a avaliação original; CT-M09 exige inicialmente conferir a origem de pelo menos 5 resultados. Metas exploratórias não significam testes já executados.

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
| Autorização | Cobrir 100% das rotas protegidas e os testes de isolamento entre empresas; permitir as unidades da própria empresa | Checkpoint 2 |
| Custo de nuvem | Acumulado até US$ 5 no CP2, US$ 12 no CP3 e US$ 50 no ciclo | Checkpoints 2 a 4 |

Para desempenho, o backlog propõe 100 operações após 10 de aquecimento, com 100 unidades cadastradas e 1 usuário simultâneo, em ambiente identificado. O resultado desse teste não comprova capacidade para 300 unidades com acessos concorrentes.

### 2.2 Características da Solução

**Fontes e limites do inventário**

O catálogo fixo inclui eletricidade comprada por localização (Escopo 2), diesel e gasolina de frota própria e GLP estacionário (Escopo 1). Não inclui gás natural ou refrigerantes. Eletricidade aceita kWh/MWh, diesel e gasolina L, GLP kg; 1 MWh equivale a 1.000 kWh. A metodologia fixa documenta premissas ambientais, fatores agregados e suas limitações, inclusive gases/GWP incorporados; não há escolha de GWP ou decomposição por gás. Fatores não conferidos ficam identificados como preliminares, sem alegação de homologação ambiental.

As cinco jornadas obrigatórias são:

| Jornada | Comportamento previsto | Histórias do backlog |
| --- | --- | --- |
| Cadastrar unidades e fontes | Registrar/editar/desativar código e nome, vincular fontes fixas e rejeitar código repetido na empresa; sem área/vínculo obrigatório | CT-M01 e CT-M02 |
| Registrar dados mensais | Validar quantidade/unidade, registrar competência/autor/instante e evitar duplicatas; corrigir ou cancelar com histórico | CT-M03, CT-M07 e CT-M11 |
| Calcular emissões | Converter unidades permitidas, aplicar fatores versionados/metodologia fixa e preservar o snapshot | CT-M04, CT-M05 e CT-M09 |
| Consolidar o dashboard | Totais por competência/unidade/escopo, pendências, ranking, mediana e alertas no snapshot selecionado | CT-M08 e CT-M12 |
| Exportar PDF e CSV | Arquivos do mesmo snapshot com revisões, fatores, metodologia, autoria e datas, sem fechamento ou manifesto criptográfico | CT-M09 e CT-M10 |

**Versionamento, fechamento e auditoria**

Fatores publicados e revisões de lançamentos não são editados. A equipe prepara versões A/B, sem tela/API de publicação. Correção exige motivo e revisão-base, criando outra revisão com referência à anterior, autor e data. Cancelamento lógico exige confirmação e motivo, preserva a origem e retira o lançamento dos totais correntes. Não há aprovação, estados de fechamento/reabertura ou DELETE físico na aplicação. O fechamento pode existir no processo do cliente, mas CT-M06 está adiada.

Cada detalhe de cálculo contém entradas/revisões, conversões, fórmula, resultado, fatores/versões e metodologia fixa. Snapshots guardam também fontes esperadas, autoria e datas; B, correção ou cancelamento não altera snapshot A. Recalcular A usa suas próprias entradas/fatores e deve produzir diferença zero. PDF/CSV usam o mesmo snapshot e preservam essas referências, sem hash, assinatura, manifesto, job assíncrono ou Storage obrigatório. A rastreabilidade não comprova a veracidade do consumo nem constitui certificação.

No CRUD, unidade/fonte/competência identificam o lançamento e não mudam na correção; corrigem-se quantidade e unidade de medida compatível. Identificação errada exige cancelar e cadastrar em outra chave. A chave permanece reservada após cancelamento, sem reativação no MVP; avisar essa limitação antes de confirmar. Desativar unidade/fonte impede novas coletas, sem apagar ou subtrair dados anteriores silenciosamente.

Quantidade zero explícita é válida; ausência, negativo, NaN/infinito, competência inválida e unidades incompatíveis são rejeitados antes de gravar. Domínio usa Decimal, JSON/banco usam strings; somar valores exatos antes de exibir em 3 casas ROUND_HALF_EVEN e informar a diferença de arredondamento quando houver.

**Priorização**

O [backlog MoSCoW](backlog/MoSCoW.md) preserva a correspondência dos IDs anteriores, registra o adiamento de CT-M06 e acrescenta CT-M11/CT-M12. O prefixo histórico não substitui a prioridade vigente. A implementação deve respeitar esta divisão:

| Prioridade | Conteúdo |
| --- | --- |
| Must | Cinco jornadas, login/isolamento por empresa, fatores imutáveis, revisões/cancelamento, snapshots, ranking/mediana básicos e PDF/CSV rastreável |
| Should | Qualidade e plausibilidade histórica automáticas, além das validações obrigatórias (CT-S03) |
| Could | Importação de unidades por CSV (CT-S04), somente após Must e avaliação de capacidade |
| Won't | Aprovação/fechamento/reabertura, quatro perfis, auditor temporário, intensidade/MAD, refrigerantes, seleção GWP, infraestrutura criptográfica e exclusões originais: Escopo 3, OCR, concessionárias, certificação, créditos, mercado, ERP, anexos, app nativo e benchmark entre clientes |

Vínculo própria/franqueada e área não são obrigatórios. CT-C02, incluindo filtros/abordagem e eventual rótulo de categoria 14, fica adiada. O MVP não calcula emissões de Escopo 3; exclusão de escopo não deve ser apresentada como emissão zero.

A comparação básica agora é Must (CT-M12). Ranking por total kgCO2e decrescente, desempate por código; somente unidades ativas/completas com pelo menos uma fonte esperada entram na mediana, incluindo zeros explícitos. Com pelo menos 3 unidades e mediana maior que zero, alertar se abs((valor−mediana)/mediana×100)>50%; exatamente ±50% não alerta. Menos de 3 ou mediana zero produz aviso sem classificação. Unidades incompletas exibem pendências/total parcial, sem transformar ausência em zero.

O alerta é operacional, não teste estatístico ou comparação de eficiência. Intensidade kgCO2e/m² e z modificado/MAD (CT-S02) ficam adiados. CT-S03 continua como possível evolução após Must, sem bloquear as cinco jornadas.

**Dados pessoais e operação do piloto**

A proposta limita os dados pessoais a nome/e-mail corporativo para conta individual, papel único e vínculo com empresa. Revisões/snapshots usam UID opaco, com correspondência de identificação mantida separadamente; não há infraestrutura própria de eventos criptográficos. Logs de aplicação não devem conter dados pessoais.

A política proposta prevê retenção dos dados de identificação durante o contrato e por até 90 dias após seu término, seguida de eliminação ou anonimização. Papéis contratuais, bases legais e procedimentos para solicitações de titulares dependem de validação jurídica antes de uso com dados reais. A demonstração utiliza somente dados sintéticos; eventuais registros de entrevistas devem ter consentimento e não conter nome, CPF ou telefone. Essas são condições planejadas, não uma declaração de conformidade já verificada.

### 2.3 Tecnologias a Serem Utilizadas

| Componente | Tecnologia prevista | Uso no projeto |
| --- | --- | --- |
| Autenticação | Firebase Authentication | Identificar usuários antes de autorizar operações |
| Persistência | Cloud Firestore | Armazenar unidades, fontes, revisões, fatores e snapshots com separação por cliente |
| API | Cloud Run, no Google Cloud | Executar validações, cálculo, correção/cancelamento e exportações síncronas |
| Aplicação web | Firebase Hosting | Publicar interface CRUD, cálculo e dashboard do funcionário |
| Ambiente local | Docker Compose e emuladores Firebase | Reproduzir API, frontend e serviços de desenvolvimento com dados sintéticos |
| Contrato da API | OpenAPI | Documentar operações, entradas, respostas e erros |
| Entrega e acesso à nuvem | CI e Workload Identity Federation | Executar verificações e autenticar o processo de implantação |

As escolhas existentes são Python/FastAPI com decimal.Decimal, React/TypeScript/Vite e GitHub Actions, registradas nos [ADRs](adr/README.md). Bibliotecas de geração PDF são detalhe de implementação a verificar. Manter a base evita migração durante o ciclo. A documentação e a OpenAPI 0.2.0 são alvo planejado: não demonstram que o código atual já os implementa. O workflow existente contém TODOs e não prova execução completa dos testes.

O acesso é autorizado na API pelo tenant/papel do token; claims tenant_id e papel=funcionario. Firestore direto usa deny-all para clientes anônimos/autenticados. Admin SDK ignora Rules, portanto a API valida cada operação, inclusive histórico e relatórios. Funcionários acessam todas as unidades da própria empresa; testes devem permitir esse caso e negar outra empresa.

Manter TLS em trânsito e criptografia gerenciada pelo provedor. Listas paginadas usam padrão 20/máximo 100; queries são filtradas/controladas e não há listener amplo sem justificativa. Totais/mediana usam o snapshot completo, nunca a soma de uma página da interface. Escritas têm Idempotency-Key com janela de 24 horas e controle de revisão-base, sem exigir hash criptográfico.

### 2.4 Pesquisa de Mercado e Análise Competitiva

A pesquisa de mercado ainda é preliminar. Os materiais disponíveis identificam alternativas, mas não registram entrevistas concluídas, preços coletados ou testes comparativos. A tabela define o que deverá ser verificado; não atribui limitações a fornecedores sem evidência.

| Alternativa a investigar | Questões da comparação | Evidência a registrar |
| --- | --- | --- |
| Planilhas usadas pelas redes | Como são controladas versões, aprovações, conversões e correções? | Exemplo anonimizado do fluxo e tempo gasto no fechamento |
| Ferramenta do Programa Brasileiro GHG Protocol | Quais cálculos e fatores podem servir de referência para o recorte adotado? | Versão utilizada, premissas e 10 casos de cálculo comparáveis |
| Consultorias de inventário | Como organizam a coleta mensal e a entrega de evidências ao cliente? | Descrição do serviço, frequência e proposta comercial quando disponível |
| Plataformas corporativas, como IBM Envizi, Watershed, Persefoni, Sweep e Greenly | Como tratam coleta por unidade, revisões, reprodução de resultados e exportação de evidências? | Documentação oficial ou demonstração, com data e limitações observadas |

A hipótese de diferenciação da CarbonTrace é oferecer um fluxo restrito de coleta por unidade, CRUD com histórico e reprodução de versões anteriores dos cálculos dos consumos cobertos. Cálculo e rastreabilidade, isoladamente, não comprovam diferenciação exclusiva. A hipótese será avaliada com pelo menos duas entrevistas com profissionais do segmento. Devem ser registrados o processo atual, os erros encontrados, o esforço para rastrear um número, os responsáveis pela compra e a disposição de testar a solução. Se forem usadas evidências secundárias no Checkpoint 1, elas precisam ser identificadas como tal, sem serem apresentadas como entrevistas.

### 2.5 Análise de Viabilidade

**Viabilidade técnica**

O MVP dispensa integrações externas de coleta e concentra o trabalho em formulários, autorização, cálculo e versionamento. Isso reduz dependências de concessionárias e ERPs, mas não elimina a necessidade de validar fatores e regras de cálculo. A viabilidade será demonstrada por jornadas completas, testes de isolamento e comparação com casos de referência.

A avaliação metodológica original propôs 10 casos de referência com tolerância de 0,1%, a confirmar na ficha metodológica, e resultado exatamente zero quando a referência for zero. Essa avaliação permanece uma proposta de validação adicional para casos compatíveis com o recorte; não declara equivalência a inventário completo nem cria novas fontes no MVP. A reprodução de uma mesma versão deve ter diferença zero, independentemente da tolerância externa. A revisão dos fatores por duas pessoas e a conferência das condições de redistribuição são necessárias antes de apresentá-los como verificados; fatores sintéticos/preliminares precisam estar rotulados.

**Prazo e entregas**

| Marco | Data em 2026 | Entrega prevista |
| --- | --- | --- |
| Checkpoint 1 | 18/10 | Visão, Ishikawa, backlog com aceite mensurável, 2 entrevistas ou evidências secundárias identificadas, hipótese de diferenciação, ADRs, OpenAPI v0.1, Definition of Done, ambiente local, CI com lint e build, dados de teste reproduzíveis, `.env.example`, alertas de orçamento, teste de isolamento escrito, ficha metodológica e fatores v1 |
| Checkpoint 2 | 01/11 | J1-J4 locais, contrato-alvo 0.2.0, revisões/cancelamento, fatores A/B, dashboard/ranking/mediana, deny-all, segurança, EP/BVA, testes de contrato, matriz/relatório e custo acumulado até US$ 5 |
| Checkpoint 3 | 15/11 | Implantação no Firebase Hosting e Cloud Run, verificações de disponibilidade, logs JSON, Workload Identity Federation e custo acumulado até US$ 12 |
| Checkpoint 4 | 29/11 | Verificações de CI que bloqueiem entregas reprovadas, cobertura e Lighthouse nas metas, reversão de implantação ensaiada e relatório de custos |
| Demo Day | 05/12 | Homologação das cinco jornadas obrigatórias e apresentação das evidências |

Should e Could só devem ser assumidos após avaliar a capacidade restante. Ranking/mediana e recálculo com fator original são obrigatórios (CT-M12/CT-M05), sem depender de CT-S02. Sprint 2 (19 a 25/10) prepara J1/J2 e base do cálculo; Sprint 3 (26/10 a 01/11) entrega J3/J4 e evidências do CP2. J5 PDF/CSV continua Must até 05/12, com planejamento posterior explícito e fora das 20 issues do CP2. O tutor deve ser informado da revisão; critérios técnicos dos gates permanecem.

**Viabilidade financeira**

A estimativa inicial de infraestrutura é US$ 3 a US$ 8, com teto de US$ 50 para o ciclo. Esse valor ainda não foi acompanhado de uma memória de cálculo por serviço e volume de uso. Não deve ser tratado como cotação vigente, custo de produção ou custo total do projeto: trabalho da equipe, suporte e eventual validação especializada não estão quantificados.

O acompanhamento deve usar os gastos acumulados dos serviços e os limites de cada checkpoint. Ao atingir US$ 40, a equipe deverá revisar instâncias e rotinas agendadas, quando existentes, para conter o consumo antes do teto. Consultas e exportações precisam ser observadas nos testes para identificar leituras excessivas no Firestore.

**Viabilidade de mercado e riscos**

A viabilidade comercial depende de confirmar a frequência do problema e o interesse em adotar a coleta proposta. Ainda não há base para estimar receita, retorno financeiro ou prazo de aquisição de clientes.

| Risco | Tratamento previsto |
| --- | --- |
| Problema pouco frequente ou sem orçamento para solução | Validar processo, esforço e decisão de compra em entrevistas |
| Fator inadequado, desatualizado ou sem permissão de uso | Registrar fonte, publicação e vigência; revisar conteúdo e condições de uso antes da publicação |
| Divergência de cálculo ou consolidação | Usar decimal, conversões explícitas, casos de referência e 1.000 cenários de consistência |
| Perda de histórico em correções | Preservar revisões e testar consultas/recálculos de snapshots anteriores |
| Acesso indevido entre clientes | Testar isolamento entre empresas na API e deny-all no Firestore; permitir unidades do mesmo cliente |
| Prazo ou orçamento excedido | Priorizar Must, revisar MoSCoW semanalmente e acompanhar limites de custo |
| Relatório interpretado como certificação | Identificar metodologia e versões e informar no PDF que não constitui certificação de terceira parte |

### 2.6 Impacto da Solução

Para a unidade, o benefício esperado é concluir o registro de quatro fontes em até cinco minutos, com retorno imediato sobre campos ausentes, valores negativos e unidades incompatíveis. O teste com três participantes deverá verificar se o formulário atende essa meta sem assistência durante a execução.

Para a controladoria, como interessada nos resultados, espera-se consultar pendências e receber dados corrigidos sem perder o cálculo anterior. A diferença zero entre soma das unidades e total da rede será verificada nos testes de consistência. Uma eventual redução do trabalho de conferência ou do fechamento organizacional só poderá ser calculada depois de medir o processo atual e compará-lo a um piloto equivalente; a aplicação não implementa aprovação ou fechamento.

Para ESG e destinatários externos de relatórios, o benefício esperado é localizar a origem de um resultado, com meta exploratória de até sessenta segundos e três cliques, e reproduzir o cálculo com as mesmas versões. PDF/CSV derivados do mesmo snapshot permitem conferir revisões, fatores, metodologia, autoria e datas; não há manifesto, assinatura ou prova criptográfica de integridade. A qualidade ambiental continua dependendo dos dados e da adequação dos fatores escolhidos.

A demonstração deverá mostrar correção/cancelamento preservando o snapshot anterior e cem recálculos com diferença zero usando as entradas e o fator original, mesmo após B. Também deverá mostrar ranking/mediana da própria rede e os limites dos alertas: ±50%, amostra insuficiente, mediana zero e pendências. PDF/CSV do mesmo snapshot completam J5 no Demo Day; sem registros, CSV tem cabeçalho e PDF explica ausência, sem inventar consumo zero. O projeto não promete redução de emissões, certificação, economia financeira ou conformidade jurídica comprovada apenas pela adoção da plataforma.
