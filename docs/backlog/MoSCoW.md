# Backlog MoSCoW — CarbonTrace

23/10/2026 · [Visão](../visao-de-produto.md) · [ADR-008](../adr/ADR-008-recorte-crud.md).
Critérios são planejados, não resultados. Perfil único funcionario, acesso às unidades do próprio tenant. Must compreende J1–J5 até 05/12; as 20 issues CP2 cobrem J1–J4.

Consultar a [matriz planejada de rastreabilidade](rastreabilidade.md); atualizar com testes/PR/CI reais durante a implementação.

## Correspondência dos IDs

IDs históricos não são reutilizados com outro significado. Prefixo M/S/C indica origem; a prioridade vigente está abaixo.

| ID | Prioridade atual | Revisão |
| --- | --- | --- |
| CT-M01 | Must | Código/nome/fontes fixas; sem área/vínculo obrigatório |
| CT-M02 | Must | Perfil único e isolamento por empresa; sem restrição individual por unidade |
| CT-M03 | Must | Coleta validada, conversões delimitadas |
| CT-M04 | Must | Fatores imutáveis preparados pela equipe, sem publicação pela aplicação |
| CT-M05 | Must | Cálculo/reprodução; sem hash nem escolha de GWP |
| CT-M06 | Fora deste MVP | Aprovação, fechamento e reabertura adiados; ID reservado |
| CT-M07 | Must | Revisão sem fechamento/aprovação |
| CT-M08 | Must | Consolidado corrente/snapshot, sem versão de fechamento |
| CT-M09 | Must | Detalhe rastreável, sem recibo criptográfico |
| CT-M10 | Must | PDF/CSV do mesmo snapshot, sem jobs/manifesto |
| CT-M11 | Must — novo | Cancelamento lógico com histórico |
| CT-M12 | Must — novo | Ranking absoluto e alerta operacional |
| CT-S01 | Fora deste MVP | Ponte de variação adiada |
| CT-S02 | Fora deste MVP | Intensidade/MAD adiados; comparação básica migra para CT-M12 |
| CT-S03 | Should | Plausibilidade histórica/qualidade automática após Must |
| CT-S04 | Could | Importação de unidades por CSV após Must |
| CT-C01 | Fora deste MVP | Auditor temporário adiado |
| CT-C02 | Fora deste MVP | Vínculo/abordagem/filtros adicionais adiados |
| CT-C03 | Fora deste MVP | Escolha AR5/AR6 adiada; metodologia fixa |
| CT-W01–CT-W10 | Fora deste MVP | Exclusões originais mantidas |

## Must — critérios de aceite

### CT-M01 — Unidades e fontes (J1)

Como funcionário, quero organizar unidades da minha empresa.

1. Cadastrar/editar código e nome não brancos; desativar sem apagar histórico. Código repetido no tenant: 409 e zero duplicatas; em outro tenant permitido.
2. Vincular/desativar uma fonte por tipo: eletricidade, diesel de frota, gasolina de frota, GLP estacionário. Outros tipos rejeitados; sem área/vínculo obrigatório.
3. Listas paginadas: padrão 20/máximo 100. UI com carregando, vazio, erro e sucesso.
4. Desativação bloqueia nova coleta, preserva dados existentes e não subtrai consumos silenciosamente.

### CT-M02 — Login e isolamento

Como funcionário, quero acessar as unidades da minha empresa com conta individual.

1. Toda rota protegida exige token: ausente/inválido/expirado →401; sem tenant ou papel permitido →403. Somente papel funcionario; sem privilégios diferentes na empresa.
2. Massa com duas empresas e duas contas por empresa: todas as tentativas entre empresas retornam 403/404, zero dados e zero alterações, inclusive por troca de IDs.
3. Funcionários do mesmo tenant acessam todas as unidades dele. A restrição antiga por unidade foi retirada.
4. Firestore direto deny-all para anônimo/autenticado; API via Admin SDK mantém autorização própria. Provisionamento pela equipe, sem gestão de usuários na interface.

### CT-M03 — Coleta validada (J2)

Como funcionário, quero preencher fonte, competência, quantidade e unidade.

