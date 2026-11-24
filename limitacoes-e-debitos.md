# Limitações conhecidas, débitos técnicos e riscos

Registro único do que o MVP do FieldOps não faz, dos débitos técnicos assumidos, dos defeitos conhecidos e dos riscos, com o plano para cada item. Atende o card [docs#6](https://github.com/FieldOps-Project/docs/issues/6) (PBI-071) e a seção 18.7 do [Definition of Done](notion/18-definicao-de-pronto-definition-of-done.md).

- **Última atualização:** 2026-11-24
- **Base verificada:** `backend` em `develop` (commit `89cac75`) e `app` em `develop` (commit `f00b9ac`)

## Como manter este documento

- Atualizar ao final de **cada sprint**, não apenas na sprint 8. Registrar a atualização no [histórico](#histórico-de-atualizações).
- Toda limitação, débito ou defeito encontrado em revisão de PR vem para cá; o comentário do PR não basta.
- Cada entrada usa um identificador fixo, que não é reaproveitado: `LIM-` para limitação de escopo, `DEB-` para débito técnico, `DEF-` para defeito e `RSK-` para risco.
- Quando um item for resolvido, ele não é apagado: mude o campo `Status` para `resolvido` e cite o PR.
- Itens marcados com **(a confirmar)** ainda precisam de validação da equipe.

### Formato de cada entrada

```markdown
### LIM-001 — Título curto
- Tipo: limitação de escopo | débito técnico | defeito | risco
- Origem: documento, seção, issue ou arquivo de onde veio
- Impacto: o que acontece para quem usa ou mantém o sistema
- Plano: o que será feito e quando
```

Defeitos têm também os campos `Severidade` e `Status`.

### Severidade de defeito

Escala da seção 18.7 do Definition of Done:

| Severidade | Definição | Conduta |
| --- | --- | --- |
| Crítico | Perda de dados, falha de segurança, impossibilidade de executar o fluxo principal ou duplicidade grave. | Bloqueia a entrega. |
| Alto | Funcionalidade P0 indisponível sem alternativa aceitável. | Corrigir antes da release. |
| Médio | Problema com alternativa conhecida, sem perda de dados. | Aceitável com registro e plano. |
| Baixo | Problema visual ou melhoria não essencial. | Permanece no backlog. |

Um defeito crítico sem plano de contenção registrado reprova o DoD do MVP.

## Resumo

| ID | Título | Tipo | Severidade | Status |
| --- | --- | --- | --- | --- |
| LIM-001 a LIM-014 | Recursos fora do escopo do MVP | limitação de escopo | — | aceito |
| DEB-001 | Teste de persistência exige Docker | débito técnico | — | aberto |
| DEB-002 | Backend sem plugin de lint | débito técnico | — | aberto |
| DEB-003 | `ignoreUnknown = false` não é aplicado | débito técnico | — | aberto |
| DEB-004 | Operação `UPSERT` fora do contrato de sincronização | débito técnico | — | aberto |
| DEB-005 | App sincroniza por rotas de recurso, não pelo `/sync/push` | débito técnico | — | aberto (a confirmar) |
| DEF-001 | Revisão permitida a qualquer SUPERVISOR | defeito | Alto (a confirmar) | aberto |
| DEF-002 | Tipo de evidência `PHOTO` não é validado | defeito | Médio | aberto |
| DEF-003 | Evidência com arquivo pendente conta para o envio da inspeção | defeito | Médio (a confirmar) | aberto |
| DEF-004 | Códigos de erro fora do padrão e divergentes do catálogo | defeito | Médio | aberto |
| RSK-001 | Colisão de chave no lock consultivo da sincronização | risco | — | aceito |

## 1. Limitações de escopo

Recursos explicitamente fora do MVP, conforme a seção 1.7 do documento [01 - Visão Geral](notion/01-visao-geral.md#17-fora-do-escopo-do-mvp). O motivo comum é que nenhum deles é obrigatório na primeira versão; o plano de cada um só existe onde o documento de origem diz. Se a seção 1.7 mudar, esta lista deve ser revista.

### LIM-001 — Resolução colaborativa de conflitos de sincronização
- Tipo: limitação de escopo
- Origem: documento 01, seção 1.7
- Impacto: conflito de versão é detectado e bloqueado, mas não é resolvido na interface
- Plano: tratamento completo previsto para P2

### LIM-002 — Aplicativo mobile exclusivo para clientes
- Tipo: limitação de escopo
- Origem: documento 01, seção 1.7
- Impacto: o cliente não tem app próprio
- Plano: sem previsão no MVP

### LIM-003 — Portal público
- Tipo: limitação de escopo
- Origem: documento 01, seção 1.7
- Impacto: não há acesso sem autenticação às informações do sistema
- Plano: sem previsão no MVP

### LIM-004 — Multiempresa com isolamento completo de dados
- Tipo: limitação de escopo
- Origem: documento 01, seção 1.7
- Impacto: o sistema atende uma única empresa por instalação
- Plano: sem previsão no MVP

### LIM-005 — Pagamento ou faturamento
- Tipo: limitação de escopo
- Origem: documento 01, seção 1.7
- Impacto: cobrança dos serviços fica fora do sistema
- Plano: sem previsão no MVP

### LIM-006 — Roteirização automática de equipes
- Tipo: limitação de escopo
- Origem: documento 01, seção 1.7
- Impacto: a ordem e a rota das visitas são definidas manualmente
- Plano: sem previsão no MVP

### LIM-007 — Transmissão contínua da localização do técnico
- Tipo: limitação de escopo
- Origem: documento 01, seção 1.7
- Impacto: a localização é registrada apenas nos eventos da inspeção, não em tempo real
- Plano: sem previsão no MVP

### LIM-008 — Chamadas de vídeo
- Tipo: limitação de escopo
- Origem: documento 01, seção 1.7
- Impacto: não há comunicação em vídeo entre técnico e supervisor pelo sistema
- Plano: sem previsão no MVP

### LIM-009 — Armazenamento de vídeos longos
- Tipo: limitação de escopo
- Origem: documento 01, seção 1.7
- Impacto: evidências se limitam aos tipos suportados (no MVP, foto)
- Plano: sem previsão no MVP

### LIM-010 — Assinatura eletrônica com validade jurídica
- Tipo: limitação de escopo
- Origem: documento 01, seção 1.7
- Impacto: a aprovação no sistema não substitui assinatura com validade legal
- Plano: sem previsão no MVP

### LIM-011 — Geração avançada de relatórios regulatórios
- Tipo: limitação de escopo
- Origem: documento 01, seção 1.7
- Impacto: relatórios ficam restritos ao que o backlog do MVP prevê
- Plano: sem previsão no MVP

### LIM-012 — Integração com ERPs
- Tipo: limitação de escopo
- Origem: documento 01, seção 1.7
- Impacto: dados de clientes e equipamentos são cadastrados no próprio sistema
- Plano: sem previsão no MVP

### LIM-013 — Inteligência artificial para diagnóstico automático
- Tipo: limitação de escopo
- Origem: documento 01, seção 1.7
- Impacto: a avaliação de conformidade é sempre feita por pessoas
- Plano: sem previsão no MVP

### LIM-014 — Integração com sensores IoT
- Tipo: limitação de escopo
- Origem: documento 01, seção 1.7
- Impacto: leituras de equipamentos são informadas manualmente
- Plano: sem previsão no MVP

## 2. Débitos técnicos

### DEB-001 — Teste de persistência exige Docker
- Tipo: débito técnico
- Origem: `backend`, `src/test/java/com/fieldops/inspection/infrastructure/persistence/InspectionPersistenceTest.java` (usa Testcontainers com PostgreSQL)
- Impacto: sem Docker disponível o teste não roda, e quem executa a suíte localmente ou num CI sem Docker não valida as migrações Flyway nem o mapeamento de persistência da inspeção
- Plano: garantir Docker no CI e documentar o requisito no README de execução do backend (backend#34) **(a confirmar)**

### DEB-002 — Backend sem plugin de lint
- Tipo: débito técnico
- Origem: `backend`, `pom.xml` não declara Checkstyle, Spotless, PMD nem SpotBugs
- Impacto: o item "Lint e verificação de tipos passando" do DoD é atendido apenas pela compilação; não há verificação automática de estilo
- Plano: a definir pela equipe

### DEB-003 — `@JsonIgnoreProperties(ignoreUnknown = false)` não é aplicado
- Tipo: débito técnico
- Origem: `backend`, DTOs de sincronização e de inspeção, por exemplo `sync/presentation/dto/SyncPushRequest.java:13`, `sync/presentation/dto/SyncOperationRequest.java:14` e `sync/application/SyncPayloads.java:17`
- Impacto: a anotação sugere que campos desconhecidos são rejeitados, mas o Spring Boot desliga `FAIL_ON_UNKNOWN_PROPERTIES` globalmente e `ignoreUnknown = false` não reativa essa regra. Campos desconhecidos são ignorados em silêncio, então um erro de digitação no cliente não gera erro
- Plano: decidir entre rejeitar campos desconhecidos de fato (configuração por DTO ou global, com teste) ou remover a anotação para não induzir a erro

### DEB-004 — Operação `UPSERT` fora do contrato de sincronização
- Tipo: débito técnico
- Origem: `backend`, `sync/application/SyncOperationProcessor.java:133` (exige `UPSERT` para `INSPECTION_RESPONSE`), `:161` e `:204` (aceitam `UPSERT` para evidência e não conformidade); documento [10 - Modelo de Dados](notion/10-modelo-de-dados.md) lista `operation_type` como `CREATE, UPDATE, DELETE_LOGICAL, TRANSITION ou UPLOAD`
- Impacto: o enum documentado não tem `UPSERT`, mas o backend o usa e o exemplo do documento [12 - API REST](notion/12-api-rest.md) também. Um cliente que siga só o enum do documento 10 (ou o contrato compartilhado de docs#2) não consegue sincronizar respostas
- Plano: alinhar documentos 10, 12 e o contrato compartilhado com o comportamento do backend, ou mudar o backend para o enum documentado

### DEB-005 — App sincroniza por rotas de recurso, não pelo `/sync/push`
- Tipo: débito técnico
- Origem: `app`, `src/infrastructure/api/mobile-inspections-api.ts:148` envia respostas para `/api/v1/inspections/{id}/responses/{responseId}`; não há chamada a `/sync/push` em `src/`
- Impacto: o lote de sincronização do backend (idempotência por `operationId`, dependências entre operações) não é usado pelo app, então as garantias descritas para a sincronização dependem de cada rota de recurso
- Plano: a definir pela equipe; migrar o app para o `/sync/push` ou documentar as rotas de recurso como caminho oficial **(a confirmar)**

## 3. Defeitos conhecidos

### DEF-001 — Revisão permitida a qualquer SUPERVISOR
- Tipo: defeito
- Severidade: Alto **(a confirmar: pode ser Crítico se tratado como falha de segurança, conforme 18.7)**
- Status: aberto
- Origem: `backend`, `auth/infrastructure/security/AuthorizationPolicies.java:20` (`INSPECTIONS_REVIEW = "hasRole('SUPERVISOR')"`); `inspection/application/InspectionReviewService.java` busca a inspeção só pelo id (linhas 86, 152 e 201), sem filtrar pelo supervisor
- Impacto: qualquer supervisor pode listar revisões, aprovar ou reprovar inspeções de outro supervisor. A leitura de não conformidades, por outro lado, já restringe ao supervisor da inspeção (`NonConformityService.java:183`)
- Plano: restringir a revisão ao supervisor da inspeção, com teste de autorização, antes da release

### DEF-002 — Tipo de evidência `PHOTO` não é validado
- Tipo: defeito
- Severidade: Médio
- Status: aberto
- Origem: `backend`, `inspection/presentation/dto/EvidenceMetadataRequest.java:19` declara `type` como `String` com apenas `@NotBlank @Size(max = 32)`
- Impacto: o enum `EvidenceType` do contrato só tem `PHOTO`, mas o backend aceita e grava qualquer texto como tipo de evidência
- Plano: validar o tipo contra o enum e devolver erro de validação para valores fora dele

### DEF-003 — Evidência com arquivo pendente conta para o envio da inspeção
- Tipo: defeito
- Severidade: Médio **(a confirmar)**
- Status: aberto
- Origem: `backend`, migração `V21__allow_pending_evidence_files.sql` (deixa `uploaded_at` nulo enquanto o arquivo não chega); `InspectionResponseService.java:163-168` e `NonConformityService.java:149-153` só verificam se existe evidência não excluída
- Impacto: uma inspeção pode ser enviada para revisão com evidência obrigatória cujo arquivo ainda não foi recebido, e o supervisor revisa sem ver a foto
- Plano: decidir se o envio deve exigir `uploaded_at` preenchido nas evidências obrigatórias, ou se a revisão deve bloquear até o arquivo chegar

### DEF-004 — Códigos de erro fora do padrão e divergentes do catálogo
- Tipo: defeito
- Severidade: Médio
- Status: aberto
- Origem: [backend#81](https://github.com/FieldOps-Project/backend/issues/81); `shared/domain/exception/ResourceNotFoundException.java:18-22`
- Impacto: a API devolve códigos como `INSPECTION SITE_NOT_FOUND` (com espaço) e `NONCONFORMITY_NOT_FOUND`, e seis códigos do catálogo inicial de docs#2 não existem com esse nome no backend. Clientes que tratam erro pelo `code` podem não reconhecer o erro
- Plano: corrigir a geração dos códigos e alinhar o catálogo, conforme backend#81

## 4. Riscos

### RSK-001 — Colisão de chave no lock consultivo da sincronização
- Tipo: risco
- Origem: `backend`, `sync/application/SyncOperationProcessor.java:280` calcula a chave de `pg_advisory_xact_lock` com XOR dos 64 bits altos e baixos do `operationId`
- Impacto: dois `operationId` diferentes podem gerar a mesma chave. A colisão é rara e só faz uma operação esperar a outra terminar; não causa perda nem duplicidade de dados
- Plano: aceito como está; revisar se aparecer espera anormal na sincronização

## Histórico de atualizações

| Data | Sprint | Alteração |
| --- | --- | --- |
| 2026-11-24 | a confirmar | Criação do registro com as limitações da seção 1.7 e os itens verificados no `backend` e no `app` em `develop` |
