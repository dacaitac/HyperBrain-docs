# Arquitectura

Esta sección documenta las decisiones arquitectónicas, el stack tecnológico, los motores de ejecución, el modelo de datos y las bases teóricas del proyecto HyperBrain.

## Contenido

| Documento | Descripción |
| :--- | :--- |
| [Stack Tecnológico](tech-stack.md) | Tecnologías seleccionadas y justificación de cada elección. |
| [Despliegue](deployment.md) | Topología de nodos y modelo híbrido Cloud/On-Premise. |
| [Componentes](components.md) | Capas del sistema, protocolos de comunicación y flujo de eventos. |
| [Modelo de Datos (ERD)](data-model.md) | Diagrama Entidad-Relación completo y especificación de dominios. |
| [Diagramas (Eraser.io)](diagrams.md) | Referencia visual de los diagramas fuente. |

### Motores de Ejecución

| Motor | Descripción |
| :--- | :--- |
| [Prioritizer Engine](engines/prioritizer.md) | Cálculo del Priority Score para filtrar y ordenar tareas. |
| [Agenda Planner Engine](engines/planner.md) | Generación de agenda por bloques energéticos. |
| [Learning Engine](engines/learning.md) | Gestor de estudio IA con Spaced Repetition y Throttling. |
| [Finance Engine](engines/finance.md) | Gestión de flujo de caja, presupuestos y fondos de amortización. |
| [Messaging Engine (EDA)](engines/messaging.md) | Arquitectura orientada a eventos con SQS. |

### Bases Teóricas

| Marco | Descripción |
| :--- | :--- |
| [4DX](theory/4dx.md) | Las 4 Disciplinas de la Ejecución. |
| [GTD](theory/gtd.md) | Getting Things Done. |
| [Spaced Repetition](theory/spaced-repetition.md) | Repetición Espaciada y Active Recall. |
| [Triángulo de Control](theory/triangle-of-control.md) | Fundamentos del modelo financiero. |