1. YYYY-MM válido, fonte ativa, quantidade decimal finita ≥0; eletricidade kWh/MWh, diesel/gasolina L, GLP kg. Ausência não vira zero; inválido →422 e zero gravação.
2. Guardar empresa, unidade, fonte, competência, quantidade/unidade originais, UID, instante e revisão 1. Um lançamento lógico por unidade/fonte/competência; nova entrada duplicada →409, inclusive após cancelamento.
3. Mesma Idempotency-Key/payload retorna o recurso anterior, sem nova revisão/evento. Chave reutilizada com outro payload →409, janela 24h. Sem hash criptográfico obrigatório.
4. Massa de 20 erros de unidade identificáveis: sinalizar ≥19 antes da gravação. Não presumir detectar quantidade errada com unidade válida. Meta de usabilidade posterior: cada um de 3 participantes registra 4 fontes em ≤5 minutos.

### CT-M04 — Fatores imutáveis (J3)

Como funcionário, quero saber o fator utilizado sem administrá-lo na aplicação.

1. Equipe prepara A/B com ID, vigência YYYY-MM, fonte, valor decimal/unidade, escopo, referência e status preliminary/verified. Metodologia fixa identifica as premissas ambientais.
2. Aplicação apenas consulta fatores; sem publicação/edição/exclusão de versão. Corrigir fator cria outra versão. B não altera cálculo salvo com A.
3. Novo cálculo escolhe versão compatível mais recente vigente na competência; sem compatibilidade →422 e nenhum resultado válido. Procedimento de carga rejeita conjunto sem campos obrigatórios.
4. Identificar dados sintéticos/preliminares na consulta e no relatório. Referência e condições de uso devem ser conferidas antes de alegar validação.

### CT-M05 — Cálculo e reprodução (J3)

Como funcionário, quero calcular e conferir um resultado anterior.

1. Fator sintético 0,5 kgCO2e/kWh: 1 MWh e 1.000 kWh retornam exatamente 500 kgCO2e. Outras fontes usam suas unidades fixas; incompatíveis rejeitados.
2. Salvar snapshot imutável com entradas/revisões, configuração de fontes esperadas, conversões, fatores/versões, metodologia, UID, datas e resultados.
3. Recalcular o mesmo snapshot 100 vezes, após B ou correção, produz diferença zero com entradas/fatores originais. Referências explícitas bastam, sem hash.
4. Decimal no domínio, strings em JSON/banco, soma exata e apresentação em 3 casas ROUND_HALF_EVEN. Não arredondar intermediários.
5. Estimativa reduzida não equivale a inventário completo homologado. Casos sintéticos comprovam comportamento; referências ambientais oficiais exigem conferência específica.

### CT-M07 — Correção retroativa (J2/J3)

Como funcionário, quero corrigir preservando a origem.

1. Corrigir 100→120 exige motivo não branco e cria revisão 2 ligada à 1 com autor/data; revisão 1 continua consultável com 100.
2. Corrigir quantidade/unidade compatível, sem trocar unidade/fonte/competência identificadoras. Identificação errada exige cancelar e cadastrar em outra chave.
3. Revisão-base desatualizada →409; reenvio idempotente não cria revisão extra. Novo cálculo usa corrente; anterior continua usando a entrada original.
4. Sem aprovação, período fechado ou reabertura. Fator unitário sintético mantém resultados 100 e 120 consultáveis, sem duplicar totais.

### CT-M08 — Consolidação (J4)

Como funcionário, quero acompanhar a rede por mês, unidade e escopo.

1. Somar uma única revisão corrente válida/não cancelada por lançamento. Dados parciais são rotulados; pendências identificam unidade/fonte, sem converter ausência em zero.
2. Snapshot selecionado usa apenas seu conjunto salvo; nova execução não altera anteriores.
3. Em 1.000 cenários com semente registrada, incluindo frações, revisões e cancelamentos, soma exata das unidades = total. Mostrar diferença de arredondamento quando houver.
4. Linhas paginadas 20/100; total e mediana usam todo o snapshot autorizado, não só a página. Queries filtradas/limitadas; sem listener amplo sem justificativa.

### CT-M09 — Origem do resultado (J3/J5)

Como funcionário, quero explicar de onde veio o número.

1. Exibir snapshot, revisão original, valor/unidade, conversão, fórmula, fator/versão/referência/status, metodologia, resultado, UID e datas.
2. Consultar origem de pelo menos 5 resultados sintéticos conhecidos, inclusive anteriores a correção/cancelamento.
3. Sem SHA-256, assinatura, manifesto ou infraestrutura separada de auditoria. Revisões/snapshots ligados formam a trilha mínima.

