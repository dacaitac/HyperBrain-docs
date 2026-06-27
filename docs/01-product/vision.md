# Visión del Producto

> **Versión:** 1.0
> **Última actualización:** 2026-06-27

---

## Declaración de Visión

HyperBrain es un **Ecosistema de Orquestación Productiva, Financiera y Cognitiva** que integra métricas de comportamiento continuo —transacciones financieras, telemetría de uso, progreso de aprendizaje y biometría— para alimentar un motor de inteligencia artificial. Su propósito es generar una agenda hiperoptimizada que maximice la eficiencia, proteja del colapso cognitivo y facilite la consecución de los objetivos del usuario.

El usuario interactúa activamente con el sistema mediante la definición de sus objetivos estratégicos, la creación de tareas y el registro de transacciones financieras, ya sea a través de las interfaces nativas (Notion, iOS Reminders) o de la GUI interna (Appsmith). La gestión activa se complementa con el **monitoreo de dashboards** alineados a las Disciplinas de la Ejecución (4DX), donde el usuario supervisa el avance de sus Metas Crucialmente Importantes (MCIs) y sus medidas predictivas (Lead Measures).

El motor de IA actúa como el **backend analítico**: automatiza la priorización de tareas, la generación de la agenda, la gestión de repasos de estudio y la detección de alertas presupuestarias. Las plataformas externas (Notion, Anytype, iOS) funcionan como su capa de presentación.

---

## Pilares Teóricos

El diseño del sistema es la implementación técnica de cinco marcos metodológicos:

| Marco | Aplicación en HyperBrain |
| :--- | :--- |
| **4DX** (The 4 Disciplines of Execution) | Enfoque en MCIs y Lead Measures. Los ciclos `CoreCycle` modelan las WIGs; los ejecutables con `type: LEAD_MEASURE` representan las acciones predictivas. Los dashboards de Appsmith reflejan el tablero de indicadores (Scoreboard). |
| **GTD** (Getting Things Done) | Captura absoluta mediante webhooks y GUI, clarificación por mapeo de impacto/energía, organización por el Agenda Planner Engine y reflexión estratégica a través de `CoreCycle`. |
| **Atomic Habits** | Diseño de entorno donde la base de datos es la fuente de verdad y las interfaces nativas (iOS) son el punto de ejecución con fricción mínima. Los hábitos se modelan como `CoreExecutable` con `type: HABIT`. |
| **Spaced Repetition y Active Recall** | El Learning Engine utiliza repetición espaciada para programar sesiones de repaso y Active Recall como método de diagnóstico. El Throttling cognitivo previene la saturación. Ver: [Motor de Aprendizaje](../02-architecture/engines/learning.md). |
| **Triángulo de Control (Finanzas Personales)** | Separación estricta entre la realidad física del dinero (Cuentas) y su propósito lógico (Metas). Control presupuestario mediante Categorías, Presupuestos dinámicos y Fondos de Amortización. Ver: [Motor de Finanzas](../02-architecture/engines/finance.md). |

---

## Diferenciación Competitiva

El problema principal de herramientas como Notion, Obsidian o Jira es que dependen exclusivamente de la entrada manual sin inteligencia que asista la planificación. HyperBrain resuelve esto operando como el **motor analítico subyacente**: el usuario define *qué* quiere lograr (objetivos, tareas, presupuestos) y el sistema decide *cómo* y *cuándo* ejecutarlo, integrándose con las plataformas que el usuario ya utiliza.

---

## Funcionalidades Críticas del MVP

1. **Generación de Agenda Dinámica.** Sistema de resolución instantánea que estructura las tareas del día con asistencia del motor de priorización y la telemetría del usuario.
2. **Sincronización de Ecosistema.** Puente de datos bidireccional entre el núcleo de HyperBrain y el software de terceros (Notion, iOS Reminders, WhatsApp).
3. **Integración Profunda de Base de Datos con IA.** Arquitectura que permite al agente de IA extraer contexto preciso y actualizado desde la base de datos relacional y vectorial.
4. **Gestión Financiera Personal.** Flujo de caja, presupuestos dinámicos por categoría, fondos de amortización (Sinking Funds) y trazabilidad de costos por proyecto. Soporte para partida doble simplificada y gestión de pasivos (tarjetas de crédito).
5. **Motor de Aprendizaje Inteligente (Study Manager).** Tutoría IA integrada nativamente al núcleo productivo, utilizando Spaced Repetition, Active Recall y Throttling cognitivo para orquestar el crecimiento técnico.

---

## Indicadores de Éxito (KPIs) del MVP

| KPI | Descripción |
| :--- | :--- |
| **Tasa de éxito en sincronización** | Flujo bidireccional con cero pérdida de información entre PostgreSQL, iOS (Recordatorios/Calendarios) y las interfaces de usuario (Notion o Anytype). |
| **Estabilidad operativa del core** | Ejecución continua y sin errores del flujo de trabajo principal a través de todos sus componentes. |
| **Retención en Alpha Cerrada** | Adopción activa por parte de un grupo de control inicial (entorno familiar cercano). |
| **Precisión presupuestaria** | Los presupuestos dinámicos reflejan la realidad del flujo de caja del usuario con desviación menor al 5%. |

---

## Usuarios y Roles

**Perfil del usuario final:** Personas enfocadas en su productividad — estudiantes, emprendedores, profesionales.

| Rol | Permisos |
| :--- | :--- |
| **Usuario Estándar** | Creación y gestión de objetivos, tareas, transacciones financieras y temas de estudio a través de interfaces de presentación (Notion, Anytype, iOS) o GUI interna (Appsmith). Monitoreo de dashboards 4DX. Interacción con la IA. |
| **Administrador del Sistema (SysAdmin)** | Superusuario con permisos para despliegue, gestión de infraestructura, monitoreo de rendimiento, configuración de integraciones y mantenimiento del entorno de ejecución. |

---

## Seguridad (MVP)

Para la fase inicial (MVP), los requerimientos de seguridad se limitan a la implementación de un sistema de autenticación estándar para el control de acceso. Al tratarse de un entorno controlado, se posterga el cifrado de extremo a extremo y el cumplimiento de normativas de privacidad para fases posteriores.

## Restricciones

- Uso preferente de software open source en todos los componentes del stack.
