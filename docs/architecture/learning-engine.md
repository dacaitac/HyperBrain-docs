# Motor de Aprendizaje (Learning Engine)

El **Learning Engine** es un submódulo de HyperBrain que actúa como un Gestor de Estudio impulsado por Inteligencia Artificial. Su objetivo es orquestar la ingesta, el mantenimiento y la evaluación del conocimiento técnico del usuario (específicamente orientado a Ingeniería de Software y Arquitectura).

Al igual que el motor de finanzas, este módulo se integra nativamente con el núcleo de productividad (Core) de HyperBrain, inyectando tareas de estudio directamente en la agenda diaria y utilizando métricas telemetricas para evitar la saturación cognitiva.

## 1. Arquitectura y Fases del Proceso

El motor opera bajo un modelo de "Pipeline" que divide el aprendizaje en dos capas principales:

### Capa Estratégica: Gestión de Carga (Throttling)
Actúa como un **Gatekeeper** para asegurar que el ancho de banda cognitivo no colapse:
- **Estado Mix (Mantenimiento + Ingesta):** La prioridad absoluta es el repaso mediante *Active Recall* (Deuda). Si la carga de tareas de repaso es manejable (ej. $\le 15$), se permite la ingesta de nuevos temas (Expansión) hasta un tope (ej. 20 ítems diarios).
- **Tsunami Protection:** Si los repasos pendientes superan un umbral crítico, el sistema bloquea automáticamente la entrada de nuevos temas.

### Capa Operativa: Ejecución Técnica (Workflows IA)
El agente de Inteligencia Artificial (iaService) asume diferentes roles (Interviewer, Principal Engineer, Knowledge Architect) dependiendo de la madurez del usuario en el tema. Esto se orquesta mediante **Prompts Dinámicos**:

1. **Prompt A (Active Recall / Diagnostic):** Simula una entrevista Senior. Evalúa al usuario sin darle teoría inicial para medir la profundidad real del conocimiento (Internals, Arquitectura, Producción).
2. **Prompt B (Priming):** Si el diagnóstico arroja "Ignorancia Total", el agente cambia a rol educativo. Proporciona un tutorial arquitectónico sin código (para que el usuario lo escriba).
3. **Prompt C (Surgical Study):** Resuelve dudas muy puntuales ("brechas") detectadas en el diagnóstico, explicando el "Under the Hood" a nivel de memoria, red o SO.
4. **Prompt D (Zettelkasten):** Para consolidar el aprendizaje, el agente ayuda a crear una nota de alta densidad técnica (TL;DR, Internals, Anti-patrones, Vínculos en grafo).
5. **Prompt E (Regression Protocol / Backtrack N-1):** Si el usuario falla en un concepto avanzado por falta de bases, el agente "pausa" el tema actual y obliga a repasar el concepto fundamental (N-1) subyacente.

## 2. Integración con HyperBrain Core

El motor de aprendizaje no es una isla; es un "Plugin" nativo de la arquitectura HyperBrain.

### Integración Relacional (Base de Datos)
- Los temas de estudio residen en la tabla `LRN_TOPIC`.
- El resultado de cada sesión (los puntajes de la entrevista IA) se almacenan en `LRN_ASSESSMENT`.
- El sistema crea un `CORE_EXECUTABLE` (una tarea en Notion/Apple Reminders) para que el usuario sepa que le toca estudiar un tema. Cuando el usuario lo completa, se dispara el ciclo de evaluación.

### Integración Orientada a Eventos (Kafka)
1. El usuario completa la tarea de estudio en Notion/iOS.
2. `TaskCompletedEvent` llega a Kafka.
3. El `EventRouter` detecta que el `core_executable` es de tipo `LEARNING_SESSION`.
4. Dispara el `iaService`, quien carga el historial del tema (`LRN_TOPIC`), decide qué Prompt usar basándose en la evaluación anterior (`LRN_ASSESSMENT`) y notifica al usuario (vía Appsmith o Messaging) para iniciar la sesión interactiva.

## 3. Matriz de Casos de Borde (State Machine)

La IA toma decisiones algorítmicas de enrutamiento del aprendizaje:

| Caso | Disparador (Score) | Acción (Transición de Estado) |
| :--- | :--- | :--- |
| **Ignorancia Total** | Score < 20 | Inyectar **Prompt B (Priming)**. |
| **Fragilidad** | Score < 70 | Inyectar **Prompt C (Estudio Quirúrgico)** enfocado en el feedback negativo. |
| **Estancamiento** | 3+ veces Frágil | Abandonar teoría. Transición a **Práctica Deliberada** (Código real / Unit Tests fallidos). |
| **Backtrack N-1** | Red Flag de conceptos | Inyectar **Prompt E** para reparar la base antes de continuar. |
| **Consolidado** | Score > 85 | Inyectar **Prompt D (Zettelkasten)** y posponer la próxima revisión usando Spaced Repetition. |

Esta integración convierte a HyperBrain no solo en un gestor de tiempo y dinero, sino en un tutor técnico personalizado de nivel Senior/Staff.
