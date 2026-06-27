# Agentes y Skills

Documentación de los agentes de IA del ecosistema HyperBrain y sus capacidades.

!!! info "Pendiente"
    Esta sección se poblará durante la fase de implementación del iaService y OpenClaw.

## Agentes Planificados

| Agente | Responsabilidad |
| :--- | :--- |
| **iaService** | Servicio central de IA. Consume mensajes de SQS, consulta RAG y PostgreSQL, y delega a OpenClaw. |
| **Prioritizer Agent** | Calcula el Priority Score de las tareas pendientes. |
| **Planner Agent** | Genera la agenda diaria basándose en disponibilidad y telemetría. |
| **Learning Tutor** | Evalúa al usuario en sesiones de estudio y selecciona el Prompt dinámico adecuado. |
| **Finance Analyst** | Cruza transacciones con presupuestos y emite alertas de desviación. |
