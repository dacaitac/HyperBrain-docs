# Finance Engine (Motor de Gestión Financiera)

El Finance Engine es el módulo de HyperBrain responsable de la gestión patrimonial personal: flujo de caja, presupuestos dinámicos, fondos de amortización (Sinking Funds) y trazabilidad de costos por proyecto.

Se integra bidireccionalmente con el núcleo de productividad (Core), permitiendo vincular transacciones financieras a tareas y proyectos específicos.

Base teórica: [Triángulo de Control](../theory/triangle-of-control.md).

---

## Principio Arquitectónico

El sistema opera bajo la estricta separación de la **realidad física del dinero** y su **propósito lógico**, estructurado en el Triángulo de Control:

- **Cuentas (La Realidad Física):** Representan el lugar donde reside el dinero o la deuda (ej. Cuenta de Ahorros, Efectivo, Tarjeta de Crédito). Responden a la pregunta: *¿Dónde está el dinero?*
- **Metas / Fondos de Amortización (El Propósito Lógico):** Subdivisiones virtuales (bolsillos contables). Responden a la pregunta: *¿Para qué es ese dinero?*
- **Categorías + Presupuestos (El Control):** Clasifican la naturaleza del gasto y ponen límites temporales.

---

## Reglas de Negocio

### Gestión de Bolsas Generales y Alcance Dinámico

Cuando se tiene un proyecto macro pero no se han definido los ítems, se crea una Meta tipo "Bolsa General".

- El `target_amount` de la Bolsa General es la **suma dinámica** del `estimated_cost` de todas las tareas pendientes asociadas a ese proyecto.
- Si se añade una tarea nueva, el objetivo global sube automáticamente.
- Al ejecutar un gasto para una tarea, el dinero sale del `accumulated_amount` de la bolsa.

### Gestión de Tarjetas de Crédito y Deudas

Las tarjetas de crédito se modelan como cuentas de tipo **Pasivo**.

- **La Compra (Endeudamiento):** Se registra un `EXPENSE` desde la cuenta Pasivo. Afecta el presupuesto de la categoría y aumenta la deuda, pero **no reduce** el saldo de las cuentas Activo.
- **El Pago de la Tarjeta:** Se registra como una `TRANSFER` desde una cuenta Activo hacia la cuenta Pasivo. **No es un gasto**, es un traslado que neutraliza la deuda.

### Presupuestos Dinámicos

Los presupuestos operan mediante rangos de fechas (`start_date`, `end_date`) para soportar ciclos no estandarizados (semanales, quincenales, mensuales).

- **Automatización:** Un Cron Job lee una tabla de plantillas y genera automáticamente los registros de `FIN_BUDGET` al iniciar cada ciclo.
- **Políticas:** Arrastre (acumular el sobrante del ciclo anterior) o Reseteo (base cero).

---

## Integración Orientada a Eventos (SQS)

| Evento | Lógica que Dispara | Impacto en PostgreSQL |
| :--- | :--- | :--- |
| `FinanceTransactionLoggedEvent` | Cruza el evento financiero con los presupuestos activos. | Inserta en `fin_transaction`. Actualiza `accumulated_amount` en `fin_goal` o saldo en `fin_account`. Emite alerta si el presupuesto se excede. |
| `TaskCompletedEvent` (con costo) | Si la tarea tiene `estimated_cost`, puede instanciar un `fin_transaction` preliminar. | Actualiza estado en `core_executable` y crea transacción vinculada. |
| `BudgetCycleStartEvent` | Cron Job genera los nuevos presupuestos del ciclo. | Inserta registros en `fin_budget` basándose en plantillas configuradas. |

---

## Ejemplo de Flujo: Compra a Crédito de un Proyecto

1. Se fondea la bolsa del proyecto: `TRANSFER` de Cuenta Corriente → Bolsillo Hogar. (`accumulated_amount` sube.)
2. Se compra un escritorio con tarjeta: `EXPENSE` desde Tarjeta Crédito (Pasivo). (Categoría "Hogar" consume presupuesto, deuda sube, Activo no se toca.)
3. Se paga la tarjeta con fondos del proyecto: `TRANSFER` desde Bolsillo Hogar → Tarjeta Crédito. (`accumulated_amount` baja, deuda baja.)
