# Plano de Comunicação do Projeto FieldOps

| Campo | Informação |
|---|---|
| Projeto | FieldOps — Plataforma de Inspeção em Campo (MVP) |
| Instituição / curso | Centro Universitário SENAI São Paulo (UniSENAI-SP), Campus Sorocaba. Análise e Desenvolvimento de Sistemas |
| Disciplina / atividade | Gestão de Projetos (Prof. Me. Deivison S. Takatu). Aula 06, Comunicações |
| Gerente do projeto | Felipe Marques (FM) |
| Integrantes | Felipe Marques; Gabriel Ribeiro Camargo Miranda; Pedro Henrique Dias de Paula; André dos Santos Fernandes; João Victor de Souza Santos |
| Data / versão | 18/11/2026 — versão 1.0 |
| Período coberto | 10/08/2026 a 27/11/2026: 8 sprints de 2 semanas, marcos M1 a M8 |
| Processos PMBOK | 10.1 Planejar (este documento), 10.2 Gerenciar (§2 a §6), 10.3 Controlar (§9) |
| Situação | Proposta de plano. Canais ou práticas que **não** estão documentados nos repositórios ou na especificação aparecem marcados como **(premissa)** |

---

## 1. Premissas e canais

O que está documentado: o fluxo de GitHub (branches, Conventional Commits, PR com revisão, CI), a rotina semanal de aulas (2 de conteúdo e 3 de projeto, com a Aula 5 de cada semana reservada a "integração, testes, revisão de código e demonstração parcial", doc. 16 da especificação), os marcos M1 a M8 e o Notion do Prof. Glauco como fonte da especificação. Todo o resto é premissa e precisa de confirmação do grupo.

| Canal | Uso no projeto | Tipo | Situação |
|---|---|---|---|
| GitHub Issues, Projects e milestones (org `FieldOps-Project`, 4 repositórios) | backlog, tarefas, impedimentos, milestone por sprint | Assíncrono | Documentado (S1 a S8 já são milestones) |
| GitHub Pull Requests | revisão de código, aceite técnico | Assíncrono | Documentado (CONTRIBUTING.md) |
| Repositório `docs` | contrato OpenAPI, ADRs, atas, relatórios de status, lições aprendidas | Assíncrono | Documentado para contrato e ADR; atas e relatórios em `docs/` são **premissa** |
| Reunião presencial em aula (laboratório/sala) | planning, review, retro, demos, alinhamento com o orientador | Síncrono | Rotina de aulas documentada; uso para cerimônias é **premissa** |
| WhatsApp, grupo da equipe | dailies, avisos curtos, bloqueios urgentes | Assíncrono | **Premissa** |
| E-mail institucional | relatórios, comunicação formal com coordenação e professores | Assíncrono | **Premissa** |
| Videochamada (Teams ou Meet) | reuniões remotas excepcionais, como semanas de feriado | Síncrono | **Premissa** |
| Notion do Prof. Glauco | leitura da especificação e do roadmap | Assíncrono (só leitura) | Documentado como fonte; comentários nele são **premissa** |

Idioma: português nas comunicações e atas; inglês nos commits (regra do CONTRIBUTING.md).

---

## 2. Plano de comunicação (formato do slide)

Colunas do exemplo da Aula 06: descrição, frequência, canal (síncrono ou assíncrono), público, proprietário. O momento de realização e o formato do documento estão na §3, com os mesmos códigos.

