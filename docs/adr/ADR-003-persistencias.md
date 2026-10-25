# ADR-003: Persistência

## Status

Registro original: Proposto (16/10/2026) - Autor: Tech Lead (Grupo 05).
Revisado em 23/10/2026 pelo [ADR-008](ADR-008-recorte-crud.md). Não há fechamento obrigatório, materialização periódica ou Cloud Storage obrigatório no recorte atual.

## Contexto

O banco precisa separar empresas, preservar consumos anteriores e permitir reconstruir um cálculo depois de correções ou mudança de fator. Usar sobrescrita ou exclusão física no CRUD destruiria a informação necessária para a cena de demonstração e para explicar a origem dos números.

A base já utiliza Firestore. Valores de domínio não podem depender de ponto flutuante, e o modelo precisa combinar dados correntes editáveis com evidências históricas imutáveis. Guardar tudo como append-only sem distinguir ponteiros/metadados também dificultaria operações comuns, como renomear uma unidade.

## Decisão

Manter **Firestore**, com coleções de dados organizadas por tenant e acesso somente pela API. Quantidades, fatores e emissões são persistidos como **strings decimais**, convertidas para Decimal ao calcular. Escolher esse formato preserva a representação entre JSON/Python/banco, conforme ADR-007.

Fatores publicados, revisões e snapshots são imutáveis. Um lançamento lógico é identificado por tenant/unidade/fonte/competência. Correção/cancelamento cria nova revisão com referência anterior, número, motivo, UID e instante; uma transação verifica a revisão-base e atualiza o ponteiro corrente. Não alterar a revisão antiga para marcá-la como substituída. Cancelamento preserva a chave e o histórico; não há reativação no MVP.

Metadados de unidade/fonte e ponteiros correntes podem ser atualizados. Desativar bloqueia nova coleta, sem apagar ou cancelar entradas existentes. O snapshot captura configuração esperada, revisões, conversões, fatores, metodologia, resultados exatos e autoria/data, permitindo consulta histórica independente do estado corrente.

Agregação considera o recorte completo e apenas uma revisão válida por lançamento. Totais/mediana do snapshot não são a soma de uma página da interface. Consultas são filtradas, limitadas e paginadas, sem listeners amplos sem justificativa. PDF/CSV são gerados síncronos do mesmo snapshot, sem manter arquivos permanentemente nem criar serviço separado de auditoria.

Alternativas descartadas:
- **Sobrescrever/apagar consumos:** simplifica o CRUD, mas inviabiliza explicar correções e reproduzir resultados anteriores.
- **Cloud SQL/PostgreSQL nesta etapa:** tipo NUMERIC e consultas relacionais são alternativas válidas; migrar agora exige infraestrutura/manutenção sem necessidade comprovada. Não se afirma preço atual como justificativa.
- **Inteiros escalados:** exigem convencionar escala por tipo e conversão; strings + Decimal reduzem essa gestão manual no contrato adotado.
- **Jobs periódicos, Storage e eventos de auditoria separados:** adiados porque snapshots/revisões e geração síncrona atendem o recorte com menos componentes.

Aplicação por etapa em 25/10/2026: o CP2 exige persistir entrada original, autoria/data, fator/versão/valor e resultado, sem substituir silenciosamente o cálculo anterior após B. Transações de correção concorrente, cancelamento, reserva de chave e snapshot completo da rede descrevem a evolução do MVP; não bloqueiam J1-J3. O histórico já existente deve ser preservado.

## Consequências

- **Fica mais fácil:** Reutilizar a persistência existente e consultar origens antigas sem reconstruir planilhas ou depender de arquivos externos.
- **Fica mais difícil:** Agregar strings com precisão exige cálculo na API e desenho explícito de transações/índices; histórico aumenta o volume armazenado.
- **Riscos e mitigação:** Admin SDK pode sobrescrever histórico; Rules deny-all não o impedem. Validar operações na API/procedimento de carga e testar imutabilidade, concorrência, cancelamento e isolamento.
- **Gatilho de revisão:** Leituras, armazenamento ou tempo de agregação medidos deixarem de atender custo/latência. Só então avaliar outra materialização ou banco.
