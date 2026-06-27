# 4DX — Las 4 Disciplinas de la Ejecución

Fuente: *The 4 Disciplines of Execution* — Chris McChesney, Sean Covey, Jim Huling.

---

## Resumen

4DX es un marco de ejecución estratégica diseñado para lograr resultados extraordinarios en medio del torbellino operativo del día a día. Se basa en cuatro disciplinas:

### Disciplina 1: Enfocarse en lo Crucialmente Importante (WIG)

Seleccionar un número reducido de Metas Crucialmente Importantes (MCIs / WIGs) en lugar de dispersar esfuerzos en decenas de objetivos menores.

**Implementación en HyperBrain:** Los `CoreCycle` con `type: MCI` modelan las WIGs. El dashboard de Appsmith muestra el progreso en tiempo real.

### Disciplina 2: Actuar sobre las Medidas de Predicción (Lead Measures)

Identificar y ejecutar las actividades predictivas que tienen el mayor impacto en la consecución de la WIG, en lugar de limitarse a observar los resultados (Lag Measures).

**Implementación en HyperBrain:** Los `CoreExecutable` con `type: LEAD_MEASURE` son las acciones predictivas. El `TaskCompletedEvent` recalcula el Lag Measure asociado al completarse.

### Disciplina 3: Llevar un Tablero de Indicadores Motivador (Scoreboard)

Un tablero visible y comprensible que muestre si se está ganando o perdiendo. Debe ser mantenido por el equipo (o usuario).

**Implementación en HyperBrain:** El dashboard de Appsmith actúa como el Scoreboard. El usuario monitorea activamente el progreso de sus MCIs, Lead Measures y Lag Measures.

### Disciplina 4: Crear una Cadencia de Rendición de Cuentas

Sesiones regulares donde se rinde cuentas sobre los compromisos asumidos. Cada sesión termina con nuevos compromisos para la siguiente.

**Implementación en HyperBrain:** Los ciclos estratégicos (`CoreCycle`) tienen fechas de inicio y fin. La agenda diaria generada por el Planner Engine actúa como la sesión de compromiso automatizada.
