# Termo de Abertura de Projeto — FieldOps

| Campo | Informação |
|---|---|
| **Projeto** | FieldOps — Plataforma de Inspeção em Campo (MVP) |
| **Gerente do projeto** | Felipe Marques |
| **Integrantes** | Felipe Marques; Gabriel Ribeiro Camargo Miranda; Pedro Henrique Dias de Paula; André dos Santos Fernandes; João Victor de Souza Santos |
| **Data** | 18/11/2026 |
| **Versão** | 2.0 (consolidada) |
| **Instituição / disciplina** | UniSENAI-SP, Campus Sorocaba (Santa Rosália) — Análise e Desenvolvimento de Sistemas — Gestão de Projetos (Prof. Me. Deivison S. Takatu) |
| **Modelo** | Modelo 4.1 do professor; as linhas do modelo foram mantidas na ordem. Foram acrescentadas as linhas **Principais entregas**, **Requisitos iniciais**, **Decisões e divergências registradas** e **Responsáveis**, que o enunciado da Aula 02 exige e o modelo não traz |
| **Histórico** | TAP de 30/09/2026 (versão 1.0, Downloads) e `TAP_FieldOps.pdf` do repositório docs (versão anterior, sem data) → esta versão 2.0 reconcilia os dois com a Declaração de Escopo (04/10/2026), a documentação do produto e o estado dos repositórios em 18/11/2026 |

> Este termo formaliza o projeto FieldOps e reconcilia as fontes existentes. O que não está em documento do projeto aparece como **premissa** e precisa de confirmação do grupo. Valores monetários são **hipóteses de valoração**: o projeto não tem investimento financeiro direto.

---

## Título do projeto

FieldOps — Plataforma de Inspeção em Campo (MVP).


## Objetivo geral e objetivos específicos

**Objetivo geral:** desenvolver e entregar, até 27/11/2026, o MVP do FieldOps, uma plataforma integrada (aplicativo mobile Android com operação offline, interface administrativa web e API REST central) que permita configurar, planejar, executar, sincronizar, revisar e auditar inspeções técnicas em campo de forma padronizada, segura e rastreável, substituindo formulários em papel, planilhas e registros informais por um fluxo digital único.

**Objetivos específicos:**

1. padronizar as inspeções com modelos de checklist versionados e reutilizáveis (RP05);
2. permitir a execução offline, com sincronização idempotente e sem perda ou duplicidade de dados (RP02, RP03);
3. dar ao supervisor visibilidade do andamento, das evidências e das não conformidades (RP04, RP08);
4. centralizar cadastros e autorização por perfil em uma API validada no backend (RP06, RP07);
5. estabelecer um fluxo formal de revisão, aprovação e reprovação, com histórico e auditoria (RP09, RP10);
6. demonstrar o fluxo completo (configuração, modelo, agendamento, execução offline, sincronização, revisão e aprovação) no marco M8 e cumprir os marcos quinzenais M1 a M8 (RJ02).


## Justificativa do projeto (benefícios)

Hoje as inspeções técnicas em campo dependem de formulários impressos, planilhas isoladas, fotos sem identificação e mensagens em vários canais. A cadeia entre planejamento, execução e análise fica fragmentada e causa retrabalho, perda e preenchimento incompleto de informações, baixa rastreabilidade, demora na entrega do resultado e dificuldade de auditoria.

Digitalizar o processo só com planilhas compartilhadas ou formulários genéricos não resolve o essencial: essas alternativas não funcionam sem internet, não ligam a evidência ao equipamento, não têm fluxo formal de revisão e não guardam histórico auditável. Por isso a opção é uma plataforma própria.

**Benefícios esperados:** padronização por modelos versionados; menos retrabalho e menos campos em branco; associação direta entre resposta, evidência, data, usuário, equipamento e localização; execução em locais sem conectividade; menor intervalo entre a execução em campo e o resultado; mais visibilidade para a supervisão; menos dependência de acesso ao banco ou ao Swagger nas tarefas administrativas.

**Benefício acadêmico:** integra as disciplinas de mobile (Expo/React Native), backend (Java/Spring Boot) e web em um produto único e realista, com Git, PR e revisão de código.


