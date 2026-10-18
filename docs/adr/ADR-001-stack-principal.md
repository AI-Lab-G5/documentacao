# ADR-001: Stack principal

## Status
Proposto (16/10/2026) - Autor: Tech Lead (Grupo 05)

## Contexto
O projeto CarbonTrace (ciclo de 9 semanas, orçamento de US$ 3–8) requer uma API que calcule emissões com precisão absoluta, proibindo o uso de ponto flutuante. A linguagem da API deve suportar decimais exatos nativamente. Além disso, precisamos definir a infraestrutura de alojamento (GCP), o frontend e o ambiente de CI/CD para uma equipa de estudantes.

## Decisão
Vamos usar **Python (FastAPI)** como linguagem da API. A biblioteca padrão `decimal` garante o cumprimento do requisito de precisão exata. A API será alojada no **GCP Cloud Run**, e o frontend (React) no **Firebase Hosting**. O ambiente local usará **Docker Compose** integrando os emuladores locais do Firebase (Auth e Firestore). No GitHub Actions (CI), correremos validações de *lint*, testes unitários/integração e *build* da imagem a cada Pull Request.

Alternativas avaliadas para a API:
- **JavaScript/TypeScript (Node.js):** Rejeitada. Requer bibliotecas de terceiros (ex: `decimal.js`) para decimais exatos, violando a exigência técnica obrigatória.
- **Java (Spring Boot) com `BigDecimal`:** Rejeitada. Suporta decimal exato, mas apresenta maior consumo de memória e *cold starts* lentos no Cloud Run, dificultando a gestão do orçamento.
- **C# (.NET) com `decimal`:** Rejeitada. Suporta decimal exato e tem bom desempenho, mas a equipa tem maior familiaridade presumida com Python, e o ecossistema GCP/Firebase SDK para Python é mais adequado para *scripts* de dados e relatórios (PDF/CSV).

## Consequências
- **Fica mais fácil:** Manipular operações matemáticas complexas sem perda de precisão através do módulo nativo do Python, bem como a geração de ficheiros CSV/PDF com bibliotecas maduras (ex: `pandas`, `ReportLab`).
- **Fica mais difícil:** A tipagem em Python exige maior rigor estático (usando `mypy` e Pydantic) para garantir a segurança dos contratos.
- **Riscos e mitigação:** Tempos de *cold start* no Cloud Run. Mitigado pela escolha do FastAPI (leve) e pelo uso do Gunicorn/Uvicorn.
- **Gatilho de revisão:** Se o tempo de *cold start* exceder 3 segundos no percentil 95, dificultando a experiência do utilizador no frontend.