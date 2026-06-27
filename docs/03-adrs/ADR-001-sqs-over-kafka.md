---
id: ADR-001
status: Aceptada
date: 2026-06-27
---

# ADR-001: SQS como Bus de Mensajería sobre Kafka

## Contexto

Durante la sesión JAD, se evaluaron dos opciones para la cola de mensajes del sistema:

- **Apache Kafka:** Bus de eventos distribuido con persistencia de logs, ideal para ecosistemas con múltiples consumidores y necesidades de replay.
- **Amazon SQS:** Cola de mensajes administrada con Dead Letter Queue (DLQ) nativa, menor complejidad operativa y modelo pull.

Los diagramas iniciales en Eraser.io mostraban Kafka como bus de mensajería. Sin embargo, al evaluar las necesidades reales del MVP, se identificó que:

1. No hay necesidad de replay de eventos (event sourcing) en el MVP.
2. No hay múltiples consumidores para el mismo mensaje.
3. La complejidad operativa de Kafka (ZooKeeper/KRaft, particiones, consumer groups) es excesiva para un despliegue on-premise con un solo usuario.
4. SQS puede emularse localmente con **LocalStack** o **ElasticMQ** para desarrollo.

## Decisión

Se adopta **Amazon SQS** (con DLQ) como bus de mensajería para el MVP.

## Consecuencias

- **Positivas:** Menor complejidad operativa, DLQ nativa para tolerancia a fallos, menor consumo de recursos en el servidor local.
- **Negativas:** Si en el futuro se requiere event sourcing o múltiples consumidores, será necesario migrar a Kafka. El diseño del `EventRouter` y los `IEventHandler` está desacoplado del transporte, facilitando esta migración.
- **Mitigación:** El patrón Transactional Outbox (`outbox_events`) es agnóstico al transporte. El `Producer` es la única pieza que conoce SQS directamente.
