# ADR-001: Stack principal

## Status

Registro original: Proposto (16/10/2026) - Autor: Tech Lead (Grupo 05).
Revisado em 23/10/2026 conforme [ADR-008](ADR-008-recorte-crud.md). A stack é mantida; implementação e operação devem ser comprovadas pelas entregas.

## Contexto

O CarbonTrace precisa entregar cinco jornadas em um ciclo curto, com cálculo reproduzível, precisão decimal e baixo custo de infraestrutura. A base existente já utiliza Python/FastAPI, React/TypeScript e Firebase. Mudar de linguagem, framework ou banco durante o recorte consumiria tempo de integração e testes sem resolver a complexidade principal, que estava nos perfis e fluxos de negócio.

A estimativa de US$ 3 a US$ 8 é uma hipótese de infraestrutura, com teto de US$ 50 para o ciclo, não um custo já comprovado. A escolha deve permitir desenvolvimento com emuladores, implantação em GCP/Firebase e manutenção por uma equipe pequena. Precisão depende da forma de representar/calcular os valores, não apenas da linguagem escolhida.

## Decisão

Manter **Python com FastAPI** na API, **decimal.Decimal** no domínio e **React/TypeScript/Vite** na interface. Usar **Cloud Run** para API e **Firebase Hosting** para frontend; **Firebase Authentication** para contas e **Firestore** para persistência. Desenvolvimento local com **Docker Compose e Auth/Firestore Emulator**, mantendo **GitHub Actions** como CI previsto.

Essa escolha reaproveita o código existente e permite tratar decimais com a biblioteca padrão, sem uma migração de stack. PDF/CSV serão gerados síncronos a partir de snapshots, sem introduzir jobs ou Storage obrigatório. A biblioteca de PDF é detalhe da implementação, a validar pela equipe.

Alternativas avaliadas:
- **Node.js/TypeScript:** poderia atender precisão com uma biblioteca decimal, mas a troca da API exigiria migração e repetição de testes. Não é rejeitado por incapacidade técnica.
- **Java/BigDecimal ou C#/.NET decimal:** também são viáveis, porém uma mudança agora amplia trabalho de implantação e manutenção; não há benchmark do projeto que demonstre vantagem suficiente.
- **Novos serviços para filas, jobs e arquivos permanentes:** adiados porque relatórios síncronos e snapshots já atendem o MVP reduzido.

Em 25/10/2026, o planejamento concentra o CP2 no ambiente local. Cloud Run/Hosting, Workload Identity e operação em nuvem continuam escolhas para os marcos posteriores; não são tarefas extras das 10 issues atuais. DevOps prepara Compose/CI/custos, Backend e Frontend implementam o núcleo e TL revisa.

## Consequências

- **Fica mais fácil:** Reutilizar a base atual, compartilhar ambiente local reproduzível e concentrar esforço nas jornadas, precisão e histórico.
- **Fica mais difícil:** Manter contratos coerentes entre TypeScript/Python e evitar conversões inadvertidas de strings decimais em números binários na interface.
- **Riscos e mitigação:** Cold start, custo de leituras e dependências podem afetar a operação. Medir latência/gasto, limitar consultas e validar imagens/dependências nos marcos previstos; a documentação não comprova resultados.
- **Gatilho de revisão:** Mudança de requisitos, custo acima dos limites ou desempenho insuficiente demonstrado por medição. Preservar a referência anterior de observar cold start p95 acima de 3 segundos como sinal de investigação, não como resultado medido.
