# Task Prioritizer Engine

Calcula el **Priority Score (P)** para filtrar y ordenar las tareas del día.

## Fórmula

```
P = (Impacto × 0.4) + (Urgencia × 0.3) + (Effort_inv × 0.1) + (Alineación × 0.2)
```

### Parámetros

| Parámetro | Escala | Descripción |
| :--- | :--- | :--- |
| **Impacto** | Fibonacci (1–8) | Peso de la contribución de la tarea al objetivo estratégico. |
| **Urgencia** | 0–5+ | Escala de 0 (fecha de creación) a 5 (fecha límite). Escala más allá de 5 si la tarea está vencida. |
| **Effort_inv** | Inverso del Effort | Tareas con menor esfuerzo reciben mayor puntaje (Quick Wins). |
| **Alineación** | 0–1 | Grado de alineación con la MCI (Meta Crucialmente Importante) activa del ciclo. |

## Integración

- Almacena el resultado en `core_executable.priority_score`.
- Es invocado por el [Agenda Planner Engine](planner.md) durante la generación de agenda.
- Consulta la telemetría reciente para ajustar los multiplicadores de energía.