| ID | Descrição | Frequência | Canal | Público | Proprietário |
|---|---|---|---|---|---|
| COM-01 | Planning da sprint: escolher issues, atribuir responsáveis, confirmar capacidade | A cada 2 semanas (8 vezes) | Presencial em aula (síncrono); milestone no GitHub (assíncrono) | PI-05, PI-06; PI-02 convidado | FM |
| COM-02 | Daily assíncrona: ontem, hoje, bloqueios (3 linhas) | Diária, em dias úteis | WhatsApp (assíncrono) | PI-05, PI-06 | Cada integrante envia; FM consolida os bloqueios |
| COM-03 | Review da sprint: conferir o incremento contra os critérios das issues | A cada 2 semanas | Presencial em aula (síncrono) | PI-05, PI-06, PI-02 | FM |
| COM-04 | Demo do marco M1 a M8 ao comitê | A cada 2 semanas, nas datas dos marcos | Presencial em aula (síncrono) | PI-03, PI-02, PI-01 (convite), PI-04 (convite) | FM |
| COM-05 | Retrospectiva da sprint | A cada 2 semanas | Presencial em aula (síncrono) | PI-05, PI-06 | FM (relator em rodízio) |
| COM-06 | Relatório de status quinzenal (uma página) | A cada 2 semanas; semanal na sprint de risco | E-mail (assíncrono), cópia em `docs/` | PI-01, PI-02, PI-03, PI-04 | FM (PD compila os números do GitHub) |
| COM-07 | Pull request e revisão de código | Contínua | GitHub PR (assíncrono) | Autor e revisor; PI-06 | Autor do PR |
| COM-08 | Issues, backlog e GitHub Project: criar, triar, atualizar e fechar | Contínua; triagem semanal | GitHub Issues e Project (assíncrono) | PI-05, PI-06; PI-02 para regra de negócio | FM (triagem); autor (cada issue) |
| COM-09 | Reunião de integração de contrato OpenAPI: mudanças de endpoint, enum e erro | Semanal; extra a cada mudança de contrato | Presencial em aula ou videochamada (síncrono); PR no `docs` (assíncrono) | PI-06 (JV, GR, PD, AF) e FM | JV |
| COM-10 | Comunicação de mudança de escopo, de plano ou de papéis | Sob demanda | Issue com rótulo `mudanca-de-escopo` + e-mail (assíncrono); decisão na reunião COM-12 | PI-02 (decide), PI-01, PI-03, PI-06 | FM |
| COM-11 | Escalonamento de riscos e atrasos | Sob demanda, disparado por gatilhos (§7) | WhatsApp para a equipe (imediato); e-mail ao orientador | PI-06; PI-02; PI-01 se afetar marco | FM (qualquer membro pode disparar) |
| COM-12 | Alinhamento semanal com o orientador: demo parcial, dúvidas de regra de negócio, decisões | Semanal | Presencial em aula (síncrono); lista de dúvidas em issue `duvida-de-negocio` (assíncrono) | PI-02 | FM |
| COM-13 | Entrega das atividades de GP (TAP, EAP, cronograma, orçamento, riscos, etc.) | Por aula, conforme calendário da disciplina | Repositório GitHub da disciplina; aula (assíncrono e síncrono) | PI-04 | FM |
| COM-14 | Aviso de build ou deploy (APK, painel web, API de demonstração) | A cada build relevante; obrigatório antes de cada demo | WhatsApp + release/tag no GitHub (assíncrono) | PI-06; PI-02 e PI-03 antes de demos | FM (app), JV (API), AF (web) |
| COM-15 | Teste com usuários-proxy (personas Carlos, Marina, Ana): validar usabilidade | Nos marcos M3, M5 e M7 | Presencial, sessão curta (síncrono); registro em issue (assíncrono) | PI-07, PI-08, PI-09 (representados por colegas); PI-02 | GR |
| COM-16 | Solicitação de apoio institucional: laboratório, rede, calendário, eventos | Sob demanda; revisão do calendário a cada início de sprint | E-mail institucional (assíncrono) | PI-13, PI-01 | FM |
| COM-17 | Monitoramento de provedores e limites dos planos gratuitos (EAS, GitHub Actions, hospedagem, S3) | Semanal; diário na semana do M8 | Páginas de status e painéis (assíncrono); aviso no WhatsApp | PI-12 (observado); PI-06 | PD |
| COM-18 | Entrega final e demonstração ponta a ponta (M8): apresentação, release, roteiro | Uma vez, em 27/11/2026 | Presencial em aula (síncrono); release no GitHub | PI-01, PI-02, PI-03, PI-04, PI-11 (via PO), PI-10 (apenas resultados) | FM |
| COM-19 | Registro de lições aprendidas | Item na retro de cada sprint; consolidação única no encerramento | Reunião (síncrono) e `docs/lessons-learned.md` (assíncrono) | PI-05, PI-06, PI-02, PI-04 | PD (FM facilita a retro) |

**19 comunicações.** Proprietário principal: FM em 13 (COM-01 a 06, 08, 10 a 13, 16 e 18), PD em 2 (COM-17 e COM-19), JV em 1 (COM-09), GR em 1 (COM-15), o autor do PR em 1 (COM-07) e uma dividida por componente (COM-14: FM no app, JV na API, AF no web). A concentração em FM é natural para o canal formal com professores e coordenação, mas pesa sobre o gerente, que já acumula o app (ver `organograma-raci-fieldops.md`, §4). Por isso a compilação do relatório (COM-06), a lição aprendida (COM-19), o monitoramento de provedores (COM-17) e os dados do GitHub ficam com PD, e a relatoria das atas gira entre os cinco.

---

## 3. Momento de realização e formato do documento

