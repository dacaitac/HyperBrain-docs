# Messaging Engine (Arquitectura Orientada a Eventos)

El Messaging Engine es la columna vertebral de comunicación asíncrona de HyperBrain. Utiliza **Amazon SQS** como bus de mensajes para desacoplar los componentes del sistema.

---

## Principios

- **SSoT:** La base de datos PostgreSQL local es la única fuente de verdad.
- **Flujo de Propagación:** Todo cambio en la BD (vía webhook o lógica interna) emite un mensaje a SQS. El módulo de Sync consume estos mensajes para replicar el estado en los satélites (Notion, Apple Reminders).
- **Loop Protection:** Se utiliza un atributo `source_system` en los mensajes de SQS para evitar rebotes — no propagar de vuelta al sistema que originó el cambio.
- **Checksum Validation:** La tabla `sync_mappings` almacena el `last_known_checksum` de cada entidad sincronizada para detectar cambios reales y evitar propagaciones redundantes.
- **Tolerancia a Fallos:** Los mensajes que fallan tras N reintentos se envían a una **Dead Letter Queue (DLQ)** en SQS para inspección y reprocesamiento manual.

---

## Catálogo de Eventos

| Evento (Message Type) | Origen | Lógica que Dispara (IEventHandler) | Impacto en PostgreSQL |
| :--- | :--- | :--- | :--- |
| `TaskCompletedEvent` | Sentinel (iOS) / Gateway (Notion) | Valida cierre de tarea. Si es `LEAD_MEASURE`, recalcula el Lag Measure asociado. | Actualiza estado en `core_executable`. Si tiene costo, puede instanciar un `fin_transaction` preliminar. |
| `FinanceTransactionLoggedEvent` | Integración bancaria / Manual | Cruza con los presupuestos activos. | Inserta en `fin_transaction`. Actualiza `accumulated_amount` en `fin_goal` o saldo en `fin_account`. |
| `TimeBlockFinishedEvent` | Core (Planner) | Evalúa tiempo real vs estimado. | Actualiza `actual_duration_minutes` en `core_time_block`. |
| `DailyAgendaRequestedEvent` | Cronjob (05:30 AM) / Manual | Invoca al Prioritizer y Planner Engines. | Crea instancias de `core_executable` tipo `AGENDA`. |
| `BiometricSleepLoggedEvent` | Apple Health / Webhook | Analiza calidad del sueño. | Inserta en `tel_sleep_record`. Ajusta tope de `energy_drain` del Agenda Planner. |
| `IdeaCapturedEvent` | WhatsApp Webhook | Llama al LLM para analizar semántica. | Inserta en `brain_idea`. Ingiere en la base vectorial (RAG). |
| `LearningSessionTriggeredEvent` | Sentinel / Core | Evalúa historial del tema, decide Prompt IA e inicia sesión. | Inserta en `LRN_ASSESSMENT`, actualiza `LRN_TOPIC`, reprograma repaso. |
| `BudgetExceededEvent` | Finance Engine | Emite alerta al usuario vía Appsmith/Messaging. | Marca el presupuesto como excedido. |

!!! note "Extensibilidad"
    Los nuevos casos de uso se agregan como lógica de dominio reactiva (`IEventHandler`) que escucha mensajes específicos desde SQS, sin modificar la infraestructura existente.
