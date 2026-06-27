
## 1. Objetivo Principal
Establecer la arquitectura de datos y la lógica de negocio para un sistema de gestión financiera personal que permita:
1.  Ahorrar para objetivos específicos y gestionar presupuestos globales (bolsas de proyectos).
2.  Controlar el flujo de caja mediante presupuestos dinámicos.
3.  Administrar pasivos (deudas y tarjetas de crédito) sin alterar la liquidez real.
4.  Integrarse con un sistema de gestión de proyectos y tareas, trazando el costo real de salidas, eventos e iniciativas a gran escala.

---

## 2. Fundamentos Teóricos

El sistema opera bajo la estricta separación de la realidad física del dinero y su propósito lógico, estructurado en lo que se denomina el **Triángulo de Control**.

### 2.1 Cuentas vs. Metas
* **Cuentas (La Realidad Física):** Representan el lugar donde reside el dinero o la deuda (ej. Cuenta de Ahorros, Efectivo, Tarjeta de Crédito). Responden a la pregunta: *¿Dónde está el dinero?* Son vitales para la conciliación bancaria.
* **Metas / Fondos de Amortización (El Propósito Lógico):** Son subdivisiones virtuales (bolsillos contables). Responden a la pregunta: *¿Para qué es ese dinero?* Permiten fraccionar un único saldo bancario en múltiples objetivos (viajes, compras) sin requerir cuentas bancarias separadas.

### 2.2 El Triángulo de Control
La arquitectura no acopla directamente el presupuesto a la meta, sino que cruza sus funciones mediante las transacciones:
1.  **Categorías:** Clasifican la naturaleza del gasto a corto plazo (ej. "Alimentación").
2.  **Presupuestos:** Ponen un límite de gasto a las categorías en un rango de fechas.
3.  **Metas:** Acumulan capital a largo plazo para un proyecto. Al ejecutar el gasto de una meta, este impacta su respectiva categoría, consumiendo presupuesto si aplica, pero financiándose del fondo acumulado.

---

## 3. Diccionario de Datos (Estructura Relacional 3NF)

La integración bidireccional se logra mediante llaves foráneas opcionales (`id_proyecto`, `id_tarea`) en el módulo financiero.

### Módulo de Gestión (Externo/Existente)
* **`Proyectos`**: Agrupadores macro.
    * `id_proyecto` (PK), `nombre` (Varchar), `estado` (Enum).
* **`Tareas`**: Eventos o ítems específicos.
    * `id_tarea` (PK), `id_proyecto` (FK nula), `nombre` (Varchar), `estado` (Enum), **`costo_estimado` (Decimal)**, `fecha_ejecucion` (Date).

### Módulo Financiero (Core)
* **`Cuentas`**: Contenedores físicos o legales.
    * `id_cuenta` (PK), `nombre` (Varchar), `tipo` (Enum: Activo, Pasivo), `saldo_actual` (Decimal).
* **`Categorias`**: Clasificación contable.
    * `id_categoria` (PK), `nombre` (Varchar), `tipo_flujo` (Enum: Ingreso, Gasto).
* **`Metas_Financieras`**: Fondos de amortización o bolsas generales.
    * `id_meta` (PK), `nombre` (Varchar), `monto_objetivo` (Decimal), `monto_acumulado` (Decimal), `id_proyecto` (FK nula), `estado` (Enum: En Ahorro, Fondeada, Cumplida).
* **`Presupuestos`**: Control temporal de gastos.
    * `id_presupuesto` (PK), **`fecha_inicio` (Date)**, **`fecha_fin` (Date)**, `id_categoria` (FK), `monto_limite` (Decimal).
* **`Transacciones`**: El libro mayor.
    * `id_transaccion` (PK), `fecha` (DateTime), `monto` (Decimal), `tipo` (Enum: Gasto, Ingreso, Transferencia), `estado` (Enum: Planificado, Completado), `id_cuenta_origen` (FK nula), `id_cuenta_destino` (FK nula), `id_categoria` (FK nula), `id_proyecto` (FK nula), `id_tarea` (FK nula), `descripcion` (Varchar).

---

## 4. Reglas de Negocio y Casos de Uso