| ID | Momento de realização | Formato do documento | Saída e onde fica |
|---|---|---|---|
| COM-01 | Segunda-feira de abertura da sprint, 1 h: 10/08, 24/08, **08/09** (07/09 é feriado), 21/09, 05/10, 19/10, **03/11** (02/11 é feriado), 16/11 | Ata (§5.2) + milestone do GitHub com as issues escolhidas | Lista de issues com responsável; ata em `docs/atas/` |
| COM-02 | Dias úteis, até as 10h (premissa), mensagem de 3 linhas: "ontem / hoje / bloqueio" | Mensagem de texto padronizada; bloqueios copiados para issue | Bloqueio sem resposta em 4 h vira COM-11 |
| COM-03 | Quinta-feira da semana 2 da sprint, 45 min | Roteiro: cada frente mostra o que fechou, com a issue aberta ao lado | Lista de issues aceitas, rejeitadas e carregadas; ata |
| COM-04 | Sexta-feira do marco, na aula do projeto: 21/08 (M1), 04/09 (M2), 18/09 (M3), 02/10 (M4), 16/10 (M5), 30/10 (M6), 13/11 (M7), 27/11 (M8) | Demo de 10 a 15 min seguindo o roteiro do incremento da sprint, no app e no painel reais (sem alteração manual de banco, conforme critérios do doc. 19) | Feedback do comitê anotado na ata; critérios de marco do doc. 19 |
| COM-05 | Sexta-feira do marco, logo após a demo, 30 min (ou na segunda, antes da planning, se a demo estourar) | Ata curta: o que manter, o que mudar, o que tentar; ações com dono e prazo | Ata em `docs/atas/`; ações viram issues |
| COM-06 | Até 24 h após a demo do marco (sexta, 18h, ou segunda de manhã) | Relatório de uma página (modelo na §8) em PDF ou Markdown | `docs/status/AAAA-MM-DD.md` + e-mail |
| COM-07 | Do PR aberto ao merge; revisão em até 24 h úteis | Template de PR do repositório (baseado no DoD) + comentários de revisão | PR aprovado e CI verde antes do merge |
| COM-08 | Contínuo. Triagem semanal toda segunda e na planning | Issue com título, critério de aceite, épico (`EP-xx`), responsável, milestone | Project atualizado; nenhuma issue da sprint sem responsável |
| COM-09 | Semana útil, 30 min, um dia fixo acordado. Mais frequente em S1 a S4, quando o contrato é definido | Pauta: mudanças pendentes no `openapi.yaml`; decisão registrada no PR ou ADR | PR no `docs` com a mudança; todos os consumidores aprovam antes do merge |
| COM-10 | Assim que a mudança for proposta, antes de implementar | Issue de mudança: o que muda, motivo, impacto em prazo, escopo, qualidade, decisão. Aplica a ordem de corte da RJ07 | Decisão do PI-02 registrada na issue; ata |
| COM-11 | Em até 24 h após o gatilho disparar (§7) | Alerta de 1 linha no WhatsApp + e-mail de 5 linhas: situação, impacto, opção A/B, decisão pedida | Ação registrada no relatório seguinte |
| COM-12 | Última aula de projeto da semana ("integração, testes, revisão de código e demonstração parcial", doc. 16.2), 20 min | Demo parcial + lista numerada de dúvidas de negócio enviada na véspera | Resposta registrada na issue `duvida-de-negocio` |
| COM-13 | Datas das aulas da disciplina de GP | Documento `.md` do repositório, formato do modelo do professor | Commit no repositório da disciplina |
| COM-14 | Até 24 h antes de cada demo e a cada build relevante | Mensagem com link do APK (EAS), URL do painel e da API, versão e lista de mudanças | Release ou tag no GitHub; usuários de demonstração documentados |
| COM-15 | Semana 2 das sprints S3 (M3), S5 (M5) e S7 (M7), antes da demo do marco | Roteiro de teste de 3 tarefas por persona + 3 perguntas | Issues com achados; entram no backlog |
| COM-16 | Primeira semana de cada sprint; extra quando houver feriado ou evento que afete aula | E-mail curto: pedido, data, impacto no marco | Resposta salva em `docs/` |
| COM-17 | Toda segunda; na semana de 23/11 a 27/11, todo dia | Checklist de 4 itens: cota de build EAS, minutos do Actions, disponibilidade da hospedagem, espaço do S3 | Alerta ao grupo se algo ficar abaixo de 20% |
| COM-18 | 27/11/2026 (M8), aula de projeto | Apresentação final + roteiro de demo ponta a ponta + release notes + relatório final | Material final no repositório; ata de aceite |
| COM-19 | Retro de cada sprint (item fixo) e semana de 23/11 a 27/11 para a consolidação | Registro: contexto, o que ocorreu, impacto, ação, dono | `docs/lessons-learned.md`; alimenta o encerramento (EAP 1.4) |