## Premissas

1. **P01** — Android é a plataforma mobile prioritária, com validação obrigatória em Android (o Expo permite outros ambientes);
2. **P02** — o sistema será usado por uma organização operadora que presta inspeções para vários clientes;
3. **P03** — cada inspeção tem um técnico responsável e o supervisor revisa o resultado;
4. **P04** — as inspeções partem de modelos previamente configurados e o equipamento pode ser identificado por um QR Code único;
5. **P05** — o app preserva dados não sincronizados mesmo após ser fechado; a API é o registro oficial depois da sincronização;
6. **P06** — 16 semanas efetivas de aula (10/08 a 27/11/2026) são o limite do MVP; as 4 semanas restantes do calendário institucional não são reserva de desenvolvimento;
7. **P07** — a equipe tem dispositivos Android próprios e ferramentas gratuitas ou já disponíveis;
8. **P08 (premissa deste TAP)** — os papéis de cada integrante foram inferidos dos repositórios e precisam de confirmação do grupo;
9. **P09 (premissa deste TAP)** — a dedicação é de 12 h por semana por pessoa (5 pessoas × 16 semanas × 12 h = 960 h).


## Breve descrição do projeto

O FieldOps é uma plataforma para planejar, executar, acompanhar e revisar inspeções técnicas em campo, formada por quatro componentes que compartilham um contrato de API: (1) **aplicativo mobile** para o técnico executar checklists, capturar fotos, ler QR Code e registrar localização, inclusive offline; (2) **interface administrativa web** para administradores e supervisores cadastrarem, criarem modelos, agendarem, atribuírem, acompanharem e revisarem; (3) **API REST central**, com autenticação, autorização, regras de negócio, sincronização, upload de evidências e auditoria; (4) **infraestrutura de dados** (PostgreSQL, armazenamento compatível com S3 e SQLite local no app).

**Stack real:** mobile em Expo/React Native + TypeScript com Expo Router e SQLite; web em Next.js 16 (App Router) + React 19 + TypeScript; API em Java + Spring Boot, PostgreSQL, Flyway, JWT e OpenAPI/Swagger, com Docker Compose; GitHub (organização FieldOps-Project, repositórios docs, backend, frontend e app), Conventional Commits e CI por repositório.

**Fora do escopo:** portal do cliente (perfil de responsável do cliente), multiempresa, pagamento, inteligência artificial, integração com IoT, assinatura com validade jurídica, mais de um técnico por inspeção e validação obrigatória fora do Android.


## Principais entregas

Sete entregas de nível 1 da EAP (ver `aula-03-escopo/eap-fieldops.md`):

1. **Gerenciamento do Projeto** — TAP e Declaração de Escopo, plano de gerenciamento, relatórios e demonstrações dos marcos, encerramento;
2. **Fundação Técnica** — repositórios e convenções, contrato OpenAPI, integração contínua, ambiente padronizado (M1);
3. **API REST Central** — identidade e acesso, cadastros, modelos versionados, planejamento, execução e evidências, sincronização idempotente, revisão e auditoria;
4. **Interface Administrativa Web** — shell e autenticação, usuários e cadastros, construtor de modelos, agendamento e acompanhamento, evidências, revisão;
5. **Aplicativo Mobile (Android)** — base e login, lista e detalhes, checklist dinâmico, câmera, QR Code e localização, persistência offline com outbox, sincronização;
6. **Qualidade e Testes** — plano de testes, testes automatizados, integração em dispositivos Android, acessibilidade e correção de falhas;
7. **Entrega e Release (M8)** — dados de demonstração, build Android, painel e API executáveis, documentação final e demonstração ponta a ponta.


## Requisitos iniciais

**Produto (Declaração de Escopo, 04/10/2026):** RP01 app mobile de execução; RP02 operação offline com SQLite; RP03 sincronização idempotente por outbox; RP04 interface administrativa web (não substituível por Swagger ou banco); RP05 modelos de inspeção versionados; RP06 API REST central com OpenAPI; RP07 autenticação e autorização por perfil validadas na API; RP08 evidências rastreáveis em armazenamento S3; RP09 fluxo de revisão (aprova, reprova, corrige; aprovada fica protegida); RP10 auditoria.

