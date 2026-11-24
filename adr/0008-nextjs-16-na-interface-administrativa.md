# ADR-0008 — Next.js 16 (App Router) no lugar do Angular sugerido no documento 11.5

- Status: aceito
- Data: 2026-11-24
- Sprint: 1

## Contexto

O documento 11.1 desenha a interface administrativa em Angular + TypeScript, e o documento
11.5 sugere uma estrutura Angular (`core/guards`, `core/interceptors`, serviços tipados
injetáveis, formulários reativos). A equipe decidiu adotar Next.js 16 com App Router. Esta é
uma divergência consciente da documentação original e precisa ficar registrada para que o
avaliador e a equipe não sigam o documento 11.5 como se ainda valesse.

O painel lida com sessão de usuários administrativos. Guardar o JWT no navegador
(`localStorage` ou cookie legível por JavaScript) deixaria o token exposto a qualquer script
carregado na página.

## Decisão

A interface administrativa (`frontend`) usa **Next.js 16 com App Router**, React 19 e
TypeScript em modo estrito. Em relação ao documento 11.5, muda o seguinte:

| Documento 11.5 (Angular) | Adotado (Next.js 16) |
| --- | --- |
| Guards de rota em `core/guards` | `src/proxy.ts` (em Next.js 16, substitui o `middleware.ts`) faz só a checagem barata de presença do cookie de sessão. A barreira real é a camada de acesso a dados `src/lib/dal.ts`, marcada `server-only`, que verifica sessão e perfil em cada Server Component, Server Action e Route Handler. |
| Serviços tipados injetáveis e interceptador HTTP | Server Actions (`'use server'`, em `src/features/*/actions.ts`) para escrita e leitura pela camada de acesso a dados no servidor. O cliente HTTP (`src/lib/api/client.ts`) roda apenas no servidor do Next. |
| Token tratado no navegador pelo interceptador | Token de acesso e refresh token em cookies `httpOnly`, fora do alcance do JavaScript da página. O navegador nunca recebe o token. |
| Navegador chama a API | O navegador fala só com o servidor do Next; o servidor do Next chama a API (`API_BASE_URL`, nunca com prefixo `NEXT_PUBLIC_`). Downloads protegidos, como evidências, passam por Route Handlers (`src/app/api/evidence/[evidenceId]`). |
| Estrutura `src/app/core`, `shared`, `features` | Rotas em `src/app` (grupos `(app)` e `(public)`), funcionalidades em `src/features`, infraestrutura em `src/lib`. |

Continua valendo do documento 11.5: carregamento por rota, componentes reutilizáveis para
tabelas, filtros e formulários, e autorização real na API (RN-003). Esconder botão não é
controle de acesso.

## Alternativas consideradas

- **Angular, como sugerido no documento 11.5**: atende o escopo, mas a sessão ficaria no
  navegador e a proteção dependeria de guard e interceptador no cliente.
- **SPA React sem servidor próprio**: mesma exposição do token no navegador e sem lugar para
  a camada de acesso a dados no servidor.

## Consequências

- O documento 11.5 deixa de descrever a estrutura do `frontend`; a referência passa a ser o
  README do `frontend` e este ADR.
- O servidor do Next precisa alcançar a API pela rede; se a API estiver em rede privada, o
  painel precisa estar na mesma rede.
- Toda Server Action e todo Route Handler precisa chamar a camada de acesso a dados, porque
  cada um é um ponto de entrada independente.
- Os diagramas (ver [diagrama de contexto](../diagramas/contexto.md)) mostram o painel como
  servidor Next.js entre o navegador e a API.
