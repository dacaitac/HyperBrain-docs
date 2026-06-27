# Triángulo de Control — Fundamentos del Modelo Financiero

Fuente: Documento de diseño interno *"Arquitectura y Modelado de Sistema de Finanzas Personales"*.

---

## Resumen

El modelo financiero de HyperBrain opera bajo el principio de **separación estricta entre la realidad física del dinero y su propósito lógico**. Esta separación se estructura en tres vértices que se cruzan únicamente a través de las transacciones:

### 1. Cuentas (La Realidad Física)

Representan el lugar donde reside el dinero o la deuda.

- **Activos:** Cuentas de ahorro, efectivo, cuentas corrientes.
- **Pasivos:** Tarjetas de crédito, préstamos.
- Responden a la pregunta: *¿Dónde está el dinero?*
- Son vitales para la conciliación bancaria.

### 2. Metas / Fondos de Amortización (El Propósito Lógico)

Subdivisiones virtuales (bolsillos contables) del dinero disponible.

- Responden a la pregunta: *¿Para qué es ese dinero?*
- Permiten fraccionar un único saldo bancario en múltiples objetivos (viajes, compras, emergencias) sin requerir cuentas bancarias separadas.
- Pueden vincularse a un `CoreProject` para calcular dinámicamente el objetivo basado en los costos estimados de las tareas del proyecto.

### 3. Categorías + Presupuestos (El Control)

- **Categorías:** Clasifican la naturaleza del gasto a corto plazo (ej. "Alimentación", "Transporte").
- **Presupuestos:** Ponen un límite de gasto a las categorías en un rango de fechas flexible.

---

## Interacción entre los Vértices

La arquitectura **no acopla directamente** el presupuesto a la meta. Las funciones se cruzan mediante las transacciones:

1. Al ejecutar el gasto de una meta, este impacta su respectiva **categoría** (consumiendo presupuesto si aplica).
2. El gasto se **financia** del fondo acumulado en la meta.
3. El resultado neto se refleja en el saldo de la **cuenta** correspondiente.

---

## Regla Clave: Partida Doble Simplificada

| Operación | Tipo en Sistema | Efecto en Activo | Efecto en Pasivo | Efecto en Presupuesto |
| :--- | :--- | :--- | :--- | :--- |
| Compra con cuenta corriente | `EXPENSE` | Reduce saldo | — | Consume categoría |
| Compra con tarjeta de crédito | `EXPENSE` | **Sin efecto** | Aumenta deuda | Consume categoría |
| Pago de tarjeta | `TRANSFER` | Reduce saldo | Reduce deuda | **Sin efecto** |
| Fondeo de meta | `TRANSFER` | Traslado interno | — | **Sin efecto** |
