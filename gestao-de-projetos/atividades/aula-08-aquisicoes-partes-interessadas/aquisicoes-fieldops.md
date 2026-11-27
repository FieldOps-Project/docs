# 🛒 Gerenciamento das Aquisições — FieldOps

> Atividade da Aula 08 (Aquisições e Partes Interessadas) — Gestão de Projetos, Prof. Me. Deivison S. Takatu. Os valores desta tabela são **as mesmas linhas de custo direto do orçamento** (`../aula-05-custos-qualidade/orcamento-fieldops.md`) e as datas vêm do cronograma (`../aula-04-tempo/cronograma-fieldops.md`); a consistência foi conferida por script.

---

## 📌 Identificação

| Campo | Informação |
|---|---|
| **Projeto** | FieldOps — Plataforma de Inspeção em Campo (MVP) |
| **Gerente do projeto** | Felipe Marques (FM) |
| **Integrantes** | Felipe Marques; Gabriel Ribeiro Camargo Miranda; Pedro Henrique Dias de Paula; André dos Santos Fernandes; João Victor de Souza Santos |
| **Data de elaboração** | 18/11/2026 |
| **Versão** | 1.0 |
| **Itens a adquirir** | 14 (12 de caixa, 2 não-caixa) |
| **Total mais provável (cM)** | R$ 1.070,00 (R$ 374,00 de caixa + R$ 696,00 não-caixa) |
| **Valor esperado PERT** | R$ 1.391,58 |
| **Fornecedores** | Provedores de serviço (Expo/EAS, GitHub, hospedagem, banco, armazenamento) — parte interessada PI-12 |

> **Valores hipotéticos.** O TAP declara “sem investimento financeiro direto”: o cenário mais provável usa planos gratuitos sempre que possível e o plano pago só em semana de pico. Preços em US$ convertidos a **US$ 1,00 = R$ 5,50**. Preços e limites de planos mudam; **confirmar no site do fornecedor antes de contratar**.

---

## 📋 Tabela de aquisições

Formato da Aula 08. **Custo total (cM)** = quantidade × custo unitário no cenário mais provável. **Previsão de entrega** é a data da linha de base em que o item precisa estar disponível, ligada à atividade do cronograma que o consome. Urgência: 5-Muito alta a 1-Muito baixa.