### 4.1 Gestión de Bolsas Generales y Alcance Dinámico
Cuando se tiene un proyecto macro (ej. Equipar Hogar por 15M) pero no se han definido los ítems, se crea una Meta tipo "Bolsa General".
* **Regla:** El `monto_objetivo` de la Bolsa General es la suma dinámica del `costo_estimado` de todas las tareas pendientes asociadas a ese proyecto. Si se añade una tarea nueva, el objetivo global sube automáticamente. Al ejecutar un gasto para una tarea, el dinero sale del `monto_acumulado` de la bolsa.

### 4.2 Gestión de Tarjetas de Crédito y Deudas
Las tarjetas de crédito se modelan como cuentas de tipo **Pasivo**.
* **La Compra (Endeudamiento):** Se registra un `Gasto` desde la cuenta Pasivo. Esto afecta el presupuesto de la categoría y aumenta la deuda en la cuenta, pero no reduce el saldo de tus cuentas Activo.
* **El Pago de la Tarjeta:** Se registra como una `Transferencia` desde una cuenta Activo (ej. Cuenta Corriente) hacia la cuenta Pasivo (Tarjeta de Crédito). **No es un gasto**, es un traslado de dinero que neutraliza la deuda.

### 4.3 Presupuestos Dinámicos y Automatización
Los presupuestos operan mediante rangos de fechas (`fecha_inicio`, `fecha_fin`) para soportar ciclos no estandarizados (semanales, quincenales).
* **Automatización:** El sistema debe utilizar un *Cron Job* que lea una tabla de plantillas y genere automáticamente los registros de la tabla `Presupuestos` al iniciar cada ciclo, aplicando reglas de arrastre (acumular el sobrante) o reseteo (base cero) según la configuración.

---

## 5. Ejemplos de Evolución (Estados de Tiempo)

**Contexto:** Se planifica un proyecto de Equipamiento de Hogar (PRJ-003) estimado en 15M. Se cuenta con una Cuenta Corriente (Activo) y una Tarjeta de Crédito (Pasivo).

### Estado 1: Inicio y Provisión de Bolsa General
Se inyectan 4M a la bolsa del proyecto hogar sin definir aún qué comprar.

**Metas_Financieras:**
* `MET-001` (Bolsa General Hogar) -> Objetivo: 15M | Acumulado: 4M | Proyecto: PRJ-003

**Transacciones:**
* `TX-001` | Ingreso | +8M | Destino: Cta. Corriente | Salario
* `TX-002` | **Transf.** | 4M | Origen: Cta. Corriente | Destino: Bolsillo Hogar | Proyecto: PRJ-003 | *Nota: Fondeo de la bolsa.*

### Estado 2: Ejecución de Gasto a Crédito y Pago
Se compra un escritorio de 1.5M usando la Tarjeta de Crédito y se registra una salida casual de 100k. Días después, se paga la tarjeta con el dinero que estaba guardado en la bolsa del hogar.

**Transacciones:**
* `TX-003` | **Gasto** | 1.5M | Origen: Tarjeta Crédito (Pasivo) | Cat: Hogar | Proyecto: PRJ-003 | Tarea: TAR-104 | *Nota: Aumenta la deuda, ejecuta presupuesto, usa fondos del proyecto lógicamente.*
* `TX-004` | **Gasto** | 100k | Origen: Cta. Corriente (Activo) | Cat: Ocio | Tarea: TAR-105 | *Nota: Salida casual vinculada directamente a una tarea.*
* `TX-005` | **Transf.** | 1.5M | Origen: Bolsillo Hogar | Destino: Tarjeta Crédito | *Nota: Se usa el dinero ahorrado (provisión) para liquidar la deuda adquirida por la compra del escritorio.*

---

## 6. Resumen

El sistema está diseñado bajo el principio de partida doble simplificada y alta cohesión relacional. Aísla las entidades financieras mediante el control de **Cuentas** (para rastrear la ubicación del dinero y deudas) y **Metas** (para gestionar la acumulación de ahorros e intenciones de gasto). La integración con la gestión de tareas se efectúa de manera opcional a nivel transaccional. Esto asegura que operaciones complejas, como el uso de crédito para proyectos a gran escala o la modificación dinámica de estimaciones de costos, se resuelvan algorítmicamente mediante transferencias internas y sumatorias de llaves foráneas, garantizando en todo momento la integridad del patrimonio, la precisión del presupuesto y la trazabilidad del costo por proyecto.