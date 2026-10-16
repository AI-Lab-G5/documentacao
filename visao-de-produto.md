# Documento de Visão do Produto: CarbonTrace

AI Lab Makers · Maker Foundation · G5

## 1. Resumo do projeto

A CarbonTrace é uma plataforma web desenvolvida no AI Lab Makers para redes de varejo e franquias fecharem, todo mês, o inventário de emissões de gases de efeito estufa (Escopos 1 e 2) de diversas unidades buscando maximizar a confiabilidade dos dados, de modo que cada número publicado possa ser explicado, refeito e verificado por um auditor.

Nossa tese é que o problema não está somente em calcular carbono. O problema é **o nível de confiança do número consolidado**: dados de ponta digitados sem controle, fatores trocados sem registro, correções que sobrescrevem o histórico e totais que não fecham com a soma das unidades. Por isso a CarbonTrace surge como um **livro-razão de emissões**, no qual dados e fatores são imutáveis, correções viram revisões com autor e motivo, e todo resultado possui um recibo de cálculo reproduzível.

**Posicionamento.** Para gerentes de ESG e analistas de controladoria de redes de varejo e franquias, que precisam consolidar e defender o inventário de muitas unidades, a CarbonTrace é uma plataforma de inventário que torna cada número rastreável e reproduzível. Diferente de planilhas e das plataformas encontradas no mercado atualmente, nós versionamos de forma imutável dados, fatores e metodologia, permitindo recalcular qualquer período passado com o fator da época e obter exatamente o mesmo resultado.

**Abstração básica do projeto:**

- Complexidade: baixa na interface, mais concentrada no motor de cálculo.
- MVP: cinco jornadas, emissões de Escopo 1 e Escopo 2.
- Base técnica: GCP e Firebase (Auth e Firestore), API em Cloud Run, frontend em Firebase Hosting.
- Custo estimado de nuvem do MVP: aproximadamente US$ 8 (temos como orçamento US$ 50).

## 2. O problema

Aualmente, o inventário em grandes redes funciona de forma a acumular erros entre as partes, o gerente lança, por exemplo, o consumo mensal de luz e combustível numa planilha e manda para a matriz, porém, na hora de juntar dezenas de arquivos de diferentes origens, a rastreabilidade do dado é severamente comprometida.

**Falhas recorrentes (hipóteses a validar nas entrevistas)**

1. Erros de unidade e de digitação na entrada, como kWh por MWh.
2. Fatores desatualizados ou trocados sem registro, o que altera resultados passados.
3. Correções que sobrescrevem o valor original, sem autor, data ou motivo.
4. Agregados que não fecham com a soma das unidades, por arredondamento e cópia manual.
5. Nenhum responsável pelo dado: ninguém sabe quem informou ou aprovou cada número.
6. Auditoria reativa: responder “de onde veio este número?” é geralmente burocrático e moroso.

**Consequências.** Retrabalho mensal da controladoria, risco de greenwashing (divulgar um número que não se sustenta), perda de credibilidade com clientes, bancos e franqueadora, e um inventário que não resiste a uma verificação de terceira parte.

**Diagrama de Ishikawa (6M) - visão geral**

| Grupo | Causas |
| --- | --- |
| Método | Coleta sem padrão entre unidades; correções por sobrescrita; ausência de política de fechamento de período; metodologia de cálculo não documentada |
| Máquina | Planilhas com fórmulas frágeis; conversão de unidades manual; fatores sem controle de versão; consolidação por copiar e colar |
| Mão de obra | Gerentes de unidade sem formação em carbono; rotatividade na ponta; controladoria pequena e sobrecarregada |
| Material | Fatores de fontes e anos diferentes; contas e notas em formatos variados; dados faltantes ou estimados sem rótulo |
| Medida | Nenhuma checagem de plausibilidade contra o histórico; sem indicador de qualidade do dado; comparação entre unidades sem normalizar pelo tamanho |
| Meio ambiente | Pressão crescente de clientes, bancos e franqueadoras por dados verificáveis; metodologias em evolução; prazos de divulgação apertados |

## 3. Público-alvo e personas - visão geral