### 3.1 Calendário das cerimônias por sprint

| Sprint | Planning (COM-01) | Review (COM-03) | Demo do marco (COM-04) e retro (COM-05) | Relatório (COM-06) |
|---|---|---|---|---|
| S1 | Seg 10/08 | Qui 20/08 | Sex 21/08, M1 Fundação | até 22/08 (24 h) |
| S2 | Seg 24/08 | Qui 03/09 | Sex 04/09, M2 Autenticação | até 05/09 |
| S3 | **Ter 08/09** (07/09 feriado) | Qui 17/09 | Sex 18/09, M3 Planejamento | até 19/09 |
| S4 | Seg 21/09 | Qui 01/10 | Sex 02/10, M4 Execução online | até 03/10 |
| S5 | Seg 05/10 | Qui 15/10 | Sex 16/10, M5 Recursos nativos | até 17/10 |
| S6 | Seg 19/10 | Qui 29/10 | Sex 30/10, M6 Offline | até 31/10 |
| S7 | **Ter 03/11** (02/11 feriado) | Qui 12/11 | Sex 13/11, M7 Revisão | até 14/11 |
| S8 | Seg 16/11 | Qui 26/11 | Sex 27/11, M8 Release | relatório final (EAP 1.4) |

Os dias da semana foram conferidos por script. A sexta 20/11 (Consciência Negra) é feriado: a S8 tem 7 dias úteis entre 18/11 e 27/11. O dia da review (quinta) é premissa.

---

## 4. Matriz partes interessadas × informação

Cada parte interessada recebe o que precisa, no nível de detalhe e na frequência que cabe ao seu papel. "Detalhe" distingue o resumo (decisão e risco) do detalhe técnico (issues, PRs, contrato).

| ID | Parte interessada | O que precisa saber | Detalhe | Comunicações | Frequência | Canal principal | Responsável |
|---|---|---|---|---|---|---|---|
| PI-01 | Coordenação do curso | andamento por marco, riscos que ameaçam marco, mudança de escopo material, aceite final | resumo | COM-06, COM-04 (convite), COM-10, COM-11, COM-16, COM-18 | quinzenal e por marco | E-mail e demo presencial | FM |
| PI-02 | Prof. Glauco Todesco | regras de negócio, incremento real, desvios, mudanças de escopo (ele decide), qualidade | detalhe | COM-12, COM-04, COM-06, COM-09 (regras), COM-10, COM-11, COM-15, COM-18 | semanal | Aula presencial e issues | FM |
| PI-03 | Professores das disciplinas (comitê) | demonstração do marco, critérios do doc. 19 atendidos, participação equilibrada | resumo | COM-04, COM-06, COM-14, COM-18 | por marco e quinzenal | Demo presencial e e-mail | FM |
| PI-04 | Prof. Deivison Takatu | artefatos de GP entregues e coerentes com o projeto | detalhe dos documentos | COM-13, COM-06 (consulta), COM-19 | por aula e por entrega | Repositório GitHub da disciplina | FM |
| PI-05 | Felipe Marques (gerente) | tudo: progresso, bloqueios, riscos, carga da equipe | detalhe | COM-02, COM-03, COM-07, COM-08, COM-11 | contínua | GitHub e WhatsApp | PD compila dados; FM consome |
| PI-06 | Equipe (JV, GR, PD, AF) | prioridades da sprint, contratos, bloqueios, mudanças de plano | detalhe | COM-01, COM-02, COM-03, COM-05, COM-07, COM-08, COM-09, COM-14, COM-17, COM-19 | contínua | GitHub, WhatsApp e aula | FM; JV nos contratos |
| PI-07 | Técnicos de campo (Carlos) | se o app serve o fluxo offline: clareza, feedback de sincronização | feedback de uso | COM-15 | M3, M5, M7 | Teste presencial com usuário-proxy | GR |
| PI-08 | Supervisores (Marina) | se o painel cobre planejamento, acompanhamento e revisão | feedback de uso | COM-15 | M3, M7 | Teste presencial com usuário-proxy | GR |
| PI-09 | Administradores (Ana) | se cadastros e permissões funcionam sem apoio técnico | feedback de uso | COM-15 | M3 | Teste presencial com usuário-proxy | GR |
| PI-10 | Responsáveis do cliente (Roberto) | apenas o que o MVP entrega ao cliente (somente leitura fica para o futuro) | resumo | COM-18 | uma vez | Apresentação final | FM |
| PI-11 | Organização operadora (cliente representado) | se padronização, rastreabilidade e retrabalho estão cobertos | resumo | COM-12, COM-18 | semanal (via PO) e final | Aula; representação pelo PI-02 | FM |
| PI-12 | Provedores (Expo/EAS, GitHub, hospedagem, S3) | disponibilidade e limites; **eles não recebem comunicação do projeto**, é a equipe que observa | monitoramento | COM-17 | semanal | Painéis e páginas de status | PD |
| PI-13 | UniSENAI-SP, infraestrutura e calendário | feriados, eventos, laboratório e rede que afetam as datas | pontual | COM-16 | início de sprint e sob demanda | E-mail institucional | FM |

