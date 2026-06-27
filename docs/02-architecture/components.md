# Arquitectura de Componentes

Capas del sistema, protocolos de comunicación y flujo de eventos.

Para los diagramas visuales completos, consultar: [Diagramas (Eraser.io)](diagrams.md).

---

## Capa de Interfaces (UI)

Los siguientes sistemas actúan como frontends de HyperBrain:

| Interfaz | Protocolo de Comunicación |
| :--- | :--- |
| **Notion** | REST bidireccional ↔ Gateway |
| **iCloud** (Reminders / Calendar) | EventKit → Sentinel → Gateway (entrada); Gateway → eventAPI → EventKit (salida) |
| **Appsmith** | Acceso directo a PostgreSQL. Dashboards 4DX y paneles de gestión financiera. |
| **Messaging** (WhatsApp) | Webhook → Gateway |

---

## Capa de Gateway y Mensajería

- **Gateway (API Gateway):** Punto de entrada único. Recibe peticiones REST de Notion, Sentinel y Messaging. Envía comandos REST a eventAPI. Se comunica bidireccionalmente con SQS en formato JSON.
- **SQS (Amazon Simple Queue Service):** Cola de mensajes central. Conecta Gateway, Core, iaService y OutboxWorker. Incluye Dead Letter Queue (DLQ) para tolerancia a fallos.
- **OutboxWorker:** Consume la tabla `outbox_events` de PostgreSQL y publica los mensajes correspondientes en SQS (patrón Transactional Outbox).

---

## Capa Core (Lógica de Negocio)

Implementa un patrón Event-Driven interno:

```
SQS ──→ Consumer ──→ EventRouter ──┬──→ IEventHandler ──→ CoreRepo ──→ PostgreSQL
                                   │
                                   └──→ SyncServ ──→ CoreRepo ──→ PostgreSQL
                                            │
                                            └──→ Producer ──→ SQS
```

| Componente | Responsabilidad |
| :--- | :--- |
| **Consumer** | Deserializa mensajes de SQS y los entrega al `EventRouter`. |
| **EventRouter** | Despacha mensajes al handler correspondiente (`IEventHandler`) o al servicio de sincronización (`SyncServ`). |
| **IEventHandler** | Ejecuta la lógica de negocio del dominio y persiste a través de `CoreRepo`. |
| **SyncServ** | Gestiona la propagación bidireccional de cambios. Persiste vía `CoreRepo` y publica mensajes de respuesta a través de `Producer`. |
| **Producer** | Serializa y publica mensajes hacia SQS. |
| **CoreRepo** | Capa de acceso a datos contra PostgreSQL. |

---

## Capa de Inteligencia Artificial

| Componente | Responsabilidad |
| :--- | :--- |
| **iaService** | Servicio de IA que consume mensajes de SQS, consulta la capa vectorial (RAG) para contexto histórico y delega la ejecución a OpenClaw. |
| **OpenClaw** | Orquestador de workflows de IA. Enruta las solicitudes a OpenRouter. |
| **OpenRouter** | Pasarela de enrutamiento a múltiples LLMs, incluyendo Ollama-MLX para inferencia local. |
