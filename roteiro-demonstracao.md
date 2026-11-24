# Roteiro da demonstração ponta a ponta (AC-RELEASE)

Roteiro ensaiável dos 19 passos do critério AC-RELEASE (documento [17 - Critérios de Aceitação](notion/17-criterios-de-aceitacao.md#1719-ac-release--aceitação-da-versão-final), seção 17.19), com os dados usados, o tempo previsto e a contingência de cada passo. Atende o card [docs#5](https://github.com/FieldOps-Project/docs/issues/5) (PBI-072). O ambiente é o do [guia de execução local](execucao-local.md).

- **Última atualização:** 2026-11-24
- **Base verificada:** a mesma do [guia de execução local](execucao-local.md): `backend` em `feat/EP-10-demo-container`, `frontend` em `develop` e `app` em `feat/EP-08-backend-contract` (app#70).
- **Situação:** o roteiro foi montado a partir do código e do seed, **sem ensaio ainda**. Os passos marcados com ⚠️ dependem de algo que o código atual não entrega e usam a contingência como caminho principal. O resultado de cada ensaio vai para o [registro de ensaios](#registro-de-ensaios).

Regra que vale para o roteiro inteiro: **nenhum passo edita o banco**. Onde o painel ainda não tem a tela, a contingência usa a própria API pelo Swagger, que aplica as mesmas regras de negócio e grava o histórico.

## Papéis e equipamentos

| Papel | Quem faz | Onde |
| --- | --- | --- |
| Apresentador do painel | pessoa 1 | navegador na máquina que roda a API, projetado |
| Técnico em campo | pessoa 2 | celular com Expo Go (ou emulador Android espelhado na tela) |
| Apoio | pessoa 3 | Swagger aberto numa aba, este roteiro e o cronômetro |

Com duas pessoas, o apresentador do painel acumula o apoio.

## Dados da demonstração

Todos vêm do seed do profile `demo`; nada é digitado no banco.

| Dado | Valor |
| --- | --- |
| Administrador | `admin@fieldops.local` / `Demo@1234` |
| Supervisor (revisa) | `supervisor@fieldops.local` / `Demo@1234` |
| Técnico | `tecnico@fieldops.local` / `Demo@1234` |
| Cliente | Industria ABC |
| Local | Fabrica Sorocaba (Avenida Industrial, 1000, Sorocaba/SP) |
| Equipamento | Gerador GD-001, patrimônio `GD-001`, QR `FIELDOPS-GD-001` |
| QR impresso | [`backend/docs/demo/qr-gerador-gd-001.svg`](https://github.com/FieldOps-Project/backend/blob/feat/EP-10-demo-container/docs/demo/qr-gerador-gd-001.svg) |
| Modelo | Checklist de Inspecao de Geradores, versão 1 publicada |
| Inspeção da demonstração | Inspecao preventiva - Gerador GD-001, `ASSIGNED`, prioridade alta, id `d0000000-0000-4000-8000-000000000501` |

### Respostas planejadas

Dividir o checklist em duas partes é o que permite mostrar o preenchimento offline: a seção 1 é respondida com rede, as seções 2 a 4 sem rede.

| Item | Pergunta | Resposta | Quando |
| --- | --- | --- | --- |
| GER-GER-001 | Placa de identificacao | Conforme | online (passo 9) |
| GER-GER-002 | Limpeza e conservacao | **Não conforme**, observação "Acúmulo de óleo na base do gerador." e uma foto (o item exige observação e evidência na falha) | online (passos 9 e 10) |
| GER-GER-003 | Estado da carenagem | Bom | online (passo 9) |
| GER-SEG-001 | Botao de emergencia | Sim | offline (passo 13) |
| GER-SEG-002 | Sinalizacao de seguranca | Conforme | offline |
| GER-FUN-001 | Tensao de saida (V) | 380 | offline |
| GER-FUN-002 | Horimetro (h) | 3900 (maior que os 3845 da inspeção anterior) | offline |
| GER-FUN-003 | Data da ultima manutencao | 10/05/2026 | offline |
| GER-REG-001 | Ordem de servico | OS-2026-0201 | offline |
| GER-REG-002 | Parecer do tecnico | "Gerador operando; limpeza da base recomendada." | offline (opcional) |

## Checklist de preparação

Na véspera:

- [ ] Branches da [base verificada](#roteiro-da-demonstração-ponta-a-ponta-ac-release) atualizadas nos três repositórios, com `npm install` feito no painel e no aplicativo.
- [ ] Imagem da API compilada uma vez (`docker compose up -d --build`), para não depender da internet do local para baixar dependências do Maven.
- [ ] QR do gerador impresso (ou aberto num segundo aparelho) e uma foto qualquer para o item GER-GER-002 (o próprio QR serve).
- [ ] Ensaio completo feito e registrado no [registro de ensaios](#registro-de-ensaios).

No dia, uma hora antes:

- [ ] Banco recriado a partir das migrações e do seed, para a inspeção `ASSIGNED` ficar no dia corrente: na pasta `backend`, `docker compose down -v` e depois `docker compose up -d`.
- [ ] `docker compose ps` com `fieldops-db` e `fieldops-api` `healthy`; `http://localhost:8080/actuator/health` responde `UP`.
- [ ] Painel no ar com `NEXT_PUBLIC_APP_ENV=staging` (`npm run dev`); o cabeçalho mostra **Homologação**, nunca **Desenvolvimento**.
- [ ] IP da máquina conferido com `ipconfig` e gravado em `EXPO_PUBLIC_API_URL` do aplicativo; `npx expo start --clear` rodando.
- [ ] No celular, `http://<IP-DA-MÁQUINA>:8080/actuator/health` abre no navegador. Celular e máquina na mesma rede Wi-Fi; rede marcada como privada no Windows e porta `8080` liberada no firewall.
- [ ] Aplicativo sem sessão e sem dados de ensaios anteriores (sair da conta ou limpar os dados do Expo Go).
- [ ] Permissões de câmera e localização do Expo Go **ainda não concedidas**, para mostrar a explicação antes do pedido do sistema (ou já concedidas, se o tempo for curto; decidir antes).
- [ ] Localização do celular ligada; GPS testado ao ar livre ou perto de janela.
- [ ] Celular carregado (acima de 80%), brilho alto, notificações silenciadas, modo avião **desligado**.
- [ ] Swagger aberto em `http://localhost:8080/swagger-ui.html` numa aba separada, para as contingências.
- [ ] Painel aberto na tela de login; aplicativo aberto na tela de login.

## Roteiro

Tempo total previsto: cerca de 20 minutos, sem perguntas. Os tempos são estimativas até o primeiro ensaio.

| # | Passo AC-RELEASE | Ator | Tela | Ação | Resultado esperado | Tempo |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | Login administrativo | Supervisor | Painel, `/login` | Entrar com `supervisor@fieldops.local` / `Demo@1234` | Abre o `/dashboard`; o cabeçalho mostra o nome, o perfil e o ambiente **Homologação** | 0:30 |
| 2 | Cadastro ou seleção de cliente | Supervisor | Painel, **Clientes** | Buscar e abrir **Industria ABC** | Detalhe do cliente com os dois locais | 1:00 |
| 3 | Local e equipamento | Supervisor | Painel, **Locais** e **Equipamentos** | Abrir **Fabrica Sorocaba** e depois **Gerador GD-001** | Equipamento com patrimônio `GD-001` e QR `FIELDOPS-GD-001` | 1:00 |
| 4 | Modelo publicado | Supervisor | Painel, **Modelos de inspeção** | Abrir **Checklist de Inspecao de Geradores** e a lista de versões | "Publicado · versão 1"; a versão aceita novos agendamentos | 1:00 |
| 5 ⚠️ | Agendamento e atribuição | Supervisor | Painel, **Inspeções** | Filtrar por **Atribuída** e mostrar **Inspecao preventiva - Gerador GD-001**, agendada para hoje, atribuída ao Tecnico Demo | Inspeção atribuída visível com técnico, prazo e prioridade alta | 1:00 |
| 6 | Login do técnico | Técnico | App, login | Entrar com `tecnico@fieldops.local` / `Demo@1234` | Abre o **Início** com o ambiente indicado e a inspeção em **Hoje** | 0:30 |
| 7 | Download da inspeção | Técnico | App, aba **Inspeções** | Puxar a lista para baixo para baixar | A inspeção aparece com o selo de sincronização; abrir o detalhe mostra cliente, local, equipamento e o checklist de 10 itens | 1:00 |
| 8 | Início | Técnico | App, detalhe → **Iniciar inspeção** | Ler a explicação da localização, conceder a permissão e tocar **Confirmar início** | Estado **Em andamento**; abre o checklist | 1:00 |
| 9 | Resposta do checklist | Técnico | App, checklist | Responder a seção 1 conforme as [respostas planejadas](#respostas-planejadas), incluindo a observação do GER-GER-002 | Cada resposta salva na hora ("Cada resposta é salva neste aparelho na hora"); o progresso avança | 1:30 |
| 10 | Foto e QR Code | Técnico | App, câmera do item GER-GER-002; depois **Início** → **Ler QR Code do equipamento** | Fotografar e confirmar a foto; em seguida ler o QR impresso | A miniatura aparece no item com a marca de envio pendente; a leitura mostra o Gerador GD-001 e a inspeção dele | 2:00 |
| 11 | Localização | Técnico | App, detalhe da inspeção | Mostrar o cartão de localização registrado no início e tocar **Abrir no mapa** | Posição, precisão e horário do registro no início | 0:45 |
| 12 | Perda simulada de conectividade | Técnico | Celular, central de controle | Ligar o **modo avião** (não desligar a API: o painel precisa continuar no ar) | Aviso global "Sem conexão com a internet" no topo, com a quantidade de registros aguardando envio | 0:30 |
| 13 | Conclusão offline | Técnico | App, checklist → resumo | Responder as seções 2 a 4, abrir o resumo e tocar **Concluir inspeção** | Resumo sem pendências; a inspeção fica salva no aparelho como **aguardando envio** | 2:30 |
| 14 | Retorno da conectividade | Técnico | Celular, central de controle | Desligar o modo avião | O aviso some; depois de 3 s online o envio automático começa sozinho | 0:30 |
| 15 | Sincronização sem duplicidade | Técnico | App, aba **Sincronização** | Esperar o envio automático e tocar **Sincronizar** mais uma vez | Última sincronização "tudo sincronizado"; o segundo toque não envia nada novo; selo **enviado** na inspeção | 1:00 |
| 16 | Visualização administrativa | Supervisor | Painel, **Inspeções** → detalhe | Abrir a inspeção, agora **Enviada** | Planejamento, horários do dispositivo e do servidor lado a lado, localização e progresso 10 de 10 | 1:30 |
| 17 ⚠️ | Aprovação ou reprovação | Supervisor | Swagger (contingência como caminho principal) | `POST /inspections/{id}/begin-review` e depois `POST /inspections/{id}/approve` com `{"comments": "Aprovada; programar a limpeza da base."}` | `200` nas duas chamadas; o painel passa a mostrar **Aprovada** | 1:30 |
| 18 | Atualização no aplicativo | Técnico | App, aba **Inspeções** | Puxar a lista para baixo | A inspeção aparece como **Aprovada** e não pode mais ser editada | 0:45 |
| 19 | Consulta do histórico mínimo | Supervisor | Painel, detalhe da inspeção, linha do tempo | Mostrar a linha do tempo | Os eventos desde o início no passo 8 até a aprovação (início, respostas e fotos sincronizadas, envio, decisão), com autor e horário do servidor e, quando veio do app, do dispositivo | 1:00 |

### Como chamar a API pelo Swagger (passos 5, 15, 17 e 19)

1. Em `http://localhost:8080/swagger-ui.html`, abra `POST /api/v1/auth/login` e envie `{"email": "supervisor@fieldops.local", "password": "Demo@1234"}`.
2. Copie o `accessToken` da resposta, clique em **Authorize** e cole o token.
3. Use o id da inspeção da demonstração: `d0000000-0000-4000-8000-000000000501`.

O token vale 15 minutos (`JWT_ACCESS_TOKEN_TTL`); se uma chamada responder `401`, repita o login.

## Contingência por passo

| # | Se falhar | O que fazer |
| --- | --- | --- |
| 1 | Login do painel recusado | Conferir se a API subiu no profile `demo` (`docker compose logs api`). Sem tempo para investigar, entrar com `admin@fieldops.local`. |
| 1–4 | Painel não carrega dados ou mostra "API_BASE_URL nao configurada." | Conferir o `.env.local` e reiniciar `npm run dev`. Enquanto isso, mostrar os mesmos dados pelo Swagger: `GET /clients`, `GET /sites`, `GET /equipment`, `GET /inspection-templates`. |
| 5 ⚠️ | **Gap conhecido**: fora de `development`, o formulário de nova inspeção do painel não lista nenhum modelo publicado, então o agendamento pelo painel não fecha | O caminho principal é mostrar a inspeção já atribuída pelo seed. Se a banca pedir um agendamento ao vivo, criar pelo Swagger com `POST /inspections` (corpo conforme o esquema mostrado no Swagger) usando o modelo, o cliente, o local, o equipamento e o técnico do seed. Não usar o modo `development` do painel: os dados dele não chegam ao aplicativo. |
| 6 | Login do app diz que não há conexão | O app não alcança a API: conferir `EXPO_PUBLIC_API_URL`, a mesma rede Wi-Fi e o `actuator/health` no navegador do celular. Reiniciar com `npx expo start --clear` depois de corrigir. |
| 7 | A inspeção não aparece ou o download falha | Puxar a lista de novo. Conferir que o app está na branch do app#70 e com `EXPO_PUBLIC_DEMO_INSPECTIONS` desligado. Confirmar pelo Swagger, logado como técnico, que `GET /mobile/inspections` devolve a inspeção. |
| 8 | Localização negada ou sem GPS | Seguir sem localização: negar a permissão não trava a inspeção. Comentar que a posição é opcional por permissão e mostrar a localização no passo 11 de uma das inspeções do ensaio, se houver. |
| 9 | Uma resposta não aceita o valor | Conferir o tipo do item (número, data, opção). O app recusa valor fora do tipo de propósito; corrigir o valor conforme a tabela de respostas planejadas. |
| 10 (foto) | Câmera negada ou foto recusada | Conceder pelo atalho das configurações que o app oferece. Sem câmera, trocar a resposta do GER-GER-002 para Conforme e seguir sem foto, comentando a alternativa. |
| 10 (QR) | O QR não lê (reflexo, foco, tela) | Tocar **Ler novamente** e aproximar; sem sucesso, usar a identificação manual autorizada: digitar `FIELDOPS-GD-001` ou o patrimônio `GD-001` e comentar a alternativa. |
| 11 | O mapa não abre | Mostrar só as coordenadas e a precisão no cartão; o mapa depende de internet. |
| 12 | Aviso de offline não aparece | Conferir se o Wi-Fi também desligou no modo avião (alguns aparelhos mantêm o Wi-Fi). Desligar o Wi-Fi à mão. Não parar a API. |
| 13 | Conclusão bloqueada | O resumo lista o que falta (obrigatório sem resposta, observação ou foto). Completar o item indicado e concluir de novo. |
| 14–15 | O envio não começa ou termina em falha | Tocar **Sincronizar**. Se uma operação falhar, abrir os detalhes da sincronização e ler o motivo. Uma foto recusada reabre a inspeção para correção: descartar a foto e concluir de novo. Para provar que não houve duplicidade, logar como supervisor no Swagger e conferir `GET /inspections/{id}/responses` (10 respostas, ou 9 sem o parecer) e `GET /inspections/{id}/evidence` (1 foto). |
| 16 | O detalhe não abre ou um bloco aparece como indisponível | Mostrar `GET /inspections/{id}` e `GET /inspections/{id}/responses` no Swagger. As miniaturas das fotos não carregam no painel (caminho do arquivo divergente); a foto pode ser mostrada por `GET /evidence/{id}` no Swagger. |
| 17 ⚠️ | **Gap conhecido**: a tela de revisão do painel só abre a inspeção de exemplo de `development` | O caminho principal já é o Swagger. Para reprovar em vez de aprovar: `POST /inspections/{id}/reject` com `{"reason": "Foto da base não mostra a extensão do vazamento.", "itemsToCorrect": ["<id do item GER-GER-002 em GET /mobile/inspections/{id}>"]}`. `409` em `begin-review` indica que a inspeção não está enviada: voltar ao passo 15. |
| 18 | O estado não muda no app | Puxar a lista de novo; conferir a rede do celular. Mostrar o estado pelo painel ou pelo Swagger. |
| 19 | A linha do tempo do painel não carrega | Mostrar `GET /inspections/{id}/history` no Swagger. As inspeções do seed não têm eventos anteriores: o histórico começa no início feito pelo app no passo 8, como esperado. |

## Pontos de atenção conhecidos

Verificados no código da base acima; afetam a apresentação e estão também na seção "O que ainda não funciona integrado" do [guia de execução local](execucao-local.md#o-que-ainda-não-funciona-integrado).

- **Agendamento e revisão pelo painel** não estão integrados à API (passos 5 e 17). Até a integração, a demonstração depende do Swagger nesses dois passos.
- **Fotos no painel**: o painel pede `GET /evidence/{id}/file`, e a API serve o arquivo em `GET /evidence/{id}`; as miniaturas aparecem quebradas ou como indisponíveis.
- **Dashboard e auditoria** do painel aparecem como indisponíveis fora de `development`, porque a API não tem `GET /dashboard/summary` nem `GET /audit-events`. Não abrir essas telas na apresentação.
- **`GET /mobile/inspections/{id}` devolve sempre `responses: []`**. Use um único aparelho do início ao fim: um segundo aparelho, ou o mesmo depois de limpar os dados, não recebe o que já foi respondido.
- **Inspeções do seed sem histórico**: só as ações feitas durante a demonstração aparecem na linha do tempo.
- **Não alternar o painel para `development`** durante a apresentação: nesse modo aprovações e cancelamentos acontecem só na memória do painel e não chegam ao aplicativo.

## Registro de ensaios

O card exige ao menos um ensaio completo antes da apresentação. Registre cada um, com o passo onde houve problema e a contingência usada:

| Data | Quem participou | Base (commits) | Passos concluídos | Problemas e contingências usadas | Tempo total |
| --- | --- | --- | --- | --- | --- |
| — | — | — | pendente | — | — |