Verificação por script: todas as 13 partes interessadas (PI-01 a PI-13) aparecem; todos os códigos COM citados existem na §2; todo proprietário é uma das siglas FM, JV, GR, PD, AF (ou "autor do PR").

---

## 5. Regras de comunicação

### 5.1 Tempo de resposta (SLA) (premissa de equipe)

| Situação | Prazo de resposta | Se não responder |
|---|---|---|
| Bloqueio urgente (mensagem com `[BLOQUEIO]`) | 4 h corridas entre 8h e 22h | FM aciona COM-11 |
| Primeira revisão de PR | 24 h úteis | em 48 h, FM atribui outro revisor (§7) |
| Pergunta técnica em issue ou PR | 1 dia útil | cobrar na daily |
| Consulta (C na RACI) | 2 dias úteis | o R segue e registra a decisão |
| Triagem de issue nova | 2 dias úteis | entra na pauta da planning |
| Pedido ao orientador ou à coordenação (e-mail) | 5 dias úteis esperados | levar o assunto à aula semanal (COM-12) |
| Ata ou relatório de status | 24 h após a reunião ou demo | atraso vira indicador (§9) |

### 5.2 Formato padrão de ata (premissa)

```
Tipo / sprint: planning S4 | review S4 | retro S4 | contrato | orientador
Data, horário, participantes:
Pauta:
Decisões (o que foi decidido, por quem):
Ações (descrição, responsável, prazo, issue):
Riscos ou impedimentos novos:
Mudanças de escopo ou de contrato (link da issue ou PR):
Próxima reunião:
```

Arquivo: `docs/atas/AAAA-MM-DD-<tipo>.md`. Uma ata é publicada em até 24 h e quem não estava presente lê e confirma por mensagem. Se uma decisão mudar escopo ou contrato, ela vira issue ou PR (COM-10, COM-09).

### 5.3 Convenções de commit, branch e PR (documentadas em CONTRIBUTING.md)

| Item | Regra |
|---|---|
| Branch | `<tipo>/<epico>-<slug-curto>`, ex.: `feat/EP-03-crud-clients`, `fix/EP-08-outbox-retry` |
| Commit | Conventional Commits, modo imperativo, em inglês: `feat(clients): add CRUD for clients with pagination` |
| Origem e destino | branch criada a partir de `develop`; PR para `develop` |
| PR | template padrão (baseado no Definition of Done) preenchido por inteiro; itens não cumpridos são justificados e viram nova issue |
| Revisão | pelo menos **um outro membro** revisa; merge só com aprovação e CI verde |
| Roteamento | regra de negócio, endpoint e migração no `backend`; tela e guard do painel no `frontend`; tela, fluxo offline e recurso nativo no `app`; documentação, contrato compartilhado e ADR no `docs` |

### 5.4 Outras regras

- Decisão sobre escopo, contrato de API ou papel só vale se estiver registrada em issue, PR ou ata. Combinado só por WhatsApp não conta.
- Não colar segredo, senha ou chave em canal algum (RJ06). O WhatsApp serve para avisar, não para guardar informação do projeto.
- O que o PI-02 definir sobre regra de negócio é registrado na issue correspondente com a data.
- Informação de status é sempre a do GitHub no dia: o relatório cita números do milestone, não estimativas de memória.

---

## 6. Armazenamento e recuperação (10.2)

| Tipo de informação | Onde fica | Quem mantém |
|---|---|---|
| Backlog, tarefas e impedimentos | GitHub Issues e Project; milestones S1 a S8 | FM (triagem), autores |
| Código e revisão | repositórios `backend`, `frontend`, `app` | autores e revisores |
| Contrato, ADRs, atas, relatórios, lições | repositório `docs` | JV (contrato), FM (demais), PD (compilação) |
| Planejamento de GP (TAP, EAP, cronograma, orçamento, riscos) | repositório da disciplina, pastas `aula-xx` | FM |
| Evidências de demo (roteiro, usuários, release notes) | `docs/demo/` e releases do GitHub | FM |

