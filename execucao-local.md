# Guia de execução local do ecossistema

Como subir, a partir do zero, a API, o painel administrativo e o aplicativo do FieldOps numa mesma máquina, apontados um para o outro e com os dados de demonstração já carregados, sem editar o banco à mão. Atende o card [docs#4](https://github.com/FieldOps-Project/docs/issues/4) (PBI-067) e a seção 11.12 do documento [11 - Arquitetura](notion/11-arquitetura.md#1112-ambientes).

- **Última verificação:** 2026-11-24
- **Base verificada:**
  - `backend` na branch `feat/EP-10-demo-container` (PR [backend#83](https://github.com/FieldOps-Project/backend/pull/83), que inclui o seed do PR [backend#82](https://github.com/FieldOps-Project/backend/pull/82));
  - `frontend` em `develop` (commit `957e7b4`);
  - `app` em `develop` (commit `f00b9ac`) e o PR [app#70](https://github.com/FieldOps-Project/app/pull/70), que alinha o envio ao contrato do backend.
- Os comandos foram conferidos contra os arquivos dessas branches. O guia **ainda não foi executado de ponta a ponta por alguém que não o escreveu**; o registro fica na seção [Validação do guia](#validação-do-guia).

> **Branch padrão.** `main` de `app` e `frontend` ainda contém só o README. O código está em `develop` nos três repositórios. Enquanto os PRs backend#82 e #83 não forem integrados, o Docker Compose e o seed existem só na branch `feat/EP-10-demo-container`.

## Visão geral

```text
           navegador                               aparelho / emulador
               |                                           |
   http://localhost:3000                     EXPO_PUBLIC_API_URL (IP da máquina)
               |                                           |
     painel (Next.js, servidor) --API_BASE_URL-->  API (Spring Boot) :8080  <-- Docker
                                                       |
                                              PostgreSQL :5433 (host)  <-- Docker
                                              evidências: volume fieldops-evidence
```

O painel chama a API **a partir do servidor do Next**, não do navegador. O aplicativo chama a API direto do aparelho, por isso precisa do IP da máquina na rede local.

## Pré-requisitos

| Ferramenta | Versão | Para quê | Como conferir |
| --- | --- | --- | --- |
| Git | qualquer recente | clonar os três repositórios | `git --version` |
| Docker Desktop (ou Docker Engine) com Compose v2 | Compose v2 | banco e API | `docker compose version` |
| Node.js | 22 LTS ou superior | painel e aplicativo | `node -v` |
| npm | 10 ou superior | painel e aplicativo | `npm -v` |
| Android Studio com um AVD Android 13 (API 33) ou superior | Ladybug ou superior | emulador Android (plataforma de validação, ADR-0006) | `adb devices` |
| Expo Go no celular | a da loja | só se for usar aparelho físico em vez de emulador | — |
| JDK 21 | 21 | só para rodar a API **fora** do Docker | `java -version` |

Não é preciso instalar Java nem PostgreSQL para o caminho principal: os dois rodam dentro do Docker.

Portas que precisam estar livres na máquina: `5433` (banco), `8080` (API), `3000` (painel) e `8081` (Metro, do Expo).

## Portas e URLs

| Componente | Endereço | Observação |
| --- | --- | --- |
| API | `http://localhost:8080/api/v1` | prefixo de todas as rotas |
| Swagger UI | `http://localhost:8080/swagger-ui.html` | botão **Authorize** recebe o `accessToken` do login |
| OpenAPI | `http://localhost:8080/v3/api-docs` | usado por `npm run types:api` no app |
| Saúde da API | `http://localhost:8080/actuator/health` | responde `{"status":"UP"}` sem autenticação |
| PostgreSQL | `localhost:5433`, banco `fieldops` | usuário e senha do `.env` do backend |
| Painel administrativo | `http://localhost:3000` | a raiz redireciona para `/dashboard` |
| Metro (Expo) | `http://localhost:8081` | aberto por `npx expo start` |
| API vista do emulador Android | `http://10.0.2.2:8080` | `10.0.2.2` é a máquina hospedeira |
| API vista do aparelho físico | `http://<IP-DA-MÁQUINA>:8080` | mesma rede Wi-Fi |

> O card cita `npm start` na porta `4200` para o painel, herança do Angular sugerido no documento 11.5. O painel é Next.js ([ADR-0008](https://github.com/FieldOps-Project/docs/blob/docs/EP-01-adr-diagrams/adr/0008-nextjs-16-na-interface-administrativa.md)) e sobe na porta `3000`.

## Ordem de subida

### 0. Clonar os repositórios

```bash
git clone https://github.com/FieldOps-Project/backend.git
git clone https://github.com/FieldOps-Project/frontend.git
git clone https://github.com/FieldOps-Project/app.git

cd backend  && git checkout feat/EP-10-demo-container && cd ..   # develop, depois do merge de backend#82 e #83
cd frontend && git checkout develop && cd ..
cd app      && git checkout feat/EP-08-backend-contract && cd .. # develop, depois do merge de app#70
```

### 1. Banco e API (Docker)

Na pasta `backend`:

```bash
# Linux / macOS / Git Bash
cp .env.example .env
docker compose up -d --build
docker compose ps
```

```powershell
# Windows PowerShell
Copy-Item .env.example .env
docker compose up -d --build
docker compose ps
```

O que acontece:

1. O serviço `db` (`postgres:16-alpine`) sobe e publica a porta `5433` do host.
2. A imagem da API é compilada pelo `Dockerfile` (a primeira vez baixa as dependências do Maven e demora alguns minutos).
3. O serviço `api` só inicia depois do `pg_isready` do banco, aplica as migrações do Flyway e, como o `.env.example` define `SPRING_PROFILES_ACTIVE=demo`, aplica também o seed `db/demo/R__demo_seed.sql`.
4. A API fica `healthy` quando `GET /actuator/health` responde `UP`.

Espere a coluna `STATUS` de `fieldops-api` mostrar `(healthy)` em `docker compose ps`. Para acompanhar: `docker compose logs -f api`.

Conferência:

```bash
curl http://localhost:8080/actuator/health        # Linux / macOS / Git Bash
```

```powershell
curl.exe http://localhost:8080/actuator/health    # Windows: use curl.exe, não o alias curl do PowerShell
```

Resposta esperada: `{"status":"UP"}` (pode trazer mais campos).

#### Alternativa: API fora do Docker

Útil para depurar a API pela IDE. Suba só o banco e rode a API com o Maven Wrapper (exige JDK 21):

```bash
docker compose up -d db
SPRING_PROFILES_ACTIVE=demo JWT_SECRET=<32+ caracteres> DB_URL=jdbc:postgresql://localhost:5433/fieldops DB_USER=fieldops DB_PASSWORD=fieldops-demo ./mvnw spring-boot:run
```

```powershell
docker compose up -d db
$env:SPRING_PROFILES_ACTIVE="demo"; $env:JWT_SECRET="<32+ caracteres>"; $env:DB_URL="jdbc:postgresql://localhost:5433/fieldops"
$env:DB_USER="fieldops"; $env:DB_PASSWORD="fieldops-demo"
.\mvnw.cmd spring-boot:run
```

`DB_USER` e `DB_PASSWORD` precisam ser os mesmos do `.env` usado para criar o banco. Não rode as duas formas ao mesmo tempo: as duas usam a porta `8080`.

### 2. Seed de demonstração

Não há passo manual: o seed roda junto com as migrações quando o profile é `demo`. Nenhum outro profile carrega o seed. O conteúdo está na seção [Dados de demonstração](#dados-de-demonstração).

### 3. Painel administrativo

Na pasta `frontend`:

```bash
npm install
cp .env.example .env.local          # Windows PowerShell: Copy-Item .env.example .env.local
```

Edite o `.env.local` para usar a API de verdade:

```dotenv
API_BASE_URL=http://localhost:8080/api/v1
SESSION_SECRET=qualquer-valor-local
NEXT_PUBLIC_APP_ENV=staging
```

```bash
npm run dev
```

O painel sobe em `http://localhost:3000`.

> **`NEXT_PUBLIC_APP_ENV=development` liga dados de exemplo dentro do painel.** Com esse valor, a listagem e o detalhe de inspeções, a fila de revisão, a revisão, o dashboard, os modelos, as não conformidades, as evidências e a auditoria passam a usar dados em memória do próprio painel, e não o que está na API. Para ver o seed e o que o aplicativo enviou, use `staging` (qualquer valor diferente de `development` faz o painel chamar a API). O login vai para a API nos dois casos.

Alternativa sem Node: o repositório tem um `Dockerfile` de produção (porta `3000`). As variáveis `API_BASE_URL` e `SESSION_SECRET` são lidas em tempo de execução; como o painel roda dentro de um contêiner, `localhost` aponta para o próprio contêiner, então a `API_BASE_URL` precisa usar `http://host.docker.internal:8080/api/v1` (Docker Desktop). Esse caminho não está no Compose do backend e não foi verificado; o caminho suportado por este guia é `npm run dev`.

### 4. Aplicativo

Na pasta `app`:

```bash
npm install
cp .env.example .env                # Windows PowerShell: Copy-Item .env.example .env
```

Ajuste o `.env` conforme onde o aplicativo vai rodar (detalhes em [Apontar o aplicativo para a API](#apontar-o-aplicativo-para-a-api-na-rede-local)):

```dotenv
EXPO_PUBLIC_API_URL=http://10.0.2.2:8080
EXPO_PUBLIC_APP_ENV=local
```

Com o emulador aberto (ou o celular com Expo Go na mesma rede):

```bash
npx expo start --clear
```

Com o Metro no ar, pressione `a` para abrir no emulador Android, ou leia o QR Code do terminal com o Expo Go no celular. Na primeira execução o Expo instala o Expo Go no emulador; o projeto usa apenas módulos incluídos no Expo Go, então não é preciso gerar build nativa.

> `EXPO_PUBLIC_API_URL` é a URL **sem** `/api/v1`: o aplicativo acrescenta o prefixo em cada rota. O card sugere `http://192.168.x.x:8080/api/v1`, que resultaria em `/api/v1/api/v1/...`.

## Variáveis de ambiente

Nenhum valor abaixo é segredo real. Os valores do backend são os do `.env.example`, feitos para a demonstração; troque `DB_PASSWORD` e `JWT_SECRET` em qualquer outro uso.

### Backend (`backend/.env`, lido pelo Docker Compose)

| Variável | Obrigatória | Exemplo | Se estiver ausente |
| --- | --- | --- | --- |
| `DB_USER` | sim | `fieldops` | `docker compose` recusa subir: "defina DB_USER no .env" |
| `DB_PASSWORD` | sim | `fieldops-demo` | `docker compose` recusa subir: "defina DB_PASSWORD no .env" |
| `JWT_SECRET` | sim, com 32 caracteres ou mais | `demo-only-jwt-secret-change-me-0123456789` | `docker compose` recusa subir: "defina JWT_SECRET no .env" |
| `SPRING_PROFILES_ACTIVE` | não (padrão `demo` no Compose) | `demo` | com outro profile, o seed não é aplicado e não há usuários para entrar |

Variáveis opcionais lidas pela API (`application.yml`), com o padrão usado quando ausentes. O Compose não as repassa; para mudá-las no Docker é preciso acrescentá-las em `environment` do serviço `api`.

| Variável | Padrão | Uso |
| --- | --- | --- |
| `DB_URL` | `jdbc:postgresql://localhost:5433/fieldops` (o Compose define `jdbc:postgresql://db:5432/fieldops`) | conexão com o banco |
| `JWT_ACCESS_TOKEN_TTL` | `PT15M` | validade do access token |
| `JWT_REFRESH_TOKEN_TTL` | `P30D` | validade do refresh token |
| `APP_EVIDENCE_MAX_SIZE_BYTES` | `10485760` (10 MB) | tamanho máximo de evidência |
| `APP_EVIDENCE_STORAGE_DIR` | `/app/evidence` na imagem | diretório das evidências (volume `fieldops-evidence`) |
| `APP_SYNC_MAX_OPERATIONS` | `100` | operações aceitas por `POST /mobile/sync/push` |
| `APP_CORS_ALLOWED_ORIGINS` | no profile `demo`: `http://localhost:4200,http://localhost:3000` | origens de navegador aceitas; o painel e o app não dependem disso, porque não chamam a API de um navegador |

### Painel (`frontend/.env.local`)

| Variável | Obrigatória | Exposta ao navegador | Exemplo | Se estiver ausente |
| --- | --- | --- | --- | --- |
| `API_BASE_URL` | sim | não | `http://localhost:8080/api/v1` | toda chamada à API falha com "API_BASE_URL nao configurada." |
| `SESSION_SECRET` | sim, segundo o `.env.example` | não | `qualquer-valor-local` | nada muda hoje: o código atual não lê esta variável (a sessão usa os tokens da API em cookies `httpOnly`) |
| `NEXT_PUBLIC_APP_ENV` | não | sim | `staging` | sem a variável o painel se identifica como produção; `development` troca a API por dados de exemplo (ver acima) |
| `SESSION_COOKIE_SECURE` | não (padrão `true`) | não | `false` | só use `false` para abrir o painel por HTTP fora de `localhost`, como em outra máquina da rede; em `localhost` o padrão funciona |

### Aplicativo (`app/.env`)

| Variável | Obrigatória | Exemplo | Se estiver ausente |
| --- | --- | --- | --- |
| `EXPO_PUBLIC_API_URL` | sim | `http://10.0.2.2:8080` | o aplicativo não abre: falha na inicialização com "Configuração de ambiente inválida" e explica o que corrigir |
| `EXPO_PUBLIC_APP_ENV` | não (padrão `local`) | `local` | fica `local`; fora de `production` o ambiente aparece no início, no login e no perfil |
| `EXPO_PUBLIC_DEMO_INSPECTIONS` | não (padrão `false`) | `false` | só existe em `develop` antes do app#70. `true` serve inspeções de exemplo em memória, em vez das do seed; deixe `false` |

Tudo que começa com `EXPO_PUBLIC_` ou `NEXT_PUBLIC_` vai para o bundle e fica legível para quem usa o aplicativo ou o painel. Nunca coloque senha, token ou credencial nessas variáveis. Depois de editar o `.env` do aplicativo, reinicie com `npx expo start --clear`: as variáveis são lidas no momento do bundle.

## Dados de demonstração

Carregados pelo seed do profile `demo`, com ids fixos. Rodar de novo não duplica nada e também não desfaz o que foi alterado num ensaio.

### Usuários

Senha igual para os três, fraca de propósito e só existente no profile `demo`: **`Demo@1234`**.

| E-mail | Perfil | Onde entra |
| --- | --- | --- |
| `admin@fieldops.local` | ADMIN | painel |
| `supervisor@fieldops.local` | SUPERVISOR | painel (é quem revisa) |
| `tecnico@fieldops.local` | TECHNICIAN | aplicativo (o painel recusa o perfil técnico) |

### Cenário

- Cliente **Industria ABC**, com os locais **Fabrica Sorocaba** e **Unidade Itu**.
- Equipamentos **Gerador GD-001** (QR `FIELDOPS-GD-001`, patrimônio `GD-001`), **Empilhadeira EMP-01** (`FIELDOPS-EMP-01`) e **Extintor EXT-15** (`FIELDOPS-EXT-15`). O QR do gerador para imprimir ou mostrar numa tela está em [`backend/docs/demo/qr-gerador-gd-001.svg`](https://github.com/FieldOps-Project/backend/blob/feat/EP-10-demo-container/docs/demo/qr-gerador-gd-001.svg).
- Modelo **Checklist de Inspecao de Geradores**, versão 1 publicada, com quatro seções e dez itens cobrindo os sete tipos de resposta.
- Cinco inspeções do gerador, todas do técnico e do supervisor de demonstração:

| Inspeção | Estado | Para quê |
| --- | --- | --- |
| Inspecao preventiva - Gerador GD-001 | `ASSIGNED` | a da demonstração, agendada para as 14h (UTC) do dia em que o seed rodou |
| Inspecao preventiva semanal - Gerador GD-001 | `IN_PROGRESS` | primeira seção respondida |
| Inspecao apos manutencao - Gerador GD-001 | `SUBMITTED` | aguardando revisão |
| Inspecao preventiva mensal - Gerador GD-001 | `APPROVED` | com uma não conformidade aberta |
| Inspecao de rotina - Gerador GD-001 | `REJECTED` | com motivo e o item do horímetro a corrigir |

As inspeções do seed não têm eventos de auditoria: o histórico delas começa na primeira ação feita pela API.

### Recomeçar do zero

```bash
docker compose down -v
docker compose up -d
```

`down -v` apaga os volumes do banco **e das evidências**; o `up` recria o banco, as migrações e o seed. Como as datas do seed são relativas ao momento em que ele roda, recomeçar também traz a inspeção `ASSIGNED` para o dia corrente. No aplicativo, saia da conta antes ou limpe os dados do Expo Go: o banco SQLite do aparelho guarda o que foi baixado antes do reset.

## Apontar o aplicativo para a API na rede local

`localhost`, dentro do emulador ou do celular, é o próprio aparelho, não a sua máquina. O endereço depende de onde o aplicativo roda:

| Onde o aplicativo roda | `EXPO_PUBLIC_API_URL` |
| --- | --- |
| Emulador Android | `http://10.0.2.2:8080` |
| Simulador iOS (opcional, só em macOS) | `http://localhost:8080` |
| Aparelho físico na mesma rede Wi-Fi | `http://<IP-DA-MÁQUINA>:8080` |

Descobrir o IP da máquina:

```powershell
# Windows: procure "Endereço IPv4" do adaptador Wi-Fi (ou Ethernet) em uso
ipconfig
```

```bash
# macOS
ipconfig getifaddr en0
# Linux
hostname -I
```

Passos para o aparelho físico:

1. Coloque o celular e a máquina na **mesma rede Wi-Fi**. Redes de visitante ou com isolamento de clientes bloqueiam a conexão entre aparelhos.
2. No celular, abra `http://<IP-DA-MÁQUINA>:8080/actuator/health` no navegador. Se não aparecer `UP`, o aplicativo também não vai alcançar a API; resolva antes de seguir.
3. Grave o IP no `.env` (`EXPO_PUBLIC_API_URL=http://192.168.0.10:8080`, por exemplo) e reinicie com `npx expo start --clear`.
4. No Windows, se o passo 2 falhar, libere a porta `8080` no Firewall do Windows Defender para redes privadas e confira se a rede Wi-Fi está marcada como **privada**, não pública.

O Docker publica a porta `8080` em todas as interfaces da máquina, então a API já aceita conexões da rede local. O tráfego HTTP sem TLS só é aceito pelo Android em build de desenvolvimento (Expo Go), que é o caso deste guia.

## Está tudo no ar?

- [ ] `docker compose ps` mostra `fieldops-db` e `fieldops-api` como `healthy`.
- [ ] `http://localhost:8080/actuator/health` responde `UP` e `http://localhost:8080/swagger-ui.html` abre.
- [ ] No painel (`http://localhost:3000`), `supervisor@fieldops.local` / `Demo@1234` entra e o cabeçalho mostra o ambiente **Homologação** (`staging`).
- [ ] Em **Inspeções** do painel aparecem as cinco inspeções do Gerador GD-001.
- [ ] No aplicativo, `tecnico@fieldops.local` / `Demo@1234` entra.
- [ ] Na lista do técnico aparece **Inspecao preventiva - Gerador GD-001**, atribuída.

## Problemas comuns

| Sintoma | Causa provável | O que fazer |
| --- | --- | --- |
| `docker compose up` para com "defina DB_USER no .env" (ou `DB_PASSWORD`, `JWT_SECRET`) | o `.env` não foi criado ou está em outra pasta | rode `cp .env.example .env` (PowerShell: `Copy-Item .env.example .env`) na pasta `backend` |
| "port is already allocated" na `5433` ou na `8080` | outro PostgreSQL, outra API ou uma execução anterior ocupando a porta | pare o processo, ou `docker compose down` numa execução antiga |
| `fieldops-api` fica em `starting` ou `unhealthy` | primeira compilação ainda em curso, ou erro na subida | `docker compose logs -f api`; o `healthcheck` dá 60 s de folga antes de contar falhas |
| API não sobe com erro de autenticação no banco depois de trocar `DB_PASSWORD` | o volume do banco foi criado com a senha antiga | `docker compose down -v` e suba de novo (apaga os dados) |
| Login recusado com as credenciais de demonstração | a API não subiu no profile `demo`, então o seed não foi aplicado | confira `SPRING_PROFILES_ACTIVE=demo` no `.env`, depois `docker compose down -v` e `docker compose up -d` |
| No Windows, `curl http://localhost:8080/...` devolve um objeto do PowerShell ou pede confirmação | `curl` é apelido de `Invoke-WebRequest` no Windows PowerShell | use `curl.exe` |
| No Windows PowerShell 5, `docker compose down -v && docker compose up -d` dá erro de sintaxe | o `&&` só existe a partir do PowerShell 7 | rode os dois comandos em linhas separadas |
| O painel mostra inspeções que não estão no seed, ou aprovar e cancelar "funcionam" mas nada muda na API | `NEXT_PUBLIC_APP_ENV=development` liga os dados de exemplo do painel | use `NEXT_PUBLIC_APP_ENV=staging` e reinicie `npm run dev` |
| O painel mostra "API_BASE_URL nao configurada." | `.env.local` ausente ou sem a variável | crie o `.env.local` na pasta `frontend` e reinicie `npm run dev` |
| O técnico não consegue entrar no painel | o painel é só para ADMIN e SUPERVISOR | entre com o técnico no aplicativo |
| O aplicativo abre com "Configuração de ambiente inválida" | `EXPO_PUBLIC_API_URL` ausente ou sem `http://` | ajuste o `.env` do app e rode `npx expo start --clear` |
| O login no aplicativo diz que não há conexão, mesmo com internet | o endereço de `EXPO_PUBLIC_API_URL` não é alcançável do aparelho | siga [Apontar o aplicativo para a API](#apontar-o-aplicativo-para-a-api-na-rede-local); `localhost` nunca funciona no emulador |
| Mudou o `.env` do aplicativo e nada mudou | variáveis `EXPO_PUBLIC_` são lidas no bundle | reinicie com `npx expo start --clear` |
| A lista do técnico mostra inspeções que não são do seed | `EXPO_PUBLIC_DEMO_INSPECTIONS=true` no `.env` do app | deixe `false` e reinicie com `--clear` |
| A inspeção da demonstração aparece como atrasada | o seed rodou num dia anterior | recomece do zero no dia da apresentação |
| Depois do reset, o aplicativo ainda mostra dados antigos | o SQLite do aparelho guarda o que foi baixado | saia da conta no aplicativo ou limpe os dados do Expo Go |

## O que ainda não funciona integrado

O guia sobe os três componentes, mas parte do painel ainda não fala com a API. Estes pontos foram verificados no código e afetam a demonstração; o [roteiro da demonstração](roteiro-demonstracao.md) indica a contingência de cada um.

| Ponto | Situação verificada | Pendência |
| --- | --- | --- |
| Agendamento pelo painel | fora de `development`, o formulário de nova inspeção não lista nenhum modelo publicado (`loadPublishedTemplateVersions` devolve lista vazia) | frontend: ligar o formulário a `GET /inspection-templates/{id}/versions` |
| Revisão pelo painel | a tela de revisão só abre a inspeção de exemplo de `development`; qualquer outra responde 404, e o painel não chama `POST /inspections/{id}/begin-review` | frontend: integrar a revisão com a API |
| Imagens das evidências no painel | o painel pede `GET /evidence/{id}/file`; a API serve o arquivo em `GET /evidence/{id}` | alinhar o caminho entre frontend e backend |
| Dashboard e auditoria no painel | a API não tem `GET /dashboard/summary` nem `GET /audit-events`; os blocos aparecem como indisponíveis | backend: publicar os endpoints (backend#36 e auditoria) |
| Respostas no detalhe mobile | `GET /mobile/inspections/{id}` devolve sempre `responses: []`; o que já foi respondido em outro aparelho não chega | backend: preencher as respostas no payload mobile |
| Histórico das inspeções do seed | o seed não grava eventos de auditoria | aceito: o histórico começa na primeira ação pela API |
| Envio do aplicativo | `develop` do app ainda envia parte do contrato de um jeito que o backend não aceita; o app#70 corrige | app: integrar o app#70 |
| Build instalável do aplicativo | os perfis `demo`, `integration` e `production` do `eas.json` apontam para endereços `.example`; não existe build publicada | app#35 e definição do servidor de demonstração |
| Painel publicado | existe `Dockerfile` de produção, mas ele não está no Compose nem publicado | frontend#27 |

## Validação do guia

O card exige que alguém que não escreveu o guia suba os três componentes seguindo só este documento. Registre aqui cada execução:

| Data | Quem executou | Sistema operacional | Resultado | Observações |
| --- | --- | --- | --- | --- |
| — | — | — | pendente | — |