**Segmentação inicial:** Redes de varejo e franquias com 20 a 300 unidades, que operam refrigeração, frota ou geradores próprios e cuja matriz tem equipe pequena de ESG ou controladoria, sem time dedicado a inventário. 
Setores em observação: supermercados e atacarejos regionais, farmácias, redes de alimentação e lojas de conveniência.

**Compra (hipótese).** O decisor tende a ser a diretoria financeira ou de sustentabilidade, influenciada por auditoria, consultoria e, em franquias, pela franqueadora. A disposição de pagar é desconhecida e faz parte das entrevistas.

**Personas**

- **Gerente de ESG da matriz (decisora e usuária principal).** Quer responder a clientes e bancos com números defendíveis. Sofre para reconciliar planilhas e teme a pergunta do auditor. Recebe consolidação confiável, fechamento de período e pacote de auditoria.
- **Analista de controladoria (fechamento).** Quer fechar o mês e explicar variações. Sofre com versões trocadas por e-mail e totais que não batem. Recebe fluxo de revisão, recibos de cálculo e ponte de variação.
- **Gerente ou franqueado da unidade (coleta).** Quer informar os dados em poucos minutos e voltar à operação. Não domina o tema e teme errar. Recebe formulário guiado, na unidade de medida certa, com validação imediata.
- **Auditor ou verificador (leitura).** Quer verificar sem depender de explicações verbais. Hoje não consegue reconstruir o cálculo. Recebe acesso somente leitura, recibos, versões de fatores e hash de integridade.

## 4. Alternativas atuais e a lacuna que exploramos

| Alternativa | Pontos fortes | Lacuna que a CarbonTrace ocupa |
| --- | --- | --- |
| Planilhas próprias da rede | Flexíveis e familiares | Sem imutabilidade, sem validação na entrada, consolidação manual |
| Ferramenta de cálculo do Programa Brasileiro GHG Protocol | Autoridade metodológica, fatores brasileiros, atualização anual | É uma planilha: sem fluxo multiunidade, sem controle de quem informou e sem trilha de auditoria |
| Consultorias de inventário | Expertise e apoio à verificação | Retrato anual, não rotina mensal; custo recorrente; dependência de e-mail e planilha |
| Plataformas corporativas de carbono (por exemplo IBM Envizi, Watershed, Persefoni, Sweep, Greenly) | Cobertura ampla, incluindo Escopo 3, e integrações | Leitura nossa, a confirmar em análise competitiva: foco em médias e grandes empresas e em integrações; coleta em muitas unidades pequenas e reprodutibilidade verificável por terceiros não são o centro da proposta |

**Onde nos posicionamos.** No espaço entre a planilha e o meio corporativo: leve o bastante para uma rede média adotar em semanas e rigoroso o bastante para atravessar uma verificação de terceira parte. Vale ressaltar que nosso escopo não é consultoria, certificação de inventário ou vender compensações.

## 5. A solução

Seis princípios guiam o nosso produto e servem de critério de decisão se funcionalidades entrarem em conflito.

1. **Reprodutível.** Mesmos dados, mesma versão de fatores e mesma versão de metodologia produzem sempre o mesmo resultado.
2. **Imutável com revisões.** Nada é sobrescrito. Uma correção cria uma nova revisão, com autor, data e motivo.
3. **Explicável.** Todo número tem um recibo de cálculo e um caminho de consulta até o dado de origem.
4. **Preciso.** Aritmética robusta, unidades canônicas e arredondamento apenas na apresentação.
5. **Isolado.** Os dados de cada cliente (tenant) e de cada unidade são protegidos por desenho, com autorização em todas as rotas.

## 6. Diferenciais

**D1. Fatores versionados e imutáveis.** Cada versão de um conjunto de fatores registra fonte, data de publicação, vigência, valores gás a gás (CO2 fóssil, CO2 biogênico, CH4 e N2O) e a versão do GWP (AR5 ou AR6). Uma versão publicada nunca é editada: corrige-se publicando outra que a substitui, com motivo registrado.