---

## 7. Gatilhos de escalonamento

Os gatilhos acionam a COM-11. O escalonamento sobe de nível só se o anterior não resolver no prazo.

| # | Gatilho | Como medir | Quem detecta | Ação e prazo | Destinatário | Situação em 18/11/2026 |
|---|---|---|---|---|---|---|
| G1 | Sprint com **menos de 70%** das issues concluídas | issues fechadas ÷ total do milestone na review | FM | Plano de recuperação na retro; aplicar a ordem de corte; avisar o orientador em 24 h | PI-02 (PI-01 se afetar marco) | Abaixo do limite em S5 (4 de 8, 50%), S6 (2 de 13, 15%) e S7 (2 de 14, 14%). S3 (70%) e S4 (71%) ficaram no limite |
| G2 | Issue da sprint **sem responsável** | issues abertas sem assignee na planning | FM | A issue não entra na sprint até ganhar dono; FM e JV redistribuem em 24 h | PI-06 | Atingido: 21 issues abertas do backend sem responsável |
| G3 | PR sem revisão por mais de **48 h** | `gh pr list` | FM ou PD | Reatribuir o revisor; em 72 h, revisar pelo gerente | autor e revisor | Sem medição registrada; passa a ser medido (§9) |
| G4 | Bloqueio sem resposta em **4 h** (mensagem `[BLOQUEIO]`) | daily e WhatsApp | quem sinalizou | FM chama a pessoa dona do assunto; se for contrato, JV decide | PI-06 | Não medido |
| G5 | Mudança de contrato da API sem PR no `docs` ou sem aprovação dos consumidores | revisão do PR | JV | Bloquear o merge e levar à COM-09 | PI-06 | Não medido |
| G6 | **Marco em risco:** a 3 dias úteis do marco, com menos de 50% das issues do milestone fechadas | contagem na terça (3 dias úteis antes) do marco de sexta | FM | Aplicar a ordem de corte (RJ07), propor reescopo do marco e comunicar PI-02 e PI-03 em 24 h | PI-02, PI-03, PI-01 | **M8** (27/11): 0 de 10 fechadas, 7 dias úteis até o marco. O gatilho dispara em 24/11 (3 dias úteis antes) se continuar abaixo de 50%; já tratado como risco no relatório da §8 |
| G7 | Indisponibilidade de pessoa-chave (FM ou AF) por mais de 3 dias úteis | aviso na daily | FM ou JV | Acionar o suplente (organograma-raci-fieldops.md, §5.2) e rever a sprint | PI-06, PI-02 | Nenhuma registrada |
| G8 | Risco do registro com severidade alta que se materializa | registro de riscos (`aula-07-riscos`) | FM | Executar a resposta do registro e comunicar | PI-02, PI-01 | Atraso de S6 e S7 materializa o risco de prazo |
| G9 | Limite de plano gratuito (EAS, Actions, hospedagem, S3) abaixo de 20% | checklist da COM-17 | PD | Avisar o grupo; priorizar builds; avaliar plano pago ou build local | PI-06, FM | Não medido |
| G10 | Evento ou feriado que remove aula de marco | calendário (COM-16) | FM | Replanejar a cerimônia, comunicar PI-02 e PI-03 | PI-02, PI-03 | 20/11 (sexta) é feriado; M8 permanece em 27/11 |

Níveis: (1) conversa na daily ou na issue, (2) FM decide com a equipe, (3) orientador PI-02, (4) coordenação PI-01 (só quando afeta marco ou avaliação).

---

## 8. Modelo de relatório de status de uma página (preenchido em 18/11/2026)

Este é o relatório do snapshot de 18/11/2026 (quarta, semana 15 de 16, sprint S8). Fonte: GitHub, milestones por sprint, 4 repositórios somados. Marcado como situação monitorada, com ações propostas ainda por confirmar.

**FieldOps — Relatório de status | 18/11/2026 | Sprint S8 (16/11 a 27/11), próximo marco M8 Release em 27/11 | Gerente: Felipe Marques**