### CT-M10 — PDF/CSV (J5)

Como funcionário, quero baixar o mesmo recorte em dois formatos.

1. Ambos recebem calculationId salvo; identificam empresa, competência, snapshot, unidade/fonte, entradas/revisões, conversões, fatores, metodologia, UID e datas.
2. Após correção/cancelamento/B, reexportação do snapshot A preserva dados; data de geração do arquivo pode mudar.
3. Síncrono, sem fechamento, aprovação, jobs ou Storage obrigatório. Indicar pendências e preliminaridade; PDF não certifica. Vazio: CSV com cabeçalho e PDF explicativo.
4. Must até 05/12/2026; implementação não faz parte das 20 issues do CP2. Planejamento posterior deve ser explicitado.

### CT-M11 — Cancelamento lógico (novo, J2)

Como funcionário, quero retirar lançamento errado sem apagar a origem.

1. Interface confirma e avisa a limitação de reativação. API exige motivo/revisão-base e cria revisão cancelled, com autor/data, ligada à anterior.
2. Corrente exclui dos totais; consultas mostram cancelamento e histórico. Snapshot anterior permanece. Reenvio idempotente não cria nova revisão.
3. Sem DELETE físico nem reativação pela interface. A chave unidade/fonte/competência permanece reservada após cancelar.

### CT-M12 — Ranking e mediana (novo, J4)

Como funcionário, quero comparar totais da minha empresa.

1. Apenas unidades ativas com ≥1 fonte esperada e coleta completa no snapshot. Zero explícito vale. Ordenar kgCO2e decrescente, desempate por código, sem m².
2. Com ≥3 completas e mediana >0, desvio=(valor−mediana)/mediana×100. Alerta só se abs(desvio)>50, calculado antes do arredondamento; ±50 não alertam.
3. Menos de 3 ou mediana zero: nenhuma classificação e aviso. Incompletas continuam visíveis como pendências.
4. Indicador operacional, sem alegação estatística/eficiência. Exemplo 100/100/200: mediana 100, só 200 alerta; testar limites e zeros.

## Should, Could e exclusões

CT-S03 (Should): plausibilidade/qualidade automática além da validação obrigatória, somente após Must. CT-S04 (Could): importar unidades por CSV após Must e avaliação de capacidade. Não integram as 20 issues.

Adiadas: CT-M06 fechamento/aprovação/reabertura; CT-S01 ponte; CT-S02 intensidade/MAD; CT-C01 auditor; CT-C02 abordagem/vínculo; CT-C03 escolha GWP.

Exclusões originais: CT-W01 Escopo 3; CT-W02 OCR; CT-W03 concessionárias; CT-W04 certificação; CT-W05 créditos; CT-W06 Escopo 2 mercado; CT-W07 ERP; CT-W08 anexos; CT-W09 app nativo; CT-W10 benchmark entre clientes. Adicionalmente: refrigerantes, gestão de usuários, publicação de fatores pela aplicação e infraestrutura criptográfica.

Critérios negativos: contrato/catálogo não oferecem esses recursos; fonte não suportada é rejeitada sem persistência; relatórios não afirmam inventário de fontes/escopos excluídos nem certificação; agregados nunca incluem outro tenant.

## Marcos, qualidade e dependências

CP2: 3–5 jornadas locais (planejadas J1–J4), OpenAPI atualizada, quatro estados UI, deny-all, testes sem token/inválido/expirado/papel sem permissão/tenant incorreto/acesso indevido, EP/BVA, contrato/prioridades, matriz e primeiro relatório em /docs; custo ≤US$5, paginação e queries controladas. Não contar teste pulado como aprovado.

Exigências posteriores do caderno permanecem: publicação/operação e custo ≤US$12 no CP3; cobertura ≥80% domínio/≥60% global, Lighthouse ≥90, p95 leitura ≤800ms/escrita ≤1,5s, segurança, rollback e gates no CP4. Alterações dependem do tutor.

Dependências: J1→J2→J3→J4; correção/cancelamento produz novo resultado corrente. J5 depende de snapshots/origem J3/CT-M09, não de CT-M06. Cada conclusão exige aceite/teste/PR/CI/documentação reais. As milestones preservam 10 issues por semana e duas por integrante.
