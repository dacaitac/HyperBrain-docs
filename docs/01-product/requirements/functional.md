# Requerimientos Funcionales (RF)

Los Requerimientos Funcionales describen los comportamientos y funciones que el sistema HyperBrain debe realizar de manera obligatoria para cumplir con las necesidades del MVP, según lo definido en el [Backlog MVP](index.md).

---

## Dominio: Agenda y Productividad

| ID | Requerimiento Funcional | Historia Asociada |
| :--- | :--- | :--- |
| **RF-01** | El sistema debe permitir la creación, edición, categorización (Task, Habit, Lead Measure) y eliminación de `CoreExecutable`. | [HU-01](HU-01.md) |
| **RF-02** | El sistema debe calcular el *Priority Score* de cada tarea basándose en impacto, urgencia, esfuerzo y alineación. | [HU-01](HU-01.md) |
| **RF-03** | El sistema debe generar una agenda diaria agrupando las tareas pendientes en bloques energéticos (Mañana, Tarde, Noche). | [HU-01](HU-01.md) |
| **RF-04** | El sistema debe permitir recalcular y replanificar la agenda bajo demanda durante el día. | [HU-02](HU-02.md) |
| **RF-05** | El sistema debe recibir ideas en texto libre (vía webhook), analizarlas mediante NLP y persistirlas como `brain_idea`. | [HU-03](HU-03.md) |

## Dominio: Aprendizaje Continuo (Learning)

| ID | Requerimiento Funcional | Historia Asociada |
| :--- | :--- | :--- |
| **RF-06** | El sistema debe iniciar una sesión interactiva (simulando una entrevista) cuando el usuario marca un tema de estudio como completado. | [HU-04](HU-04.md) |
| **RF-07** | El sistema debe seleccionar dinámicamente un prompt IA (A, B, C, D o E) dependiendo del puntaje de la evaluación previa del tema. | [HU-04](HU-04.md) |
| **RF-08** | El sistema debe reprogramar la siguiente fecha de revisión de un tema aplicando algoritmos de *Spaced Repetition*. | [HU-04](HU-04.md) |
| **RF-09** | El sistema debe bloquear la ingesta de nuevos temas si la cantidad de repasos vencidos supera un umbral configurable (Tsunami Protection). | [HU-05](HU-05.md) |

## Dominio: Finanzas

| ID | Requerimiento Funcional | Historia Asociada |
| :--- | :--- | :--- |
| **RF-10** | El sistema debe permitir el registro de transacciones con esquema de partida doble (cuenta origen y cuenta destino). | [HU-06](HU-06.md) |
| **RF-11** | El sistema debe validar que las compras a crédito (pasivos) aumenten la deuda sin disminuir el saldo activo. | [HU-06](HU-06.md) |
| **RF-12** | El sistema debe cruzar los gastos registrados contra los presupuestos activos y emitir alertas si se excede el límite temporal (`limit_amount`). | [HU-07](HU-07.md) |
| **RF-13** | El sistema debe actualizar el `accumulated_amount` de las metas financieras al recibir transferencias. | [HU-08](HU-08.md) |
| **RF-14** | Las metas de tipo "Bolsa General" deben recalcular su objetivo automáticamente sumando el costo estimado de sus tareas vinculadas. | [HU-08](HU-08.md) |

## Dominio: Sincronización

| ID | Requerimiento Funcional | Historia Asociada |
| :--- | :--- | :--- |
| **RF-15** | El sistema debe consumir eventos desde iOS EventKit y propagarlos hacia la base de datos central PostgreSQL. | [HU-09](HU-09.md) |
| **RF-16** | El sistema debe emitir eventos hacia Notion (vía API REST) ante cambios en el estado de una tarea (`core_executable`). | [HU-10](HU-10.md) |
| **RF-17** | El sistema debe utilizar checksums (`last_known_checksum`) para evitar la re-sincronización redundante y prevenir loops infinitos. | [HU-09](HU-09.md), [HU-10](HU-10.md) |
| **RF-18** | Los mensajes de SQS que superen el límite de reintentos fallidos deben ser movidos a una Dead Letter Queue (DLQ). | [HU-11](HU-11.md) |