**1. Resumo.** O projeto está **com desvio de prazo real**. Em 18/11, 57 das 103 issues dos milestones estão fechadas (55%). As sprints S6 (offline) e S7 (revisão) estão muito atrasadas, e a S8 tem 10 issues, nenhuma fechada. Restam **46 issues em 6 dias úteis** (19/11 a 27/11, descontado o feriado de 20/11), o equivalente a 7,7 issues por dia, ou cerca de 7 vezes o ritmo médio de S1 a S5 (10,6 issues por sprint, 1,1 por dia útil). Esse ritmo não é viável sem corte de escopo.

**2. Indicadores**

| Dimensão | Situação | Comentário |
|---|---|---|
| Cronograma | 🔴 | 46 issues abertas para 7 dias úteis; M6 e M7 não atingidos |
| Escopo | 🟡 | núcleo intocável (autenticação, planejamento, checklist, foto, offline, sincronização, revisão) precisa ser protegido; ordem de corte a aplicar |
| Qualidade | 🟡 | 66 de 69 PRs encerrados (61 mergeados e 5 fechados sem merge; 3 abertos, coleta de 18/11); não há medição de cobertura de testes |
| Equipe | 🟡 | gerente acumula gestão e app; 21 issues do backend sem responsável |

O critério do semáforo por sprint é o das issues fechadas: 🟢 90% ou mais, 🟡 70% a 89%, 🔴 abaixo de 70%. É um indicador de issues, não de funcionalidades entregues.

**3. Progresso por sprint (issues fechadas de total)**

| Sprint | Marco | Fechadas / total | % | Sinal |
|---|---|---|--:|---|
| S1 | M1 Fundação (21/08) | 15 / 17 | 88% | 🟡 |
| S2 | M2 Autenticação (04/09) | 17 / 17 | 100% | 🟢 |
| S3 | M3 Planejamento (18/09) | 7 / 10 | 70% | 🟡 |
| S4 | M4 Execução online (02/10) | 10 / 14 | 71% | 🟡 |
| S5 | M5 Recursos nativos (16/10) | 4 / 8 | 50% | 🔴 |
| S6 | M6 Offline (30/10) | 2 / 13 | 15% | 🔴 |
| S7 | M7 Revisão (13/11) | 2 / 14 | 14% | 🔴 |
| S8 | M8 Release (27/11) | 0 / 10 | 0% | 🔴 (em curso, dia 3 de 10) |
| **Total** | | **57 / 103** | **55%** | |

**4. Por repositório (todas as issues, inclusive as sem milestone)**

| Repositório | Issues fechadas / total | % | PRs encerrados / total |
|---|---|--:|---|
| backend | 14 / 41 | 34% | 23 / 24 |
| frontend | 22 / 31 | 71% | 19 / 20 |
| app | 21 / 39 | 54% | 22 / 22 |
| docs | 1 / 6 | 17% | 1 / 1 |
| **Total** | **58 / 117** | **50%** | **65 / 67** |

A diferença para o painel por sprint (58 contra 57 fechadas, 117 contra 103 no total) vem de issues sem milestone: 14 issues sem sprint associada (117 menos 103). Esse número é derivado, não lido do GitHub; conferir antes de enviar.

**5. Riscos e decisões pedidas**

| Item | Detalhe | Decisão pedida a |
|---|---|---|
| Prazo do M8 | 46 issues em 7 dias úteis | PI-02 e PI-03: aprovar a redução do escopo do M8 ao núcleo intocável |
| Backend | 14 de 41 fechadas e 21 issues sem responsável | equipe: atribuir donos até a próxima daily |
| Offline (M6) e revisão (M7) | 2 de 13 e 2 de 14 | PI-02: confirmar quais PBIs ficam no MVP |
| Calendário | feriado em 20/11 | informativo |

**6. Ações corretivas propostas** (aguardam confirmação do grupo e do PI-02)

1. Congelar escopo novo e aplicar a ordem de corte da Declaração de Escopo (RJ07): (1) remover o dashboard avançado, (2) limitar os tipos de resposta, (3) aceitar só foto da câmera, (4) construtor sem arrastar e soltar, (5) conflitos de sincronização: detectar e bloquear, (6) adiar notificações, PDF e assinatura.
2. Atribuir as 21 issues sem responsável do backend em até 24 h (G2), priorizando sincronização e revisão.
3. Passar o relatório a semanal até 27/11 e a daily a obrigatória.
4. Gravar um roteiro de demo mínimo do fluxo completo e testá-lo na quinta 26/11 (review da S8, COM-03).
5. Registrar as lições aprendidas do desvio na retro final (COM-19), sem esperar o encerramento.

**7. Próximos marcos e datas.** Review da S8 em 26/11; demo final M8 e entrega em 27/11 (COM-18).

---

