# Spaced Repetition y Active Recall

Fuentes: Hermann Ebbinghaus (Curva del Olvido), Piotr Wozniak (SuperMemo), Sebastian Leitner (Sistema Leitner).

---

## Resumen

### Spaced Repetition (Repetición Espaciada)

Técnica de estudio que programa las sesiones de repaso en intervalos crecientes, optimizando la retención a largo plazo. Se basa en la **Curva del Olvido** de Ebbinghaus: la memoria de un concepto decae exponencialmente con el tiempo si no se refuerza, pero cada repaso exitoso extiende el intervalo de retención.

### Active Recall (Recuperación Activa)

Técnica que consiste en intentar recuperar la información de la memoria sin consultar las fuentes, en lugar de releer pasivamente. La evidencia científica demuestra que el esfuerzo de recuperación fortalece las conexiones neuronales de forma más efectiva que la revisión pasiva.

---

## Implementación en HyperBrain (Learning Engine)

| Concepto | Implementación |
| :--- | :--- |
| **Curva del Olvido** | El campo `next_review_date` en `LRN_TOPIC` programa el próximo repaso. El intervalo crece tras cada evaluación exitosa. |
| **Active Recall** | El **Prompt A (Diagnostic)** del iaService evalúa al usuario sin proporcionar teoría, forzando la recuperación activa. |
| **Throttling Cognitivo** | Si la carga de repasos pendientes supera el umbral (15 ítems), el sistema bloquea la ingesta de nuevos temas para priorizar la consolidación. Previene el "Tsunami" de sesiones acumuladas. |
| **Diagnóstico de Regresión (N-1)** | Si el usuario falla en un concepto avanzado por falta de bases, el **Prompt E (Backtrack)** pausa el tema actual y obliga a dominar el concepto fundamental subyacente. |

---

## Dualidad del Proceso

| Tipo | Definición | Prioridad |
| :--- | :--- | :--- |
| **Mantenimiento (Deuda)** | Repaso de ítems existentes mediante Active Recall. | Absoluta. Siempre se ejecuta primero. |
| **Ingesta (Expansión)** | Adquisición de nuevos conceptos mediante Diagnóstico Inicial (intento de recuperación sin lectura previa). | Condicionada a que la carga de Mantenimiento esté bajo control. |