| ID | Cód. WBS/EAP | Item a ser adquirido | Urgência | Motivo da decisão de compra | Fornecedores potenciais (plano gratuito → pago) | Qtde | Custo unitário | Custo total (cM) | Previsão de entrega | Observações |
|---|---|---|---|---|---|---:|---:|---:|---|---|
| P01 | 7.2 | Registro de domínio .com.br (1 ano) | 2-Baixa | Endereço estável para painel e API de demonstração; evita depender de URL de plataforma. | Registro.br; Hostinger; GoDaddy | 1 ano | R$ 40,00 | **R$ 40,00** | 13/11/2026 (antes de A60) | Registro.br cobra R$ 40,00/ano; cP cobre domínio .com em registrador internacional. Sem plano gratuito (usar URL da plataforma é a alternativa). |
| P02 | 7.2 | Hospedagem da API Spring Boot em contêiner (2 meses) | 4-Alta | RJ05/PBI-070: API publicada ou executável por contêiner para a demonstração; Docker Compose local não serve ao avaliador remoto. | Render (Free → Starter US$ 7/mês); Railway; Fly.io; AWS Lightsail | 2 meses | R$ 38,50 | **R$ 77,00** | 16/11/2026 (antes de A60) | Plano gratuito hiberna e tem cold start; adota-se o pago só na janela de demonstração. cO = gratuito basta; cP = 4 meses. |
| P03 | 7.2 | PostgreSQL gerenciado para o ambiente de demonstração (1 mês) | 4-Alta | Banco persistente fora do Docker local para o ambiente publicado (PBI-070); migrações Flyway já existem. | Neon (Free → Launch); Supabase (Free → Pro US$ 25/mês); Render Postgres | 1 mês | R$ 137,50 | **R$ 137,50** | 16/11/2026 (antes de A60) | Plano gratuito tem limite de armazenamento e pausa por inatividade. cO = gratuito; cP = 2 meses de plano pago. |
| P04 | 3.5 | Armazenamento de evidências compatível com S3 (3 meses) | 4-Alta | RP08: fotos das inspeções precisam de armazenamento de objetos com URL assinada; banco não deve guardar binários. | AWS S3 (free tier 5 GB por 12 meses); Cloudflare R2 (10 GB grátis); Backblaze B2 | 3 meses | R$ 5,00 | **R$ 15,00** | 02/10/2026 (antes de A31) | Volume estimado < 20 GB de fotos de teste; custo marginal. Necessário a partir de S5 (upload). |
| P05 | 7.2 | EAS Build (Expo) — plano pago em semana de pico (1 mês) | 5-Muito alta | RJ05/PBI-068: build Android assinado é o único caminho de instalação no dispositivo; fila gratuita pode atrasar M8. | Expo EAS (Free 30 builds/mês → Starter US$ 19/mês); build local com Gradle | 1 mês | R$ 104,50 | **R$ 104,50** | 16/11/2026 (antes de A57) | Plano gratuito usa fila de baixa prioridade; pago só na semana 15–16. Alternativa sem custo: build local. |
| P06 | 2.1 | Organização GitHub FieldOps-Project (4 repositórios, Actions e Projects) | 5-Muito alta | RJ03: controle de versão, PR com revisão, CI e quadro de sprints; já em uso desde 10/08. | GitHub Free/Student Pack (US$ 0); GitHub Team (US$ 4/usuário/mês); GitLab | 0 (usuário-mês; plano gratuito) | R$ 22,00 (se pago) | **R$ 0,00** | 10/08/2026 (antes de A01) | Cenário provável: plano gratuito (repositórios públicos, 5 membros). cP = Team para 5 usuários por 3 meses, se for necessária proteção de branch em repositório privado. |
| P07 | 7.2 | Hospedagem do painel web Next.js | 4-Alta | PBI-069: versão publicada do painel para o avaliador; Next.js 16 roda em plataforma serverless. | Vercel (Hobby grátis → Pro US$ 20/mês); Netlify; Cloudflare Pages | 0 (mês; plano gratuito) | R$ 110,00 (se pago) | **R$ 0,00** | 17/11/2026 (antes de A60) | Hobby atende (uso acadêmico, não comercial). Pro só se houver limite de build/banda. |
| P08 | 6.4 | Monitoramento de erros e logs (web, app e API) | 2-Baixa | PBI-064 e correção de falhas de campo: stack traces de crash do app em Android real. | Sentry (Developer grátis → Team US$ 26/mês); Firebase Crashlytics (grátis) | 0 (mês; plano gratuito) | R$ 143,00 (se pago) | **R$ 0,00** | 16/11/2026 (antes de A58) | Crashlytics/Sentry gratuitos cobrem o MVP; cP = 1 mês Team se estourar cota de eventos. |
| P09 | 6.3 | Dispositivos Android da equipe para teste (uso e depreciação) | 5-Muito alta | RJ05: validação obrigatória em Android real (câmera, QR, GPS, offline); emulador não cobre sensores. | Aparelhos próprios dos integrantes (não há compra) | 12 aparelho-meses | R$ 33,00 | **R$ 396,00** | 05/10/2026 (antes de A33) | Não-caixa. 3 aparelhos × 4 meses; valor médio R$ 1.188 em 36 meses = R$ 33,00 por aparelho-mês. |
| P10 | 6.3 | Aparelho Android de entrada para teste de desempenho (contingente) | 2-Baixa | Risco de o aparelho da equipe ser potente demais e esconder lentidão em listas/imagens (PBI-064). | Mercado Livre; Amazon; Magazine Luiza (modelo de entrada com Android 12+) | 0 (unidade; só se houver gatilho) | R$ 900,00 (se pago) | **R$ 0,00** | 13/11/2026 (antes de A58) | Só se o teste em aparelho de entrada falhar nos emprestados; avaliar empréstimo no laboratório do campus antes. |
| P11 | 7.2 | Conta Google Play Console (taxa única, contingente) | 1-Muito baixa | MVP usa distribuição interna (APK/EAS); só entra se o comitê exigir teste interno via Play Store. | Google Play Console (US$ 25, taxa única) | 0 (conta; só se houver gatilho) | R$ 137,50 (se pago) | **R$ 0,00** | 16/11/2026 (antes de A57) | Fora do escopo-base; reserva de contingência cobre se a demanda surgir. |
| P12 | 6.3 | Teste em dispositivos Android em nuvem (contingente) | 2-Baixa | Complementa os aparelhos da equipe em modelos/versões de Android diferentes antes do M8. | Firebase Test Lab (cota diária grátis); BrowserStack App Live | 0 (mês; só se houver gatilho) | R$ 110,00 (se pago) | **R$ 0,00** | 19/11/2026 (antes de A61) | Usa só a cota gratuita no cenário provável. cP = 1 mês pago. |
| P13 | 2.4 | Ferramentas de desenvolvimento e design (VS Code, Android Studio, Docker Desktop, IntelliJ, Postman, Figma Education) | 3-Média | Ambiente padronizado (PBI-004/005) sem licenças pagas; todas com plano gratuito ou educacional. | JetBrains (licença estudante); Docker (Personal); Figma Education; Microsoft | 5 licenças | R$ 0,00 | **R$ 0,00** | 10/08/2026 (antes de A01) | R$ 0,00: licenças educacionais. Listada para rastreabilidade (nenhum pagamento). |
| P14 | 2.4 | Internet e energia (rateio do ambiente de trabalho dos integrantes) | 3-Média | Infraestrutura necessária ao desenvolvimento remoto; custo de oportunidade, sem desembolso do projeto. | Operadoras residenciais/campus (infraestrutura existente) | 20 pessoa-meses | R$ 15,00 | **R$ 300,00** | 10/08/2026 (antes de A01) | Não-caixa. 5 pessoas × 4 meses × R$ 15,00 (hipótese de rateio). Pode ser dado em laboratório do campus. |
| | | **Total mais provável (cM)** | | | | | | **R$ 1.070,00** | | R$ 374,00 de caixa e R$ 696,00 não-caixa |