**Projeto:** RJ01 contrato OpenAPI desde M1; RJ02 8 sprints e marcos M1 a M8; RJ03 Git, PR e revisão de código; RJ04 stack definida; RJ05 validação em Android; RJ06 segurança (sem segredos no repositório, autorização no backend); RJ07 controle de escopo com ordem de corte; RJ08 avaliação por marcos.

**Backlog:** 72 itens P0 (PBI-001 a PBI-072) em 10 épicos (EP-01 a EP-10) e 10 itens P1 (PBI-073 a PBI-082) fora da linha de base.


## Patrocinador (sponsor)

Coordenação do curso de Análise e Desenvolvimento de Sistemas do UniSENAI-SP, Campus Sorocaba (patrocinador institucional), com o corpo docente das disciplinas envolvidas. O Prof. Glauco Todesco, autor da especificação, é o orientador e product owner acadêmico.


## Estimativa de investimentos

**Sem investimento financeiro direto** (restrição R07). O recurso principal é o esforço da equipe: hipótese de 960 h (5 pessoas × 16 semanas × 12 h por semana).

**Hipóteses de valoração** (custo de oportunidade de aluno de ADS, não desembolsado): R$ 45/h para desenvolvedor e R$ 60/h para as horas de gestão do gerente; a revisão docente não é valorada (contrapartida institucional). O orçamento detalhado, com estimativa de três pontos, está em `aula-05-custos-qualidade/orcamento-fieldops.md`.

**Caixa (hipótese):** de R$ 0 a cerca de R$ 1.500 no semestre, caso seja necessário domínio, hospedagem, banco gerenciado, armazenamento S3 ou um plano pago do EAS Build em semana de pico. Hoje a infraestrutura usa recursos gratuitos ou já disponíveis.


## Estimativa de prazo

16 semanas efetivas, de **10/08/2026 (criação dos repositórios) a 27/11/2026**, em 8 sprints de 2 semanas, dentro de um calendário institucional de 20 semanas. Marcos:

| Marco | Data | Incremento demonstrável |
|---|---|---|
| M1 Fundação | 21/08/2026 | 3 aplicações executáveis e contrato OpenAPI inicial |
| M2 Autenticação | 04/09/2026 | login integrado, usuários e cadastros |
| M3 Planejamento | 18/09/2026 | supervisor cria modelo, agenda e atribui |
| M4 Execução online | 02/10/2026 | técnico executa inspeção online |
| M5 Recursos nativos | 16/10/2026 | câmera, QR Code, GPS e não conformidade |
| M6 Offline | 30/10/2026 | inspeção offline sincronizada |
| M7 Revisão | 13/11/2026 | aprovação, reprovação, correção e testes |
| M8 Release | 27/11/2026 | build Android, deploy e demonstração ponta a ponta |

**Feriados no período:** 07/09, 12/10, 02/11 e 20/11 (e 15/11, domingo). Entram no cronograma como risco de calendário, sem mudar as datas dos marcos.

**Situação em 18/11/2026 (semana 15, sprint 8):** 57 de 103 issues dos milestones por sprint concluídas (cerca de 55%). S1 15/17, S2 17/17, S3 7/10, S4 10/14, S5 4/8, S6 2/13, S7 2/14 e S8 0/10. As sprints 6 e 7 estão muito atrasadas, o que é um desvio real de prazo e aciona a ordem de corte (RJ07), tratada no cronograma.


## Fatores Críticos de Sucesso (FCS)

1. equipes integradas nas três frentes (mobile, web e API), com contrato de dados (identificadores, enums, regras de negócio) alinhado desde o início;
2. depois de aprovado o escopo do MVP, toda mudança relevante passa por avaliação formal, para não comprometer as 16 semanas;
3. cumprimento dos marcos M1 a M8, com demonstração incremental ao fim de cada sprint;
4. uso disciplinado de Git, PR e revisão de código, conforme o guia de contribuição;
5. priorização do núcleo do MVP: autenticação, planejamento, checklist, foto, operação offline, sincronização e revisão;
6. adesão à ordem de corte quando houver atraso: (1) remover o dashboard avançado; (2) limitar os tipos de resposta; (3) só foto da câmera; (4) construtor sem arrastar e soltar; (5) conflito de sincronização só detectado e bloqueado; (6) adiar notificações, PDF e assinatura.


