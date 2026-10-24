# ADR-005: Versionamento imutável de fatores e metodologia

## Status

Registro original: Proposto (16/10/2026) - Autor: Tech Lead (Grupo 05).
Revisado em 23/10/2026 pelo [ADR-008](ADR-008-recorte-crud.md), mantendo versionamento e retirando publicação pela aplicação, hashes e fechamento.

## Contexto

O mesmo consumo pode gerar outro número quando o fator é substituído. Se a aplicação buscar sempre a versão mais recente, um relatório anterior deixa de ser reproduzível e o usuário perde a explicação da diferença.

A proposta original exige recalcular um período com seu fator original. Um CRUD com fatores sobrescritos não atende essa cena. Por outro lado, construir tela de publicação, permissões especiais e infraestrutura criptográfica amplia o trabalho além do necessário para o piloto.

## Decisão

Manter **versões imutáveis** preparadas pela equipe, com conjuntos A/B e metodologia fixa identificada. Cada fator registra tipo de fonte, escopo, unidade-base, valor decimal, referência, vigência e status preliminary/verified. A metodologia documenta premissas dos gases/GWP incorporados ao fator agregado, sem motor por gás ou escolha de GWP.

A aplicação consulta versões; não publica, edita ou exclui fatores. Corrigir fator exige nova versão pelo procedimento da equipe, que valida campos obrigatórios e não sobrescreve a anterior. Somente eletricidade, diesel, gasolina e GLP compõem o catálogo.

Novo cálculo escolhe a versão compatível mais recente vigente na competência; ausência produz 422, não zero válido. O snapshot guarda referências e valores utilizados, entradas/revisões, conversões e metodologia. Recalcular A usa A após B, correção ou cancelamento; atualizar o corrente exige novo snapshot.

Não exigir hash ou assinatura para esta prova: referências imutáveis, valores preservados e testes demonstram reprodução, sem alegar evidência criptográfica contra adulteração. Fatores sintéticos/preliminares ficam rotulados; a presença de um link não significa que a fonte e as condições de uso foram conferidas.

Alternativas descartadas:
- **Editar o fator no mesmo ID:** perde o significado da referência guardada e pode mudar relatórios antigos.
- **Usar sempre o fator mais novo:** confunde atualização do corrente com reprodução histórica.
- **Tela de publicação com perfil administrativo:** retirada porque a carga controlada pela equipe atende o piloto com menos interfaces/permissões.
- **Hash/assinatura obrigatórios:** adiados para não confundir rastreabilidade com infraestrutura criptográfica e certificação.

## Consequências

- **Fica mais fácil:** Explicar a origem matemática e distinguir mudança de consumo de mudança de fator, reproduzindo o resultado demonstrado.
- **Fica mais difícil:** Manter versões antigas, referências e procedimento de carga; o fator agregado limita a interpretação ambiental do resultado.
- **Riscos e mitigação:** Carga manual via Admin SDK pode sobrescrever dados ou usar fator inadequado. Validar somente inserção, revisar referências/condições de uso e testar A após B; não apresentar preliminares como homologados.
- **Gatilho de revisão:** Necessidade de novas fontes, seleção metodológica, gestão de fatores por usuários ou custo/volume de versões fora do esperado. Exige revisão de escopo, não alteração silenciosa do fator.
