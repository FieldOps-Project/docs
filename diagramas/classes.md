# Diagrama de classes

Derivado do documento [10 - Modelo de Dados](../notion/10-modelo-de-dados.md) (10.3 e 10.5 a
10.12). Mostra as entidades centrais, os atributos que identificam e relacionam cada uma e os
enums do [contrato compartilhado](../contrato-compartilhado.md). Campos de controle comuns a
quase todas as entidades (`createdAt`, `updatedAt`, `version`) foram omitidos para manter o
diagrama legível. Nomes de atributo em camelCase, como no JSON.

```mermaid
classDiagram
    direction LR

    class User {
        UUID id
        string name
        string email
        UserRole role
        UserStatus status
    }

    class Client {
        UUID id
        string name
        string legalName
        string document
        EntityStatus status
    }

    class InspectionSite {
        UUID id
        UUID clientId
        string name
        decimal latitude
        decimal longitude
        EntityStatus status
    }

    class Equipment {
        UUID id
        UUID siteId
        string name
        string qrCode
        EquipmentStatus status
    }

    class InspectionTemplate {
        UUID id
        string title
        string category
        TemplateStatus status
    }

    class InspectionTemplateVersion {
        UUID id
        UUID templateId
        int versionNumber
        datetime publishedAt
        boolean activeForNewInspections
    }

    class TemplateSection {
        UUID id
        UUID templateVersionId
        string title
        int displayOrder
    }

    class TemplateItem {
        UUID id
        UUID sectionId
        string code
        string title
        ResponseType responseType
        boolean required
        boolean observationRequiredOnFailure
        boolean evidenceRequiredOnFailure
        JSON optionsJson
        int displayOrder
    }

    class Inspection {
        UUID id
        UUID templateVersionId
        UUID clientId
        UUID siteId
        UUID equipmentId
        UUID technicianId
        UUID supervisorId
        string title
        Priority priority
        InspectionStatus status
        datetime scheduledFor
        datetime startedAtDevice
        datetime startedAtServer
        datetime completedAtDevice
        datetime submittedAtServer
    }

    class InspectionItemSnapshot {
        UUID id
        UUID inspectionId
        UUID sourceTemplateItemId
        string sectionTitle
        string itemTitle
        ResponseType responseType
        boolean required
        JSON rulesJson
        JSON optionsJson
        int itemOrder
    }

    class InspectionResponse {
        UUID id
        UUID inspectionId
        UUID inspectionItemId
        string valueText
        decimal valueNumber
        boolean valueBoolean
        date valueDate
        JSON valueJson
        Conformity conformity
        string observation
        datetime answeredAtDevice
        datetime answeredAtServer
    }

    class Evidence {
        UUID id
        UUID inspectionId
        UUID responseId
        UUID nonConformityId
        EvidenceType type
        string storageKey
        string mimeType
        long sizeBytes
        string checksum
        datetime capturedAtDevice
        datetime serverReceivedAt
    }

    class NonConformity {
        UUID id
        UUID inspectionId
        UUID inspectionItemId
        UUID responseId
        string title
        Severity severity
        NonConformityStatus status
        datetime createdAtDevice
        datetime serverReceivedAt
    }

    class InspectionReview {
        UUID id
        UUID inspectionId
        UUID reviewerId
        ReviewDecision decision
        string reason
        int reviewCycle
        datetime reviewedAt
    }

    class AuditEvent {
        UUID id
        UUID inspectionId
        UUID actorId
        string action
        string entityType
        UUID entityId
        datetime occurredAt
        datetime deviceOccurredAt
        string requestId
        string deviceId
    }

    Client "1" --> "N" InspectionSite
    InspectionSite "1" --> "0..N" Equipment

    InspectionTemplate "1" --> "N" InspectionTemplateVersion
    InspectionTemplateVersion "1" --> "N" TemplateSection
    TemplateSection "1" --> "N" TemplateItem

    InspectionTemplateVersion "1" --> "N" Inspection
    Client "1" --> "N" Inspection
    InspectionSite "1" --> "N" Inspection
    Equipment "1" --> "0..N" Inspection
    User "1" --> "N" Inspection : executa / agenda

    Inspection "1" *-- "N" InspectionItemSnapshot
    InspectionItemSnapshot "1" --> "0..1" InspectionResponse
    InspectionResponse "1" --> "0..N" Evidence

    Inspection "1" --> "0..N" NonConformity
    InspectionResponse "1" --> "0..N" NonConformity
    NonConformity "1" --> "0..N" Evidence

    Inspection "1" --> "0..N" InspectionReview
    User "1" --> "N" InspectionReview : revisa
    Inspection "1" --> "N" AuditEvent
    User "1" --> "N" AuditEvent : realiza
```

Notas:

- `InspectionItemSnapshot` é uma composição da inspeção: é criado junto com ela e não existe
  sem ela (ADR-0004).
- A versão com `versionNumber = 0` e `publishedAt` nulo é o rascunho do modelo
  ([ADR-001 do backend](https://github.com/FieldOps-Project/backend/blob/develop/docs/adr/ADR-001-draft-representation-strategy.md)).
- `InspectionResponse.inspectionItemId` aponta para o item do snapshot, não para o
  `TemplateItem`.
- O par `*AtDevice` / relógio do servidor segue a seção 4 do contrato compartilhado.
- O `backend` tem ainda `inspection_review_items` (itens a corrigir de cada reprovação), que
  não aparece no documento 10 e por isso não está no diagrama.
- As estruturas locais do aplicativo (`SyncOperation`, `SyncMetadata`, documento 10.13) não
  fazem parte do modelo central e ficam fora deste diagrama.