## Principais riscos

1. atraso no cronograma de 16 semanas, comprometendo funcionalidades do MVP (**já materializado**: sprints 6 e 7 com 2/13 e 2/14 issues concluídas em 18/11);
2. falhas na sincronização offline, com perda de dados ou duplicidade em reenvios;
3. integração deficiente entre mobile, web e API por contrato de API mal definido;
4. complexidade dos recursos nativos (câmera, QR Code, localização, permissões) em aparelhos Android diversos;
5. distribuição desigual do trabalho e dependência de pessoa-chave: o gerente também desenvolve o mobile (sobrecarga e ponto único de falha) e o painel web tem um só responsável principal;
6. falhas de segurança, como segredos no repositório ou ausência de autorização no backend;
7. feriados e eventos institucionais reduzindo o tempo útil das sprints.


## Restrições

1. **R01** — Android como plataforma prioritária, com validação obrigatória;
2. **R02** — prazo fixo de 16 semanas efetivas para o MVP;
3. **R03** — stack definida: Expo/React Native + TypeScript (mobile), Java/Spring Boot + PostgreSQL (API) e **Next.js/React + TypeScript (web)**, conforme a decisão registrada abaixo (o TAP de 30/09 e a Declaração de 04/10 diziam Angular);
4. **R04** — a interface administrativa faz parte do MVP e não pode ser substituída por Swagger ou acesso direto ao banco;
5. **R05** — um único técnico responsável por inspeção no MVP;
6. **R06** — os itens excluídos do escopo não serão desenvolvidos;
7. **R07** — sem investimento financeiro direto; só recursos gratuitos ou já disponíveis.


## Interessados no projeto (stakeholders)

PI-01 coordenação do curso (patrocinador); PI-02 Prof. Glauco Todesco (orientador e product owner acadêmico); PI-03 professores de Expo, Spring Boot e Web (avaliadores, comitê); PI-04 Prof. Deivison S. Takatu (avaliador de Gestão de Projetos); PI-05 Felipe Marques (gerente); PI-06 equipe de desenvolvimento (JV, GR, PD, AF); PI-07 técnicos de campo; PI-08 supervisores; PI-09 administradores; PI-10 responsáveis do cliente (somente leitura, fora do MVP); PI-11 organização operadora; PI-12 provedores de serviço (Expo/EAS, GitHub, hospedagem, armazenamento S3); PI-13 infraestrutura e calendário da UniSENAI-SP.


## Comitê Executivo

Coordenação do curso de ADS (UniSENAI-SP, Sorocaba), Prof. Glauco Todesco e os professores das disciplinas de Expo/React Native, Java/Spring Boot e desenvolvimento web (nomes dos demais docentes a confirmar). Responsável pela validação por marco e pela avaliação das entregas.


## Gerente do Projeto

Felipe Marques.


## Responsáveis (equipe e papéis)

Papéis inferidos dos repositórios (**premissa P08, a confirmar com o grupo**):

| Integrante | Papel | Evidência |
|---|---|---|
| Felipe Marques (FM) | Gerente do projeto e desenvolvedor mobile (Expo) | TAP e Declaração de Escopo; issues do app; commits em docs |
| João Victor Souza (JV) | Desenvolvedor backend, líder técnico da API, convenções e docs | 25 commits e issues #1 e #2 do backend |
| Gabriel Ribeiro (GR) | Desenvolvedor backend (cadastros, modelos, qualidade da API) | issues do backend |
| Pedro Dias (PD) | Desenvolvedor backend e documentação | issues do backend; commits de TAP em docs |
| André Fernandes (AF) | Desenvolvedor web (Next.js) | 26 issues do frontend |

A atribuição de responsável (R) e aprovador (A) por pacote da EAP é proposta do gerente e está no `organograma-raci-fieldops.md`.


## Decisões e divergências registradas

As fontes existentes divergem entre si e com o código. Decisões desta versão:

