# Learning Engine (Gestor de Estudio IA)

El Learning Engine es un submódulo de HyperBrain que actúa como un Gestor de Estudio impulsado por Inteligencia Artificial. Orquesta la ingesta, el mantenimiento y la evaluación del conocimiento técnico del usuario.

Se integra nativamente con el núcleo de productividad (Core), inyectando tareas de estudio directamente en la agenda diaria y utilizando métricas telemétricas para evitar la saturación cognitiva.

Base teórica: [Spaced Repetition y Active Recall](../theory/spaced-repetition.md).

---

## Arquitectura y Fases del Proceso

### Capa Estratégica: Gestión de Carga (Throttling)

Actúa como un **Gatekeeper** para asegurar que el ancho de banda cognitivo no colapse:

- **Estado Mix (Mantenimiento + Ingesta):** La prioridad absoluta es el repaso mediante Active Recall (Deuda). Si la carga de tareas de repaso es manejable (≤ 15), se permite la ingesta de nuevos temas (Expansión) hasta un tope de 20 ítems diarios.
- **Tsunami Protection:** Si los repasos pendientes superan el umbral crítico, el sistema bloquea automáticamente la entrada de nuevos temas.

### Capa Operativa: Ejecución Técnica (Workflows IA)

El iaService asume diferentes roles dependiendo de la madurez del usuario en el tema. Esto se orquesta mediante **Prompts Dinámicos**:

1. **Prompt A (Active Recall / Diagnostic):** Simula una entrevista Senior. Evalúa sin dar teoría inicial para medir la profundidad real del conocimiento (Internals, Arquitectura, Producción).
2. **Prompt B (Priming):** Si el diagnóstico arroja "Ignorancia Total" (Score < 20), el agente cambia a rol educativo. Proporciona un tutorial arquitectónico sin código.
3. **Prompt C (Surgical Study):** Resuelve brechas puntuales detectadas en el diagnóstico, explicando el "Under the Hood" a nivel de memoria, red o SO.
4. **Prompt D (Zettelkasten):** Consolida el aprendizaje creando una nota de alta densidad técnica (TL;DR, Internals, Anti-patrones, Vínculos en grafo).
5. **Prompt E (Regression Protocol / Backtrack N-1):** Si el usuario falla en un concepto avanzado por falta de bases, pausa el tema actual y obliga a repasar el concepto fundamental (N-1).

---

## Integración con HyperBrain Core

### Integración Relacional

- Los temas de estudio residen en la tabla `LRN_TOPIC`.
- El resultado de cada sesión (puntajes de la entrevista IA) se almacena en `LRN_ASSESSMENT`.
- El sistema crea un `CORE_EXECUTABLE` (una tarea en Notion/Apple Reminders) para que el usuario sepa que le toca estudiar un tema.

### Integración Orientada a Eventos (SQS)

1. El usuario completa la tarea de estudio en Notion/iOS.
2. `TaskCompletedEvent` llega a SQS.
3. El `EventRouter` detecta que el `core_executable` es de tipo `LEARNING_SESSION`.
4. Dispara el `iaService`, quien carga el historial del tema (`LRN_TOPIC`), decide qué Prompt usar basándose en la evaluación anterior (`LRN_ASSESSMENT`) y notifica al usuario para iniciar la sesión interactiva.

---

## Matriz de Casos de Borde (State Machine)

| Caso | Disparador (Score) | Acción (Transición de Estado) |
| :--- | :--- | :--- |
| **Ignorancia Total** | Score < 20 | Inyectar **Prompt B (Priming)**. |
| **Fragilidad** | Score < 70 | Inyectar **Prompt C (Estudio Quirúrgico)** enfocado en el feedback negativo. |
| **Estancamiento** | 3+ veces Frágil | Abandonar teoría. Transición a **Práctica Deliberada** (Código real / Unit Tests fallidos). |
| **Backtrack N-1** | Red Flag de conceptos | Inyectar **Prompt E** para reparar la base antes de continuar. |
| **Consolidado** | Score > 85 | Inyectar **Prompt D (Zettelkasten)** y posponer la próxima revisión usando Spaced Repetition. |
