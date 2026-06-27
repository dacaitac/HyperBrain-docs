# Workflows

Flujos de secuencia y ciclos de automatización del sistema HyperBrain.

!!! info "Pendiente"
    Los diagramas de secuencia detallados se agregarán durante la fase de implementación.

## Workflows Principales

| Workflow | Descripción | Disparador |
| :--- | :--- | :--- |
| Generación de Agenda | Prioritizer → Planner → Sync | Cronjob 05:30 AM / Manual |
| Sincronización Bidireccional | Satélite → SQS → Core → Outbox → SQS → Satélite | Cambio en Notion/iOS/WhatsApp |
| Sesión de Estudio | TaskCompleted → iaService → LRN_ASSESSMENT → Reprogramación | Completar tarea de estudio |
| Alerta Presupuestaria | Transaction → Finance Engine → Budget Check → Alerta | Registro de transacción |
| Captura de Idea | WhatsApp → Gateway → iaService → brain_idea + RAG | Mensaje de WhatsApp |