| Tema | O que dizem as fontes | Realidade verificada | Decisão |
|---|---|---|---|
| Stack do painel web | TAP e Declaração de Escopo: Angular + TypeScript (R03, RJ04). Notion (docs 02 e 11): aceita Angular ou React | **Next.js 16.3.3 (App Router) com React 19 e TypeScript**, no repositório frontend. A issue #1 do frontend pede Next.js e o plano do frontend registra a divergência com o Notion e manda criar um ADR | Mantém Next.js. Trocar de framework na semana 15 custaria retrabalho sem ganho; o Notion permite React; o conteúdo de aprendizagem (rotas protegidas, formulários, tabelas, consumo de API) é o mesmo. R03 e RJ04 passam a ler Next.js/React + TypeScript. Nenhum pacote da EAP muda. Falta o ADR em docs (issue #3 aberta) |
| Gerente do projeto | `docs/TAP_FieldOps.pdf`: Pedro Dias. TAP de 30/09 e Declaração de 04/10: Felipe Marques | Felipe Marques | Vale o documento mais recente. Pedro Dias segue como desenvolvedor backend e responsável pela documentação |
| Patrocinador e comitê | PDF: Glauco Todesco como patrocinador e comitê executivo. TAP de 30/09: coordenação do curso e corpo docente | Coordenação do curso como patrocinador institucional; Prof. Glauco Todesco como orientador e product owner acadêmico; comitê com a coordenação e os professores das disciplinas | Une as duas leituras: quem aprova escopo e marcos é a coordenação; quem define regras de negócio e critérios é o Prof. Glauco |
| Investimento | PDF: não aplicável. TAP de 30/09: sem investimento financeiro direto | Sem investimento financeiro direto, com esforço da equipe valorado como hipótese | Mantém "sem investimento direto" (R07) e passa a registrar o esforço em horas e uma estimativa de caixa pequena, ambos hipóteses |
| Itens fora do escopo | PDF/Notion (doc 01, item 1.7): 14 itens. TAP e Declaração: 8 itens | 8 exclusões formais da Declaração + itens do Notion mantidos como não objetivos (roteirização, localização contínua, vídeo, relatórios regulatórios, ERP) | Evita ampliar escopo por omissão; a lista formal continua sendo a da Declaração de Escopo |
| Prazo | Ambos: 16 semanas efetivas, 8 sprints, marcos M1 a M8, sem datas | 10/08/2026 a 27/11/2026, com a data de cada marco | Datas derivadas da criação dos repositórios (10/08) e do calendário de 16 semanas |

As decisões de stack, patrocinador e itens fora do escopo são do gerente e devem ser **ratificadas pelo orientador e pela coordenação** (aprovação abaixo).

---

## Fontes consultadas

- TAP de 30/09/2026 e Declaração de Escopo de 04/10/2026 (Downloads).

- `docs/TAP_FieldOps.pdf` e `docs/FieldOps_Sprint1_Planejamento.pdf` (repositório docs).

- Especificação do produto no Notion (`fieldops-src/docs/notion/`, documentos 01 a 20).

- Repositórios da organização FieldOps-Project no GitHub (issues por milestone, PRs, `package.json` e `docs/PLANO-FRONTEND.md` do frontend), consultados em 18/11/2026.

- Material do professor: slide da Aula 02 e Modelo 4.1 de Termo de Abertura.

---

## Mapa do enunciado da Aula 02

| Item exigido | Onde está neste TAP |
|---|---|
| Objetivo | Objetivo geral e objetivos específicos |
| Justificativa | Justificativa do projeto (benefícios) |
| Principais entregas | Principais entregas |
| Requisitos iniciais | Requisitos iniciais |
| Premissas | Premissas |
| Restrições | Restrições |
| Stakeholders | Interessados no projeto (stakeholders) |
| Responsáveis | Gerente do Projeto e Responsáveis (equipe e papéis) |
| Prazo estimado | Estimativa de prazo |

---

## Assinaturas

| | Nome | Data | Assinatura |
|---|---|---|---|
| **Elaborado por** | Felipe Marques (gerente do projeto) | 18/11/2026 | |
| **Aprovado por** | Coordenação do curso de ADS (nome do aprovador a confirmar) | ____/____/____ | |
