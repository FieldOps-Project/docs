# ADR-0001 — Três aplicações independentes compartilhando contrato de API

- Status: aceito
- Data: 2026-11-24
- Sprint: 1

## Contexto

O produto tem três públicos com necessidades diferentes: o técnico em campo, o supervisor e o
administrador no escritório, e as regras centrais de negócio. O documento 11.1 descreve uma
arquitetura distribuída, e o Anexo B (decisões 1 e 2) fixa que o MVP tem aplicativo mobile,
interface administrativa web e API REST, e que a interface administrativa não será substituída
pelo Swagger. As três partes são desenvolvidas em paralelo, muitas vezes antes de a API estar
pronta.

## Decisão

O FieldOps é composto por três aplicações independentes, cada uma em seu repositório:

| Repositório | Aplicação | Stack |
| --- | --- | --- |
| `backend` | API REST | Java 21, Spring Boot 4, PostgreSQL |
| `frontend` | Interface administrativa | Next.js 16, React 19 (ver ADR-0008) |
| `app` | Aplicativo mobile | Expo, React Native, SQLite |

- A API é a única dona das regras de negócio e da autorização. Painel e aplicativo nunca
  acessam o banco diretamente.
- O contrato entre elas é a API REST `/api/v1`, descrita pelo OpenAPI publicado pelo
  `backend` (`openapi/openapi.json`) e, para enums, identificadores, datas e erros, pelo
  [contrato compartilhado](../contrato-compartilhado.md).
- Cada aplicação tem build, testes e implantação próprios.

## Alternativas consideradas

- **Monólito com páginas renderizadas pelo servidor da API**: não atende o aplicativo
  offline, que precisa de API própria.
- **Repositório único (monorepo)**: reduziria a duplicação de tipos, mas misturaria três
  pipelines e três stacks diferentes; a equipe optou por repositórios separados com contrato
  documentado.
- **Usar o Swagger como interface administrativa**: descartado pelo Anexo B, decisão 2.

## Consequências

- As três aplicações avançam em paralelo, desde que respeitem o contrato.
- Enums e códigos de erro são duplicados em três linguagens; divergência só aparece na
  integração. O contrato compartilhado é a fonte única e precisa ser atualizado no mesmo PR da
  mudança no `backend` (RN-090).
- Mudanças incompatíveis no contrato exigem novo ADR.
