# Diagrama de contexto

Visão das três aplicações e dos serviços de que dependem (documento
[11 - Arquitetura](../notion/11-arquitetura.md), 11.1, atualizado pelo
[ADR-0001](../adr/0001-tres-aplicacoes-com-contrato-de-api.md) e pelo
[ADR-0008](../adr/0008-nextjs-16-na-interface-administrativa.md)).

```mermaid
flowchart TB
    tecnico([Técnico de campo])
    gestor([Supervisor / Administrador])

    subgraph dispositivo[Dispositivo Android]
        app["Aplicativo mobile<br/>Expo + React Native"]
        sqlite[("SQLite<br/>persistência operacional + outbox")]
        secure["Secure Store<br/>tokens"]
    end

    subgraph web[Interface administrativa]
        navegador["Navegador"]
        next["Servidor Next.js 16<br/>App Router, Server Actions,<br/>sessão em cookie httpOnly"]
    end

    api["API REST /api/v1<br/>Java 21 + Spring Boot 4"]
    pg[("PostgreSQL<br/>dados de negócio, auditoria,<br/>operações processadas")]
    storage[("Armazenamento de evidências<br/>arquivos das fotos")]

    tecnico --> app
    app <--> sqlite
    app <--> secure
    app -- "HTTPS / JSON / multipart<br/>Bearer token" --> api

    gestor --> navegador
    navegador -- "HTTPS<br/>cookie httpOnly" --> next
    next -- "HTTPS / JSON<br/>Bearer token" --> api

    api --> pg
    api --> storage
```

- O navegador nunca chama a API nem recebe o token: todo acesso passa pelo servidor Next.js
  (ADR-0008).
- O aplicativo trabalha sobre o SQLite e sincroniza com a API quando há conexão (ADR-0002 e
  ADR-0003).
- O banco guarda só metadados e a chave do arquivo de cada evidência; o arquivo fica no
  armazenamento de evidências (documento 11.8). No `backend` atual a implementação é
  `LocalEvidenceStorage` (disco local); armazenamento de objetos compatível com S3 é a
  recomendação do documento 11.8 para produção.