**D2. Fechamento de período e correções retroativas.** Um período passa por quatro estados: aberto, em revisão, fechado e reaberto. Cada lançamento guarda duas datas, o mês de competência e o momento do registro. Assim o sistema responde tanto “qual é o número correto de março?” quanto “o que reportamos de março em 30 de abril?”.

**D3. Ponte de variação.** Quando um total muda entre revisões, o produto decompõe a diferença em efeito de dados corrigidos, efeito de troca de fator e efeito de metodologia. É a resposta direta à pergunta que mais dói numa auditoria: por que o número mudou?

**D4. Recibo de cálculo e pacote do auditor.** Cada resultado traz seu recibo: entradas, conversões, versão do fator, fórmula, resultado e hash SHA-256 das entradas. O relatório PDF e CSV exibe o hash do pacote, e quem recebe o arquivo consegue conferir sua integridade.

**D5. Coleta guiada com validação que entende o contexto.** O formulário por unidade usa a unidade de medida correta de cada fonte e compara o valor com o histórico da própria unidade: um desvio acima de 3,5 no escore z modificado (baseado em mediana e MAD) exige justificativa. Cada dado recebe um rótulo de qualidade (medido, estimado ou padrão), informação que o auditor pergunta e a planilha não registra.

**D6. Comparação justa entre unidades.** Ranking e outliers usam intensidade de emissão (por exemplo, kgCO2e por m² de área de venda) e a mediana da rede, para não punir lojas grandes só por serem grandes.

**D7. Feito para rede e franquias.** Cada unidade tem um vínculo (própria ou franqueada) e o cliente registra a abordagem de consolidação do GHG Protocol (controle operacional, controle financeiro ou participação societária). O MVP não calcula Escopo 3 de cadeia de valor, mas, conforme a abordagem escolhida, as emissões de Escopo 1 e 2 das franqueadas entram no inventário da franqueadora como Escopo 3, categoria 14 (franquias). O modelo de dados já nasce preparado para isso; no MVP, vínculo e abordagem servem para rotular e filtrar a consolidação.

## 7. Sobre o escopo do projeto

**Fontes de emissão cobertas.** Eletricidade comprada (Escopo 2, abordagem por localização), combustíveis de frota própria, combustíveis estacionários (geradores, GLP e gás natural) e gases refrigerantes (emissões fugitivas). Os refrigerantes entram desde o início porque, em varejo com refrigeração, são uma fonte relevante de Escopo 1 e porque os GWPs desses gases variam entre as versões do IPCC.

**Backlog do projeto - visão geral**

1. **Cadastrar unidades e fontes de emissão** (gestora de ESG). Cada unidade tem código, nome, vínculo, área em m² e fontes ativas. Aceite: cadastrar uma unidade com 3 fontes em até 3 minutos; código duplicado é rejeitado com erro 409; usuário de outra empresa recebe 403 ou 404 ao acessar a unidade.
2. **Registrar dados mensais com validação** (operador da unidade). Aceite: registrar 4 fontes em até 5 minutos; valor negativo ou unidade incompatível é rejeitado antes de salvar; em um conjunto de teste com 20 erros de unidade injetados, ao menos 19 (95%) são sinalizados; toda gravação registra autor, data e hora.
3. **Calcular emissões com fatores versionados** (sistema). Aceite: 100 execuções com as mesmas entradas e a mesma versão de fatores produzem resultado idêntico (diferença zero); todo resultado referencia a versão do fator, a versão da metodologia e o hash das entradas; cálculo de uma unidade em até 1,5 s (p95); 10 casos de referência conferidos com a ferramenta do Programa Brasileiro GHG Protocol dentro da tolerância definida na ficha metodológica (proposta: 0,1%).
4. **Consolidar dashboard, ranking e outliers** (gestora de ESG e controladoria). Aceite: a soma das unidades é igual ao total da rede em 100% dos testes, incluindo 1.000 casos aleatórios; dashboard com 100 unidades carrega em até 800 ms (p95); cada outlier (escore z modificado acima de 3,5 sobre a intensidade) mostra o motivo do alerta.
5. **Gerar relatório PDF e CSV com trilha de auditoria** (controladoria e auditor). Aceite: relatório de um período fechado gerado em até 10 s; 100% dos números do relatório levam a um recibo de cálculo; o hash exibido no PDF confere com o do CSV baixado; o relatório lista as versões de fatores e de metodologia usadas.

