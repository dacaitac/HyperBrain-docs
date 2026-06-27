# GTD — Getting Things Done

Fuente: *Getting Things Done: The Art of Stress-Free Productivity* — David Allen.

---

## Resumen

GTD es una metodología de productividad personal basada en el principio de que la mente es mejor para procesar ideas que para almacenarlas. Su flujo de trabajo se basa en cinco fases:

### 1. Capturar

Recoger absolutamente todo lo que reclame atención en un sistema de confianza externo a la mente.

**Implementación en HyperBrain:** Los webhooks de WhatsApp, Sentinel (iOS) y el Gateway de Notion capturan ideas, tareas y eventos sin intervención manual. Todo se persiste en `brain_idea` o `core_executable`.

### 2. Clarificar

Procesar cada ítem capturado para determinar qué es y qué acción requiere (si la requiere).

**Implementación en HyperBrain:** El iaService analiza las ideas en bruto (`brain_idea`) y las clasifica mediante LLMs. Las ideas viables transicionan a `REFINING` → `CONVERTED` (vinculadas a un `core_executable`).

### 3. Organizar

Ubicar los ítems procesados en el contenedor adecuado: proyectos, listas de acciones, calendarios, referencia.

**Implementación en HyperBrain:** Los `CoreExecutable` se organizan jerárquicamente (parent/child) y se agrupan en `CoreCycle` (estrategia) y `CoreProject` (ejecución). El mapeo de impacto (`impact`) y energía (`energy_drain`) facilita la clasificación automática.

### 4. Reflexionar

Revisar periódicamente el sistema para mantener la confianza y el control.

**Implementación en HyperBrain:** Los ciclos estratégicos (`CoreCycle`) incluyen ventanas de reflexión. El dashboard de Appsmith permite la revisión semanal del estado de todas las MCIs y proyectos.

### 5. Ejecutar

Elegir qué hacer en cada momento con confianza, basándose en contexto, tiempo, energía y prioridad.

**Implementación en HyperBrain:** El Prioritizer Engine y el Planner Engine automatizan esta decisión, considerando la prioridad calculada, la disponibilidad horaria y la telemetría de energía del usuario.