## 9. Controle das comunicações (10.3)

O plano é monitorado a cada sprint, na retrospectiva, com os indicadores abaixo. A fonte é o GitHub (via `gh` e Project) e uma pesquisa rápida de 3 perguntas (escala 1 a 5) na retro. PD compila os números e FM revisa.

| # | Indicador de eficácia | Fórmula | Meta | Fonte | Se ficar fora da meta |
|---|---|---|---|---|---|
| I1 | Relatórios de status no prazo | relatórios enviados em até 24 h da demo ÷ relatórios devidos | 100% | `docs/status/` e e-mail | FM delega a compilação a PD |
| I2 | Primeira revisão de PR em até 24 h úteis | PRs com primeira revisão no prazo ÷ PRs abertos | 80% ou mais | `gh pr list` | rodízio de revisores; avisar na daily |
| I3 | PRs com revisão de outro membro antes do merge | PRs mergeados com aprovação ÷ PRs mergeados | 100% (regra do CONTRIBUTING.md) | `gh` | bloquear merge direto |
| I4 | Issues da sprint com responsável na planning | issues com assignee ÷ issues do milestone | 100% | GitHub Project | G2: issue não entra |
| I5 | Conclusão da sprint | issues fechadas ÷ total do milestone | 70% ou mais | milestone | G1: escalonar |
| I6 | Mudanças de escopo ou contrato registradas por escrito antes do trabalho | mudanças com issue ou PR ÷ mudanças identificadas | 100% | revisão na retro | COM-10 reforçada |
| I7 | Contratos de API mudados com aprovação dos consumidores | PRs de contrato com aprovação de consumidores ÷ PRs de contrato | 100% | PRs no `docs` | G5 |
| I8 | Participação na daily | dias úteis com 3 linhas de cada pessoa ÷ dias úteis | 80% ou mais | WhatsApp | conversa individual do gerente |
| I9 | Percepção de clareza (pesquisa) | média da pergunta "recebi a informação que precisava a tempo?" | 4,0 ou mais | pesquisa na retro | tratar a causa na retro |
| I10 | Surpresas | casos em que alguém soube tarde de decisão ou requisito que o afetava | 0 | retro | revisar a matriz da §4 e a COM envolvida |
| I11 | Dúvidas de negócio resolvidas na semana | dúvidas respondidas até a aula seguinte ÷ dúvidas abertas | 90% ou mais | issues `duvida-de-negocio` | levar ao PI-02 por e-mail |

**Revisão do plano.** Na retro de cada sprint, a equipe olha os onze indicadores, decide o que ajustar (canal, frequência, público) e registra em nova versão deste documento (1.1, 1.2...). Mudança que afeta uma parte interessada externa (PI-01 a PI-04) é comunicada por COM-10.

**Situação em 18/11/2026.** Medição formal dos indicadores ainda não existe: os números de issues (I4, I5) e de PRs (I3) vêm do GitHub e já dão sinal (21 issues do backend sem dono, três sprints abaixo de 70%). Os indicadores I8 a I11 dependem de coleta que começa na retro da S8. Isso é limitação conhecida, não resultado.

---

## 10. Pontos a confirmar com o grupo

1. Canais: WhatsApp para dailies, e-mail institucional e videochamada (premissas).
2. Horários e dias: review às quintas, relatório até 24 h após a demo, daily até as 10h.
3. Se o Prof. Glauco está presente na última aula de projeto da semana (COM-12) e se aceita ser consultado por e-mail.
4. Pastas `docs/atas/`, `docs/status/`, `docs/lessons-learned.md` e `docs/demo/` (premissa de organização do repositório `docs`).
5. Se a coordenação (PI-01) quer receber o relatório completo ou só o resumo de uma linha por marco.
6. O número de issues sem milestone (14) e a diferença entre 57 e 58 fechadas: conferir no GitHub antes de enviar o relatório.

## 11. Checklist

- [x] 19 comunicações (mínimo pedido: 14) com descrição, frequência, canal síncrono ou assíncrono, público e proprietário
- [x] Momento de realização e formato do documento de cada uma
- [x] Calendário das cerimônias por sprint com feriados ajustados (08/09 e 03/11)
- [x] Matriz partes interessadas × informação cobrindo PI-01 a PI-13
- [x] Regras: SLA, formato de ata e de relatório, convenções de commit e PR do CONTRIBUTING.md
- [x] Gatilhos de escalonamento com a situação real de 18/11/2026
- [x] Relatório de status de uma página preenchido com os dados de 18/11/2026
- [x] Controle das comunicações (10.3) com indicadores de eficácia
