

## Documentação
- [Contrato compartilhado](contrato-compartilhado.md): fonte única de enums, identificadores, datas, formato de erro, códigos de erro e paginação usados por `backend`, `frontend` e `app`.
- [Documentação do projeto (Notion)](notion/README.md)

### Decisões de arquitetura (ADR)
Cada decisão de arquitetura fica em um arquivo numerado em [`adr/`](adr/), a partir do [template](adr/0000-template.md). Abra um novo ADR quando a decisão:
- mudar ou contrariar o que está na documentação do Notion (como o ADR-0008);
- afetar mais de uma aplicação ou o [contrato compartilhado](contrato-compartilhado.md);
- for difícil de desfazer depois (stack, persistência, formato de sincronização, segurança);
- tiver sido discutida mais de uma vez pela equipe.

Um ADR aceito não é editado para mudar a decisão: a nova decisão vira outro ADR, e o antigo passa a `substituído por ADR-NNNN`. Decisões internas de um único repositório podem ficar no próprio repositório, como os ADRs do `backend` em [`backend/docs/adr`](https://github.com/FieldOps-Project/backend/tree/develop/docs/adr), que usam numeração própria (`ADR-001`, `ADR-002`).

Os ADRs 0001 a 0007 registram depois do fato as decisões do [Anexo B](notion/20-anexo-b-decisoes-de-escopo-consolidadas.md); as alternativas foram reconstruídas a partir da documentação.

| ADR | Decisão | Status |
| --- | --- | --- |
| [0001](adr/0001-tres-aplicacoes-com-contrato-de-api.md) | Três aplicações independentes compartilhando contrato de API | aceito |
| [0002](adr/0002-sqlite-como-persistencia-operacional.md) | SQLite como persistência operacional, não cache descartável | aceito |
| [0003](adr/0003-sincronizacao-por-outbox-idempotente.md) | Sincronização por outbox com operações idempotentes | aceito |
| [0004](adr/0004-modelo-versionado-com-snapshot.md) | Modelo de inspeção versionado com snapshot por inspeção | aceito |
| [0005](adr/0005-uuid-gerado-no-dispositivo.md) | UUID gerado também no dispositivo | aceito |
| [0006](adr/0006-android-como-plataforma-de-validacao.md) | Android como plataforma de validação do mobile | aceito |
| [0007](adr/0007-revisao-pela-interface-administrativa.md) | Revisão realizada exclusivamente pela interface administrativa | aceito |
| [0008](adr/0008-nextjs-16-na-interface-administrativa.md) | Next.js 16 (App Router) no lugar do Angular sugerido no documento 11.5 | aceito |

### Diagramas
Diagramas em Mermaid, versionados em [`diagramas/`](diagramas/):
- [Máquina de estados da inspeção](diagramas/maquina-de-estados-inspecao.md) — as 10 transições da tabela 7.6.
- [Diagrama de contexto](diagramas/contexto.md) — mobile, painel, API, PostgreSQL e armazenamento de evidências.
- [Diagrama de classes](diagramas/classes.md) — entidades do documento 10.

## Contribuição
Veja o [Guia de Contribuição](https://github.com/FieldOps-Project/docs/blob/main/CONTRIBUTING.md) para detalhes sobre fluxo de trabalho, padrões de commit e nomenclatura de branches.


## Membros do grupo
- Felipe Marques
- Gabriel Ribeiro
- Pedro Dias
- André Fernandes
- João Victor Souza
