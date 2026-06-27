# Diagramas de Arquitectura (Eraser.io)

Diagramas fuente generados y mantenidos en [Eraser.io](https://app.eraser.io/workspace/qufdoKZ8oNs9GJjafWv0).

> ✏️ **[Abrir y editar en Eraser](https://app.eraser.io/workspace/qufdoKZ8oNs9GJjafWv0)**

---

## Referencia de Diagramas

| Diagrama | Archivo Fuente | Descripción |
| :--- | :--- | :--- |
| Deployment | `Architecture-Deployment-1.eraserdiagram` | Topología de nodos, red Tailscale, servicios por máquina y conectividad iCloud. |
| Components | `Architecture-Components-2.eraserdiagram` | Componentes del sistema, protocolos de comunicación y flujos de datos. |
| Core | `Architecture-Core-3.eraserdiagram` | Arquitectura interna del módulo Core: Consumer, EventRouter, IEventHandler, SyncServ, Producer. |
| Modelo de Datos (ERD) | `Architecture-varchar-5.eraserdiagram` | Esquema original de la base de datos en Eraser. El ERD actualizado en Mermaid se encuentra en [Modelo de Datos](data-model.md). |

!!! warning "Diagramas Legacy"
    Los archivos `.eraserdiagram` reflejan el diseño inicial. Algunas decisiones (ej. Kafka → SQS) han sido actualizadas. El [ADR-001](../03-adrs/ADR-001-sqs-over-kafka.md) documenta los cambios. La fuente de verdad actualizada está en esta documentación MkDocs.