**Situação das datas em 18/11/2026.** A linha de base de P01, P02, P03, P05 e P11 vence entre 13/11 e 16/11, antes de hoje; o cronograma mostra EAS e deploy sem avanço, então **confirmar com o gerente quais foram de fato contratados**. P06, P13 e P14 já estão em uso desde 10/08 (sem desembolso).

### Estimativa de três pontos e ligação com o orçamento

Cada aquisição é uma linha do orçamento (coluna *Linha*). **cO** = melhor caso (em geral, plano gratuito basta); **cM** = mais provável; **cP** = pior caso (upgrade de plano ou compra contingente). **Esperado = (cO + 4·cM + cP)/6.**

| ID | Linha do orçamento | Item | Tipo | cO | cM | cP | Esperado PERT | Origem do custo | Plano pago de referência |
|---|---|---|---|---:|---:|---:|---:|---|---|
| P01 | O43 | Registro de domínio .com.br (1 ano) | Caixa | R$ 40,00 | R$ 40,00 | R$ 70,00 | **R$ 45,00** | Desembolso | Registro.br R$ 40,00/ano |
| P02 | O44 | Hospedagem da API Spring Boot em contêiner (2 meses) | Caixa | R$ 0,00 | R$ 77,00 | R$ 154,00 | **R$ 77,00** | Desembolso | Render Starter US$ 7/mês |
| P03 | O45 | PostgreSQL gerenciado para o ambiente de demonstração (1 mês) | Caixa | R$ 0,00 | R$ 137,50 | R$ 275,00 | **R$ 137,50** | Desembolso | Supabase Pro US$ 25/mês |
| P04 | O17 | Armazenamento de evidências compatível com S3 (3 meses) | Caixa | R$ 0,00 | R$ 15,00 | R$ 40,00 | **R$ 16,67** | Desembolso | S3/R2/B2 pay-as-you-go (~US$ 0,015 a 0,023 por GB-mês) |
| P05 | O46 | EAS Build (Expo) — plano pago em semana de pico (1 mês) | Caixa | R$ 0,00 | R$ 104,50 | R$ 209,00 | **R$ 104,50** | Desembolso | EAS Starter US$ 19/mês |
| P06 | O06 | Organização GitHub FieldOps-Project (4 repositórios, Actions e Project… | Caixa | R$ 0,00 | R$ 0,00 | R$ 330,00 | **R$ 55,00** | Desembolso | GitHub Team US$ 4/usuário/mês |
| P07 | O47 | Hospedagem do painel web Next.js | Caixa | R$ 0,00 | R$ 0,00 | R$ 110,00 | **R$ 18,33** | Desembolso | Vercel Pro US$ 20/mês |
| P08 | O40 | Monitoramento de erros e logs (web, app e API) | Caixa | R$ 0,00 | R$ 0,00 | R$ 143,00 | **R$ 23,83** | Desembolso | Sentry Team US$ 26/mês |
| P09 | O36 | Dispositivos Android da equipe para teste (uso e depreciação) | Não-caixa | R$ 396,00 | R$ 396,00 | R$ 495,00 | **R$ 412,50** | Custo de oportunidade | — |
| P10 | O37 | Aparelho Android de entrada para teste de desempenho (contingente) | Caixa | R$ 0,00 | R$ 0,00 | R$ 900,00 | **R$ 150,00** | Desembolso | Aparelho de entrada ≈ R$ 900 |
| P11 | O48 | Conta Google Play Console (taxa única, contingente) | Caixa | R$ 0,00 | R$ 0,00 | R$ 137,50 | **R$ 22,92** | Desembolso | Play Console US$ 25 (taxa única) |
| P12 | O38 | Teste em dispositivos Android em nuvem (contingente) | Caixa | R$ 0,00 | R$ 0,00 | R$ 110,00 | **R$ 18,33** | Desembolso | Firebase Test Lab/BrowserStack ≈ US$ 20/mês |
| P13 | O10 | Ferramentas de desenvolvimento e design (VS Code, Android Studio, Dock… | Caixa | R$ 0,00 | R$ 0,00 | R$ 0,00 | **R$ 0,00** | Desembolso | — |
| P14 | O11 | Internet e energia (rateio do ambiente de trabalho dos integrantes) | Não-caixa | R$ 300,00 | R$ 300,00 | R$ 360,00 | **R$ 310,00** | Custo de oportunidade | — |
| | | **Total** | | **R$ 736,00** | **R$ 1.070,00** | **R$ 3.333,50** | **R$ 1.391,58** | | |

O total de cM (R$ 1.070,00) e o esperado PERT (R$ 1.391,58) são exatamente a soma das linhas O06, O10, O11 e demais linhas de custo direto do orçamento (verificado por script). **Caixa**: R$ 374,00 no cenário provável, R$ 669,08 esperado e R$ 2.478,50 no pior caso conjunto. O pior caso conjunto passa do teto de R$ 1.500 admitido para o projeto só por causa dos dois itens contingentes (P10, R$ 900,00, e P11, R$ 137,50), que dependem de um gatilho; sem eles o pior caso de caixa é R$ 1.441,00.

---

## ⚖️ Estratégia fazer ou comprar (make-or-buy)

| Componente | Decisão | Justificativa |
|---|---|---|
| API REST, regras de negócio, sincronização idempotente, autorização | **Fazer** | É o produto e o objetivo de aprendizagem (RP05–RP10). Nenhum serviço pronto substituiria a regra de snapshot, outbox e revisão. |
| Painel administrativo (Next.js) e app (Expo/React Native) | **Fazer** | RP01 e RP04: o enunciado exige interface própria (não vale Swagger nem acesso direto ao banco). |
| Frameworks e bibliotecas (Spring Boot, Next.js, Expo, expo-sqlite, Flyway, TanStack Query) | **Reutilizar código aberto** | Licenças permissivas e gratuitas; reduzem esforço sem aquisição. Registrar versão e licença no README (PBI-067). |
| Autenticação | **Fazer** (JWT próprio) | Escopo acadêmico de RP07; evita dependência de provedor de identidade e de custo por usuário. |
| Hospedagem da API, PostgreSQL, armazenamento S3, painel web | **Contratar (serviço)** | Operar servidor próprio não cabe em 12 h/semana por pessoa; planos gratuitos/baixo custo atendem à demonstração (P02, P03, P04, P07). |
| Build Android | **Contratar (EAS)** com plano B local | EAS é o caminho oficial do Expo e do roteiro da disciplina; build local via Gradle reduz a dependência (P05). |
| CI/CD e controle de versão | **Contratar (plano gratuito)** | GitHub Actions e Projects já em uso (P06). |
| Dispositivos Android | **Usar recursos próprios** + compra contingente | RJ05 exige aparelho real; 3 aparelhos da equipe cobrem o MVP (P09), com compra só se o teste de desempenho falhar (P10). |
| Monitoramento, domínio | **Contratar (plano gratuito ou taxa anual)** | Valor baixo e cobertura boa de crash no app (P08, P01). |

Resultado: o que gera valor para a disciplina é feito; o que é infraestrutura de demonstração é contratado pelo menor custo possível.

## 📑 Tipos de contrato por item

Quase todas as aquisições são **termos de uso de serviço com adesão online** (click-through): não há negociação de preço nem de cláusulas. Escolhe-se o tipo de cobrança.

| Tipo | Itens | Característica | Risco para o projeto |
|---|---|---|---|
| **Preço fixo (anuidade)** | P01 domínio | Valor certo por 12 meses | Renovação esquecida; sem estorno |
| **Assinatura mensal** | P02, P03, P05, P07, P08 (se pagos) | Cobrança recorrente; cancelável a qualquer momento | Esquecer de cancelar após o M8; cobrança por cartão de um integrante |
| **Pay-as-you-go (uso)** | P04 armazenamento, P12 teste em nuvem | Paga o que consome; cota gratuita | Estouro de cota gera cobrança surpresa; usar alerta de gasto |
| **Taxa única** | P11 Google Play | Pagamento único por conta | Só vale se a Play Store for exigida |
| **Compra de bem** | P10 aparelho | Preço fechado; sem recorrência | Imobiliza caixa; avaliar empréstimo antes |
| **Licença gratuita/educacional** | P06 (plano grátis), P13 | R$ 0,00 | Mudança de política do plano; renovação anual do Student Pack |
| **Recurso próprio (não-caixa)** | P09, P14 | Sem contrato | Indisponibilidade do aparelho (quebra, empréstimo) |

Não há contratos de **tempo e material** nem de **custo reembolsável**: não há fornecedor de serviço profissional.

---

## 🎯 Critérios de seleção de fornecedores (com pesos)

| Critério | Peso |
|---|---:|
| Custo total no período do projeto | 25% |
| Plano gratuito e limites adequados ao MVP | 20% |
| Aderência técnica à stack (Docker, PostgreSQL, S3, Expo) | 20% |
| Confiabilidade, SLA e suporte | 15% |
| Facilidade de adoção e documentação | 10% |
| Portabilidade (risco de lock-in) | 10% |
| **Total** | **100%** |

Notas de 1 a 5 atribuídas pelo gerente com base em documentação pública e uso prévio da equipe (**premissa subjetiva, a validar com o grupo**). Nota ponderada = Σ (nota × peso) ÷ 100.

**Hospedagem da API em contêiner (P02)**

| Fornecedor | Custo (25%) | Plano (20%) | Aderência (20%) | Confiabilidade, (15%) | Facilidade (10%) | Portabilidade (10%) | **Nota ponderada** |
|---|---:|---:|---:|---:|---:|---:|---:|
| Render | 4 | 3 | 5 | 3 | 5 | 4 | **3,95** |
| Railway | 3 | 2 | 5 | 3 | 4 | 4 | **3,4** |
| Fly.io | 4 | 2 | 4 | 3 | 3 | 4 | **3,35** |

→ Melhor nota: **Render**.

**PostgreSQL gerenciado (P03)**

| Fornecedor | Custo (25%) | Plano (20%) | Aderência (20%) | Confiabilidade, (15%) | Facilidade (10%) | Portabilidade (10%) | **Nota ponderada** |
|---|---:|---:|---:|---:|---:|---:|---:|
| Neon | 4 | 4 | 5 | 3 | 5 | 5 | **4,25** |
| Supabase | 3 | 4 | 4 | 4 | 5 | 4 | **3,85** |
| Render PostgreSQL | 3 | 2 | 5 | 3 | 4 | 4 | **3,4** |

→ Melhor nota: **Neon**.

**Armazenamento compatível com S3 (P04)**

| Fornecedor | Custo (25%) | Plano (20%) | Aderência (20%) | Confiabilidade, (15%) | Facilidade (10%) | Portabilidade (10%) | **Nota ponderada** |
|---|---:|---:|---:|---:|---:|---:|---:|
| Cloudflare R2 | 5 | 5 | 4 | 4 | 4 | 4 | **4,45** |
| AWS S3 | 4 | 3 | 5 | 5 | 4 | 4 | **4,15** |
| Backblaze B2 | 5 | 4 | 4 | 3 | 3 | 4 | **4,0** |

→ Melhor nota: **Cloudflare R2**.

**Build Android (P05)**

| Fornecedor | Custo (25%) | Plano (20%) | Aderência (20%) | Confiabilidade, (15%) | Facilidade (10%) | Portabilidade (10%) | **Nota ponderada** |
|---|---:|---:|---:|---:|---:|---:|---:|
| Expo EAS (gratuito/Starter) | 4 | 4 | 5 | 3 | 5 | 3 | **4,05** |
| Build local (Gradle/Android Studio) | 5 | 5 | 3 | 1 | 2 | 5 | **3,7** |

→ Melhor nota: **Expo EAS (gratuito/Starter)**.

**Decisão e ressalva.** Os cenários de custo do orçamento listam os três fornecedores de cada item porque o preço é parecido; a escolha final segue a nota ponderada, exceto onde o requisito do professor manda: RP08 pede armazenamento *compatível com S3*, e R2 e S3 satisfazem; o Build Android usa EAS com build local como plano B (nota menor, mas é o plano de contingência por não ter dependência externa). A diferença de notas entre hospedagens é pequena, então vale também o que a equipe já conhece.

---

## 🧾 Condução das aquisições (Aula 08): como cada elemento se aplica

| Elemento | Aplicação ao FieldOps (contexto acadêmico) | Como será verificado |
|---|---|---|
| Preços e condições de pagamento | Preços públicos em tabela do fornecedor; cartão de crédito de um integrante com **limite de gasto/alerta de orçamento em R$ 50**; sem prazo de pagamento negociável. Pagamento só de plano acima de R$ 150 mediante exceção (alçada no orçamento). | Fatura mensal conferida contra o orçamento (linhas de custo direto do orçamento). |
| Critérios de inspeção, qualidade e aceitação | Cada serviço só é aceito depois de um **teste de fumaça**: API responde `/health` e o OpenAPI, painel carrega login, upload de foto grava no S3, build instala no Android. | Checklist de aceite anexado ao relatório do marco M8; critérios derivados de PBI-068 a PBI-070. |
| Garantia e suporte futuro | Sem garantia contratual em plano gratuito; suporte por documentação e comunidade. Planos pagos têm suporte por e-mail. O projeto acaba em 27/11: não há suporte de longo prazo. | Registrar versão/plano e data de expiração do free tier (AWS S3: 12 meses; Neon/Render: pausa por inatividade) na tabela de riscos. |
| Incentivos e penalidades | Não há como impor multa a provedor. A ferramenta é o **SLA publicado** (créditos por indisponibilidade em planos pagos) e a possibilidade de **trocar de fornecedor** (portabilidade: Docker, PostgreSQL padrão, S3-compatível). | Incidente durante demonstração aciona plano B e fica registrado na lição aprendida. |
| Seguros e obrigações de realização | Sem seguro. Obrigação do projeto: manter segredos fora do repositório (RJ06), usar dados sintéticos de demonstração (sem dados pessoais reais) e respeitar os termos de uso. | Varredura de segredos no CI; revisão de dados do seed (A56). |
| Cláusula de rescisão | Todos os serviços são cancelados online sem multa. **Rescisão programada no encerramento**: cancelar assinaturas pagas após a nota final, exportar dados e remover chaves. | Item de checklist da atividade A62 (encerramento): ‘assinaturas canceladas, domínio sem renovação automática, chaves revogadas’. |

## ⚠️ Riscos de fornecedor e controle

| # | Risco (causa → risco → consequência) | Itens | Prob. | Impacto | Controle / plano B | Resp. |
|---|---|---|---|---|---|---|
| FR1 | Plano gratuito de hospedagem hiberna → primeira requisição leva dezenas de segundos → demonstração parece travada | P02, P03 | Alta | Médio | Subir o plano pago só na semana do M8 (já no orçamento); chamada de aquecimento antes da apresentação; Docker Compose local como plano B | GR |
| FR2 | Fila gratuita do EAS demora → build Android atrasa → app não instala para M8 | P05 | Média | Alto | Disparar build de teste já em S8 abertura; plano Starter por 1 mês; build local com Gradle como alternativa | FM |
| FR3 | Mudança de política de preço ou de limites do plano → custo ou bloqueio inesperado em meio ao projeto | P02–P08 | Média | Médio | Revisar termos no início de S8; manter o orçamento com cP; alternativas listadas na tabela | FM |
| FR4 | Banco/Armazenamento no free tier expira ou pausa → perda de dados de demonstração | P03, P04 | Média | Alto | Seed reproduzível (7.1) reconstrói tudo; exportar dump antes do M8; calendário com a data de expiração | JV |
| FR5 | Conta aberta no nome de um integrante → saída da pessoa bloqueia acesso (ponto único de falha) | P02–P08 | Baixa | Alto | Contas na organização FieldOps-Project ou com 2 administradores; 2FA; credenciais no gerenciador de segredos, nunca no repositório (RJ06) | FM |
| FR6 | Esquecer de cancelar assinatura paga após o projeto → cobrança no cartão | P02, P03, P05, P07 | Média | Baixo | Item de encerramento na atividade A62; alerta de cobrança em R$ 50 | FM |
| FR7 | Aparelho Android da equipe indisponível ou muito potente → teste RJ05 incompleto ou enganoso | P09, P10 | Média | Alto | 3 aparelhos distintos; empréstimo no laboratório; compra do aparelho de entrada só como contingência | PD |
| FR8 | Dependência de um único fornecedor → troca difícil em caso de falha | P02–P04 | Baixa | Médio | Stack portável: contêiner Docker, PostgreSQL padrão e API S3 padrão; trocar fornecedor muda só variáveis de ambiente | JV |

## 🔄 Controle e encerramento das aquisições

- **Controle:** cada item tem um responsável (coluna *Resp.* do orçamento). Toda sexta-feira de fim de sprint ele registra uso × limite do plano (GitHub Actions, armazenamento, builds EAS) no relatório de status; alerta de gasto configurado no cartão e nas plataformas.
- **Mudanças:** troca de fornecedor ou upgrade de plano segue a alçada do orçamento (até R$ 150,00 o gerente com a equipe; acima disso, orientador e coordenação).
- **Encerramento (atividade A62, 27/11):** aceite final das entregas dos fornecedores pelo teste de fumaça, cancelamento de assinaturas, exportação de dados, remoção de chaves e domínio sem renovação automática, conciliação das faturas com o orçamento e registro no relatório final.

---

## ✅ Verificações automáticas realizadas

| Verificação | Resultado |
|---|---|
| Itens de aquisição (mínimo 12 a 15) | 14 |
| Custo total = quantidade × custo unitário em todas as linhas | OK |
| Soma de cM da tabela = linhas de custo direto do orçamento | OK (R$ 1.070,00) |
| Soma do esperado PERT = linhas de custo direto do orçamento | OK (R$ 1.391,58) |
| cO ≤ cM ≤ cP em todos os itens | OK |
| Código WBS de cada item existe na EAP congelada | OK |
| Previsão de entrega ≤ início da atividade que consome o item | OK |
| Pesos dos critérios somam 100% | OK |
