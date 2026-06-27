# Agenda Planner Engine

Genera la agenda diaria del usuario distribuyendo las tareas priorizadas en bloques temporales basados en energía y disponibilidad.

## Fases de Ejecución

| Fase | Descripción |
| :--- | :--- |
| **Fase 1 — Input** | Lee disponibilidad horaria de iOS Calendar (excluyendo festivos y eventos bloqueados). |
| **Fase 2 — Asignación** | Clasifica tareas en bloques temporales (Mañana, Tarde, Noche) según el puntaje de **Effort**. |
| **Fase 3 — Output** | Escribe la agenda generada en Notion y sincroniza con Apple Reminders vía SQS. |

## Fórmula de Effort

```
Effort = ((T × 0.3) + (M × 0.3) + (E × 0.4) - 1) / 2.3 × 5
```

### Parámetros

| Parámetro | Escala | Descripción |
| :--- | :--- | :--- |
| **T** | 1–4 | Tiempo estimado en bloques. |
| **M** | 1–3 | Carga mental requerida. |
| **E** | 1–3 | Energía requerida. |

## Integración con Telemetría

El Planner consulta `tel_sleep_record` para obtener el Sleep Score de la noche anterior. Si la calidad del sueño es baja, el sistema reduce el tope de `energy_drain` disponible para el día, priorizando tareas de bajo esfuerzo.