**Priorização MoSCoW**

- **Must:** as cinco jornadas em versão básica; fatores versionados (D1); revisões e fechamento de período (D2); recibo de cálculo (D4); validação básica na entrada (D5); isolamento por tenant.
- **Should:** ponte de variação (D3); ranking por intensidade e outliers (D6); rótulo de qualidade do dado; importação de unidades por CSV.
- **Could:** perfil de auditor com acesso temporário somente leitura; vínculo e abordagem de consolidação com rótulo de categoria 14 (D7); escolha de AR5 ou AR6 por período.
- **Won't (neste MVP):** Escopo 3 de cadeia de valor, OCR de faturas, integração com concessionárias, certificação de terceira parte, créditos de carbono, Escopo 2 por escolha de compra, integrações com ERP, anexos de arquivos, aplicativo nativo e benchmark entre clientes.

## 8. Métricas de sucesso

| Dimensão | Métrica | Meta | Quando |
| --- | --- | --- | --- |
| Confiança | Reprodutibilidade de períodos fechados | 100% | Demo Day |
| Consistência | Soma das unidades igual ao total da rede | Diferença zero em 100% das execuções | Checkpoint 2 |
| Coleta | Erros de unidade sinalizados na entrada | ≥ 95% do conjunto de teste | Checkpoint 2 |
| Usabilidade | Tempo de coleta de uma unidade com 4 fontes | ≤ 5 minutos, medido com 3 pessoas | Checkpoint 3 |
| Auditoria | Tempo para rastrear um número até a origem | ≤ 60 segundos e 3 cliques | Demo Day |
| Desempenho | p95 de leitura e de escrita | ≤ 800 ms e ≤ 1,5 s | Checkpoint 4 |
| Qualidade | Cobertura, vulnerabilidades e Lighthouse | ≥ 80% no domínio e ≥ 60% global; zero crítica ou alta sem exceção; Lighthouse ≥ 90 | Checkpoint 4 |
| Segurança | Autorização nas rotas e teste de isolamento | 100% das rotas; teste verde | Checkpoint 2 |
| Custo | Gasto acumulado de nuvem | ≤ US$ 5 (CP2), ≤ US$ 12 (CP3), teto de US$ 50 | CP2, CP3 e CP4 |

## 9. Riscos e mitigações

- **Valor de mercado não validado.** Mitigação: duas entrevistas com pessoas do setor.
- **Fatores incorretos ou com termos de uso restritivos.** Mitigação: registrar fonte e data de cada fator, revisão por duas pessoas, conferência cruzada com a ferramenta do Programa Brasileiro GHG Protocol e com os dados do MCTI, e checagem dos termos de uso antes de redistribuir.
- **Erro numérico de unidade, precisão ou arredondamento.** Mitigação: aritmética decimal, testes de propriedade e casos de referência.
- **Complexidade de revisões e fechamento de período.** Mitigação: modelo mínimo de quatro estados registrado em ADR.
- **Custo de leitura no Firestore.** Mitigação: agregados pré-computados, paginação, limites e alertas; plano de contenção em US$ 40 (reduzir instâncias e pausar o Scheduler).
- **Uso indevido do número (greenwashing).** Mitigação: relatórios com versões, rótulos de qualidade e nota metodológica; o produto não certifica inventários nem faz alegações de neutralidade.
- **Estouro de escopo.** Mitigação: lista Won't explícita, MoSCoW revisado semanalmente e gates dos checkpoints.
- **Mudança metodológica externa.** Mitigação: versionamento de metodologia previsto desde o desenho.

## 10. LGPD e privacidade

A CarbonTrace guarda nome completo, e-mail corporativo e cargo ou papel de cada usuário apenas para autenticação, controle de acesso e trilha de auditoria, durante a vigência do contrato do cliente e por até 90 dias depois, quando os dados de identificação são eliminados ou anonimizados.

- **Dados pessoais tratados:** nome completo, e-mail corporativo, cargo ou papel e vínculo com unidade. Não coletamos CPF, telefone, endereço pessoal nem localização, e os logs de aplicação não contêm dados pessoais.
- **Finalidade e base legal (proposta, a validar com orientação jurídica):** execução de contrato e legítimo interesse em segurança e auditoria (LGPD, art. 7º, incisos V e IX). Papéis a confirmar: cliente como controlador e CarbonTrace como operador dos dados de usuários.
- **Pseudonimização:** os eventos de auditoria guardam apenas um identificador opaco do usuário. A tabela que liga o identificador à pessoa fica separada e pode ser anonimizada a pedido do titular ou ao fim do contrato, sem quebrar a imutabilidade da trilha.
- **Retenção:** dados de identificação durante o contrato e por até 90 dias após seu término, janela para o cliente exportar relatórios. Os dados de emissões pertencem ao cliente e são exportáveis em CSV.
- **Segurança:** TLS em trânsito, criptografia em repouso gerenciada pelo Google Cloud, acesso por papéis e isolamento por tenant.
- **Direitos do titular:** acesso, correção e eliminação ou anonimização, atendidos pelo administrador da conta do cliente com apoio da plataforma.
- **MVP e demonstração:** somente dados sintéticos. As entrevistas são salvas sem nome, CPF ou telefone e com consentimento.

## 11. Planejamento/calendário

- **Checkpoint 1, Fundação (18/10).** Este documento, Ishikawa, backlog MoSCoW com critérios mensuráveis, 2 entrevistas ou evidências secundárias rotuladas, hipótese de defensibilidade, ADRs, OpenAPI v0.1, Definition of Done, Docker Compose com API, frontend e emuladores, CI com lint e build, seed reproduzível, .env.example, alertas de orçamento e teste de isolamento de tenant escrito. Itens específicos do projeto: ficha metodológica e dataset de fatores v1.
- **Checkpoint 2, Núcleo funcional (01/11).** 3 a 5 jornadas ponta a ponta rodando localmente, Security Rules deny-all com testes, EP/BVA v1, matriz de rastreabilidade e gasto de até US$ 5.
- **Checkpoint 3, Nuvem e integração (15/11).** Deploy no Firebase Hosting e Cloud Run, health checks, logs JSON, Workload Identity Federation e gasto de até US$ 12.
- **Checkpoint 4, Estabilização (29/11).** Pipeline bloqueante, cobertura e Lighthouse nas metas, rollback ensaiado e relatório de custos.
- **Demo Day (05/12).** Homologação e apresentação.

## 12. Glossário

- **tCO2e:** tonelada de CO2 equivalente, unidade que soma todos os gases ponderados pelo GWP.
- **Escopo 1, 2 e 3:** emissões diretas (combustão própria e gases refrigerantes), emissões da eletricidade comprada e demais emissões indiretas da cadeia.
- **Fator de emissão:** quanto de gás é emitido por unidade de atividade, por exemplo kg de CO2 por kWh.
- **GWP:** potencial de aquecimento global, que converte cada gás em CO2 equivalente.
- **Abordagem por localização:** método de Escopo 2 que usa o fator médio da rede elétrica onde a energia é consumida.
- **Tenant:** cada empresa cliente do sistema. Os dados de uma nunca aparecem para outra.
- **Revisão:** nova versão de um lançamento que corrige o anterior sem apagá-lo.
- **Recibo de cálculo:** registro das entradas, da versão do fator, da fórmula, do resultado e do hash de um cálculo.
- **Escore z modificado:** medida de desvio baseada em mediana e MAD, pouco sensível a valores extremos.

## 13. Referências

- GHG Protocol: Corporate Standard, Scope 2 Guidance (2015, em revisão) e Corporate Value Chain (Scope 3) Standard.
- Programa Brasileiro GHG Protocol (FGV e WRI): ferramenta de cálculo e Registro Público de Emissões.
- MCTI: fatores de emissão médios mensais do Sistema Interligado Nacional.
- IPCC: relatórios de avaliação AR5 e AR6 (valores de GWP).
- Lei 13.709/2018 (LGPD).

