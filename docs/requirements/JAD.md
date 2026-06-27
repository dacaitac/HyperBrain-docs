# Sesión JAD — HyperBrain

> **Versión:** 1.0  
> **Fecha:** 2026-06-26  
> **Facilitador:** JAD Facilitator (AI-Assisted)  
> **Stakeholder:** Daniel Caita  

---

## 1. Propósito del Documento

Este documento registra los resultados de la sesión de Diseño Conjunto de Aplicaciones (JAD) para el proyecto **HyperBrain**. Su objetivo es consolidar la visión, los requerimientos funcionales y no funcionales, la arquitectura de referencia y el backlog inicial del MVP en un único artefacto de consulta permanente.

Las fuentes primarias de esta sesión son:

- Las respuestas del stakeholder durante la entrevista estructurada (Fase 1).
- Los diagramas de arquitectura publicados en Eraser.io (`Deployment`, `Components`, `Core`, `Cloud Architecture`, `Modelo de Datos`).
- La documentación existente del ecosistema: `CORE-ARCHITECTURE.md`, `DATA-MODEL.md`, `HYPERBRAIN-PRODUCTION-ROADMAP.md`, `README.md`, `README-AI-SYSTEM.md`, `4DX.md`, `GTD.md`.

---

## 2. Visión del Producto

### 2.1. Declaración de Visión

HyperBrain es un **Ecosistema de Orquestación Productiva, Financiera y Cognitiva (Gestión de Estudio)** que integra métricas de comportamiento continuo —transacciones financieras, telemetría de uso, progreso de aprendizaje y biometría— para alimentar un motor de inteligencia artificial. Su propósito es generar una agenda hiperoptimizada que maximice la eficiencia, proteja del colapso cognitivo (Throttling) y facilite la consecución de los objetivos del usuario mediante un Exoesqueleto Cognitivo.

El sistema opera como un **Exoesqueleto Cognitivo**: no depende de la entrada manual de datos ni de la gestión activa por parte del usuario. Funciona como el motor analítico subyacente (backend) que automatiza la recolección pasiva de datos y la toma de decisiones, integrándose con plataformas tradicionales (Notion, Anytype, iOS) para que estas funcionen exclusivamente como su capa de presentación (frontend).

### 2.2. Pilares Teóricos

El diseño del sistema es la implementación técnica de tres marcos metodológicos:

| Marco | Aplicación en HyperBrain |
| :--- | :--- |
| **4DX** (The 4 Disciplines of Execution) | Enfoque en MCIs (Metas Crucialmente Importantes) y Lead Measures. Los ciclos `CoreCycle` modelan las WIGs; los ejecutables `CoreExecutable` con `type: LEAD_MEASURE` representan las acciones predictivas. |
| **GTD** (Getting Things Done) | Captura absoluta mediante webhooks, clarificación por mapeo de impacto/energía, organización por el Agenda Planner Engine y reflexión estratégica a través de `CoreCycle`. |
| **Atomic Habits** | Diseño de entorno donde la base de datos es la fuente de verdad y las interfaces nativas (iOS) son el punto de ejecución con fricción mínima. |

### 2.3. Diferenciación Competitiva

El problema principal de herramientas como Notion, Obsidian o Jira es que dependen exclusivamente de la entrada manual de datos y la gestión activa por parte del usuario. HyperBrain elimina esta fricción operando como el motor analítico subyacente: automatiza la recolección pasiva de datos y la toma de decisiones, pudiendo integrarse con estas plataformas para que funcionen únicamente como su interfaz visual.

---

## 3. Entrevista Estructurada — Fase 1

### 3.1. Usuarios y Roles

**Perfil del usuario final:** Personas enfocadas en su productividad — estudiantes, emprendedores, profesionales.

**Roles del sistema:**

| Rol | Permisos |
| :--- | :--- |
| **Usuario Estándar** | Consumo del sistema a través de las interfaces de presentación (Notion, Anytype, iOS). Interacción con la IA para consultas y ajustes de agenda. |
| **Administrador del Sistema (SysAdmin)** | Superusuario con permisos exclusivos para despliegue y gestión de infraestructura técnica, monitoreo de rendimiento de base de datos, configuración de integraciones globales y mantenimiento del entorno de ejecución. |

### 3.2. Funcionalidades Críticas para el MVP

El stakeholder delimitó el alcance del MVP a tres pilares fundamentales:

1. **Generación de Agenda Dinámica.** Un sistema de resolución instantánea que estructure las tareas del día con fricción operativa casi nula por parte del usuario.
2. **Sincronización de Ecosistema.** Integración fluida que actúe como puente de datos bidireccional entre el núcleo de HyperBrain y el software de terceros utilizado cotidianamente.
3. **Integración Profunda de Base de Datos con IA.** Una arquitectura que permita al agente de inteligencia artificial extraer contexto preciso y actualizado directamente desde la base de datos relacional para personalizar su gestión.
4. **Motor de Aprendizaje Inteligente (Study Manager).** Un sistema de tutoría IA integrado nativamente al núcleo productivo, utilizando Spaced Repetition, Active Recall y Throttling cognitivo para orquestar el crecimiento técnico sin saturar la agenda.

### 3.3. Gestión del Conocimiento

La gestión del conocimiento utiliza una arquitectura de doble capa alimentada por eventos:

- **Capa Relacional (PostgreSQL):** Centraliza y estructura el modelo de dominio principal — gestión financiera, productividad, usuarios.
- **Capa Vectorial (RAG):** Ingiere el historial de interacciones en lenguaje natural (vía mensajería) y metadatos de eventos, otorgando a la inteligencia artificial la capacidad de recuperar contexto histórico y no estructurado bajo demanda.

### 3.4. Indicadores de Éxito (KPIs) del MVP

| KPI | Descripción |
| :--- | :--- |
| **Tasa de éxito en sincronización** | Flujo de datos bidireccional con cero pérdida de información entre PostgreSQL, el ecosistema nativo de iOS (Recordatorios/Calendarios) y las interfaces de usuario (Notion o Anytype). |
| **Estabilidad operativa del core (Happy Path)** | Ejecución continua y sin errores del flujo de trabajo principal a través de todos sus componentes. |
| **Retención en Alpha Cerrada** | Adopción activa y constante por parte de un grupo de control inicial (entorno familiar cercano), previo a una expansión gradual. |

### 3.5. Seguridad

Para la fase inicial de despliegue (MVP), los requerimientos de seguridad se limitan a la implementación de un sistema de autenticación estándar para el control de acceso de los usuarios. Al tratarse de un entorno controlado, se posterga la integración de cifrado de extremo a extremo y el cumplimiento de normativas específicas de privacidad para fases posteriores de escalabilidad.

### 3.6. Restricciones y Preferencias

- Uso preferente de software open source en todos los componentes del stack.

---

## 4. Stack Tecnológico

Definido por las respuestas del stakeholder y validado contra los diagramas de despliegue (`Deployment`) y componentes (`Components`).

| Dominio | Tecnología | Justificación |
| :--- | :--- | :--- |
| **Core y Backend** | Spring Boot (Java) | Solidez y escalabilidad para la lógica de negocio. Arquitectura modular DDD: `core`, `sync`, `finance`, `cognitive`, `prioritizer`, `planner`. |
| **Persistencia Relacional** | PostgreSQL + Supabase | PostgreSQL como SSoT (Single Source of Truth). Supabase para aceleración de acceso a datos y autenticación. |
| **Persistencia Vectorial** | RAG (Base de datos vectorial) | Almacenamiento de embeddings de interacciones NLP y metadatos contextuales. |
| **Mensajería y Eventos** | Apache Kafka | Bus de eventos asíncrono para el patrón Event-Driven Architecture. Identificado en los diagramas `Deployment` y `Core` como eje de comunicación entre componentes. |
| **Contenerización** | Docker | Empaquetado de todos los servicios del ecosistema. |
| **Orquestación** | Kubernetes (K8s) | Escalado y gestión de ciclo de vida de contenedores. |
| **Automatización de Workflows** | n8n | Integración de flujos de trabajo de terceros (low-code). |
| **Paneles Internos** | Appsmith | Desarrollo rápido de herramientas internas y paneles de control (low-code). Acceso directo a PostgreSQL según diagrama `Deployment`. |
| **IA — Enrutamiento LLM** | OpenRouter | Pasarela de enrutamiento unificada para múltiples proveedores de modelos de lenguaje. |
| **IA — Agentes/Workflows** | OpenClaw | Orquestación de tareas de IA pesadas. Consume OpenRouter según diagrama `Deployment`. |
| **IA — Ejecución Local** | Ollama-MLX | Ejecución de modelos de IA en hardware local (Mac Mini M4 Pro). Conectado a OpenRouter según diagrama `Deployment`. |
| **Ecosistema Apple** | Swift (EventKit, Vapor) | **Sentinel:** Demonio macOS para monitoreo de cambios nativos en iCloud (Reminders/Calendar). **eventAPI:** Servicio REST para escritura en EventKit. |
| **Red Privada** | Tailscale | VPN mesh que interconecta todos los nodos de la infraestructura distribuida. |

!!! note "Discrepancia resuelta: SQS vs. Kafka"
    Las respuestas de la Fase 1 mencionan Amazon SQS con DLQ. Sin embargo, los diagramas Eraser (`Deployment`, `Components`, `Core`) muestran consistentemente **Apache Kafka** como bus de mensajería. Se prioriza Kafka como la tecnología seleccionada, alineándose con los diagramas de arquitectura que representan el diseño más reciente. La tolerancia a fallos (equivalente a DLQ) se implementará mediante los mecanismos nativos de Kafka (dead letter topics, retry topics).

---

## 5. Arquitectura de Despliegue

Modelo híbrido distribuido (Cloud / On-Premise) compuesto por tres nodos interconectados mediante Tailscale, según el diagrama `Deployment`.

### 5.1. Topología de Nodos

```
┌──────────────────────────────────────────────────────────────────────┐
│                        RED TAILSCALE (VPN Mesh)                      │
│                                                                      │
│  ┌─────────────────┐  ┌──────────────────────────────────────────┐  │
│  │   Mac Mini M4   │  │         daniel-ubuntu (Servidor)         │  │
│  │   (On-Premise)  │  │                                          │  │
│  │                 │  │  ┌────────────────────────────────────┐  │  │
│  │  • Sentinel     │  │  │        HyperBrain (Docker/K8s)    │  │  │
│  │  • eventAPI     │  │  │                                    │  │  │
│  │  • Ollama-MLX   │  │  │  • PostgreSQL    • OpenRouter     │  │  │
│  │                 │  │  │  • RAG           • OpenClaw        │  │  │
│  └────────┬────────┘  │  │  • Backend       • Kafka           │  │  │
│           │           │  │  • Appsmith                        │  │  │
│  ┌────────┴────────┐  │  └────────────────────────────────────┘  │  │
│  │  Apple Device   │  │                                          │  │
│  │  (iOS/macOS)    │  └──────────────────────────────────────────┘  │
│  └─────────────────┘                                                │
└──────────────────────────────────────────────────────────────────────┘
                              │
              ┌───────────────┴───────────────┐
              │      Control Plane (OCI)       │
              │  • Orquestación Kubernetes     │
              │  • Observabilidad (Grafana)    │
              └───────────────────────────────┘

              ┌───────────────────────────────┐
              │         iCloud (Apple)         │
              │  • Reminders  • Calendar       │
              └───────────────────────────────┘

              ┌───────────────────────────────┐
              │      Messaging (Webhook)       │
              │  • WhatsApp → Backend          │
              └───────────────────────────────┘
```

### 5.2. Descripción de Nodos

| Nodo | Ubicación | Función |
| :--- | :--- | :--- |
| **daniel-ubuntu** | Servidor local | Aloja el core de HyperBrain contenerizado: Backend Spring Boot, PostgreSQL, Kafka, RAG, Appsmith, OpenRouter, OpenClaw. |
| **Mac Mini M4 Pro** | On-premise | Ejecuta los servicios nativos de Apple: **Sentinel** (demonio de monitoreo iCloud vía EventKit → Kafka), **eventAPI** (escritura REST → iCloud) y **Ollama-MLX** (inferencia local de modelos de IA). |
| **Control Plane (OCI)** | Oracle Cloud Infrastructure | Orquestación de Kubernetes y observabilidad integral mediante Grafana. |
| **Apple Device** | Dispositivo del usuario | Punto de interacción del usuario. Accede a Appsmith (WebUI) y sincroniza bidireccionalmente con iCloud. |

---

## 6. Arquitectura de Componentes

Derivada del diagrama `Components`. Define las capas del sistema y los protocolos de comunicación entre componentes.

### 6.1. Capa de Interfaces (UI)

Los siguientes sistemas actúan exclusivamente como frontends de HyperBrain:

| Interfaz | Protocolo de Comunicación |
| :--- | :--- |
| **Notion** | REST bidireccional ↔ Gateway |
| **iCloud** (Reminders / Calendar) | EventKit → Sentinel → Gateway (entrada); Gateway → eventAPI → EventKit (salida) |
| **Appsmith** | Acceso directo a PostgreSQL |
| **Messaging** (WhatsApp) | Webhook → Gateway |

### 6.2. Capa de Gateway y Mensajería

- **Gateway (API Gateway):** Punto de entrada único. Recibe peticiones REST de Notion, Sentinel y Messaging. Envía comandos REST a eventAPI. Se comunica bidireccionalmente con la cola SQS/Kafka en formato JSON.
- **SQS / Kafka:** Bus de eventos central. Conecta Gateway, Core, iaService y OutboxWorker.
- **OutboxWorker:** Consume la tabla `outbox_events` de PostgreSQL y publica los eventos correspondientes en la cola de mensajería (patrón Transactional Outbox).

### 6.3. Capa Core (Lógica de Negocio)

Extraída del diagrama `Core`. Implementa un patrón Event-Driven interno:

```
SQS/Kafka ──→ Consumer ──→ EventRouter ──┬──→ IEventHandler ──→ CoreRepo ──→ PostgreSQL
                                         │
                                         └──→ SyncServ ──→ CoreRepo ──→ PostgreSQL
                                                  │
                                                  └──→ Producer ──→ SQS/Kafka
```

| Componente | Responsabilidad |
| :--- | :--- |
| **Consumer** | Deserializa eventos de la cola y los entrega al `EventRouter`. |
| **EventRouter** | Despacha eventos al handler correspondiente (`IEventHandler`) o al servicio de sincronización (`SyncServ`). |
| **IEventHandler** | Ejecuta la lógica de negocio del dominio y persiste a través de `CoreRepo`. |
| **SyncServ** | Gestiona la propagación bidireccional de cambios. Persiste vía `CoreRepo` y publica eventos de respuesta a través de `Producer`. |
| **Producer** | Serializa y publica eventos hacia la cola. |
| **CoreRepo** | Capa de acceso a datos contra PostgreSQL. |

### 6.4. Capa de Inteligencia Artificial

| Componente | Responsabilidad |
| :--- | :--- |
| **iaService** | Servicio de IA que consume eventos de la cola (SQS/Kafka), consulta la capa vectorial (RAG) para contexto histórico y delega la ejecución a OpenClaw. |
| **OpenClaw** | Orquestador de workflows de IA. Enruta las solicitudes a OpenRouter. |
| **OpenRouter** | Pasarela de enrutamiento a múltiples LLMs, incluyendo Ollama-MLX para inferencia local. |

---

## 7. Motores de Ejecución

Definidos en `CORE-ARCHITECTURE.md`. Constituyen la lógica de negocio diferencial del sistema.

### 7.1. Task Prioritizer Engine

Calcula el **Priority Score (P)** para filtrar y ordenar las tareas del día:

$$P = (Impacto \times 0.4) + (Urgencia \times 0.3) + (Effort_{inv} \times 0.1) + (Alineación \times 0.2)$$

- **Urgencia:** Escala de 0 (fecha de creación) a 5 (fecha límite). Escala más allá de 5 si la tarea está vencida.
- **Impacto:** Escala Fibonacci (1–8).

### 7.2. Agenda Planner Engine

| Fase | Descripción |
| :--- | :--- |
| **Fase 1 — Input** | Lee disponibilidad horaria de iOS Calendar (excluyendo festivos). |
| **Fase 2 — Asignación** | Clasifica tareas en bloques temporales (Mañana, Tarde, Noche) según el puntaje de **Effort**. |
| **Fase 3 — Output** | Escribe la agenda generada en Notion y sincroniza con Apple Reminders. |

Fórmula de Effort:

$$Effort = \left( \frac{(T \times 0.3) + (M \times 0.3) + (E \times 0.4) - 1}{2.3} \right) \times 5$$

Donde: T = Tiempo estimado (1–4), M = Carga mental (1–3), E = Energía requerida (1–3).

### 7.3. Kafka Messaging Engine (EDA)

- **SSoT:** La base de datos PostgreSQL local es la única fuente de verdad.
- **Flujo de Propagación:** Todo cambio en la BD (vía webhook o lógica interna) emite un evento a Kafka. El módulo de Sync consume estos eventos para replicar el estado en los satélites (Notion, Apple Reminders).
- **Loop Protection:** Se utiliza un encabezado `source_system` en los eventos de Kafka para evitar rebotes — no propagar de vuelta al sistema que originó el cambio.
- **Checksum Validation:** La tabla `sync_mappings` almacena el `last_known_checksum` de cada entidad sincronizada para detectar cambios reales y evitar propagaciones redundantes.

### 7.4. Learning Engine (Gestor de Estudio IA)

Orquesta el desarrollo de habilidades técnicas operando en dos capas:
- **Capa Estratégica (Throttling):** Regula la carga cognitiva diaria. Prioriza el Mantenimiento (Active Recall) y bloquea la ingesta de nuevos temas si la carga de repaso supera el umbral crítico.
- **Capa Operativa:** El `iaService` evalúa al usuario y enruta la interacción a través de Prompts Especializados: Diagnóstico, Priming, Estudio Quirúrgico, Zettelkasten o Backtrack (N-1), adaptándose al nivel de maestría detectado en cada sesión.

---

## 8. Modelo de Datos

Derivado del diagrama ERD (`varchar-5.eraserdiagram`) y validado contra `DATA-MODEL.md`. El esquema se organiza en seis schemas de PostgreSQL.

### 8.1. Schema `core` — Productividad y Ejecución

| Entidad | Descripción |
| :--- | :--- |
| **core_executable** | Unidad mínima de trabajo. Soporta tipos: `task`, `habit`, `lead_measure`, `activity`, `agenda`. Incluye atributos de priorización (`priority_score`, `urgence_score`, `effort_score`), coste estimado, métricas de esfuerzo (`energy_drain`, `mental_load`, `impact`) y referencia jerárquica (`parent_id`, `cycle_id`). |
| **core_cycle** | Períodos estratégicos (sprints de vida). Soporta jerarquía mediante `parent_id`. |

### 8.2. Schema `brain` — Captura Cognitiva

| Entidad | Descripción |
| :--- | :--- |
| **brain_idea** | Ideas capturadas en estado bruto. Ciclo de vida: `raw` → `refining` → `converted` (vinculada a un `core_executable` vía `converted_to_id`) o `archived`. |
| **context_event** | Registro de eventos contextuales con origen (`manual`, `system`, `integration`), proveedor, tipo, payload JSON y tags. |

### 8.3. Schema `telemetry` — Biometría y Actividad

| Entidad | Descripción |
| :--- | :--- |
| **tel_sleep_record** | Registros de sueño con duración, puntaje y etapas (JSON). |
| **tel_activity_stream** | Stream de actividad digital: aplicación, ventana activa, duración, estado AFK y payload crudo. |

### 8.4. Schema `finance` — Gestión Patrimonial

| Entidad | Descripción |
| :--- | :--- |
| **fin_account** | Representación de cuentas físicas (activos y pasivos). |
| **fin_category** | Taxonomía jerárquica de categorías de ingreso y gasto. |
| **fin_budget** | Límites de gasto por categoría con rangos de fechas flexibles. |
| **fin_transaction** | Libro mayor. Soporta partida doble (`origin_account_id`, `destination_account_id`). Vinculada a `core_executable` y `core_cycle` para integración cruzada productividad-finanzas. |
| **fin_goal** | Bolsillos lógicos (Sinking Funds). `target_amount` calculable dinámicamente. Vinculada a `core_executable` y `core_cycle`. |

### 8.5. Schema `learning` — Aprendizaje Continuo

| Entidad | Descripción |
| :--- | :--- |
| **lrn_topic** | El tema o concepto técnico a dominar. Rastrea el nivel de maestría (`current_score`) y la fecha del próximo repaso espaciado. |
| **lrn_assessment** | Registro inmutable de cada sesión de evaluación realizada por la IA. Guarda calificaciones detalladas (Internals, Architecture, Production) y recomienda el siguiente flujo. |

### 8.6. Schema `sync` — Sincronización

| Entidad | Descripción |
| :--- | :--- |
| **sync_mappings** | Puente universal entre IDs locales e IDs externos (Notion, Apple). Almacena `last_known_checksum` para prevención de rebotes y `sync_status` para trazabilidad. |

### 8.7. Schema `common` — Infraestructura Transversal

| Entidad | Descripción |
| :--- | :--- |
| **outbox_events** | Implementación del patrón Transactional Outbox. Registra eventos pendientes de publicación a Kafka con `aggregate_type`, `aggregate_id`, `event_type`, `payload` y flag `processed`. |

### 8.8. Relaciones Clave

- `core_cycle` ↔ `core_executable`: Un ciclo organiza múltiples ejecutables.
- `core_executable` → `core_executable`: Jerarquía de tareas (parent/child).
- `core_executable` ↔ `fin_transaction`: Atribución de coste real a tareas.
- `core_cycle` ↔ `fin_goal` / `fin_transaction`: Vinculación de presupuesto a nivel proyecto.
- `core_executable` → `brain_idea`: Conversión de ideas en tareas ejecutables.
- `core_executable` ↔ `lrn_assessment`: Las tareas programadas detonan evaluaciones cognitivas.
- `sync_mappings` → cualquier entidad: Mapeo generalizado por `local_id`.

---

## 9. Integraciones Día 1

| Sistema Externo | Tipo | Descripción |
| :--- | :--- | :--- |
| **Notion** | Frontend bidireccional | Bases de datos de Notion como interfaz de gestión. Sincronización REST a través del Gateway. |
| **Anytype** | Frontend bidireccional | Alternativa a Notion como interfaz de gestión. |
| **iOS Reminders / Calendar** | Frontend bidireccional | Sentinel detecta cambios → Kafka → Backend. Backend → eventAPI → iCloud para escritura. |
| **WhatsApp** | Entrada unidireccional | Webhook hacia el Backend para captura de interacciones NLP e ingesta a la capa vectorial. |
| **LLMs (tier gratuito)** | Procesamiento cognitivo | Consumo de endpoints de LLM en capa gratuita vía OpenRouter. Necesario para pruebas de concepto (PoC) sobre la interacción autónoma entre la IA y el esquema relacional. |

---

## 10. Ciclos de Automatización

| Disparador | Hora | Acción |
| :--- | :--- | :--- |
| **Shortcut Diario** | 05:30 AM | Habit Generator → Prioritizer → Planner → Sync Engine. |
| **Manual (Rutina)** | Dinámico | Replanificación basada en la hora actual. |
| **Cierre de Tarea** | Instantáneo | Sync de estado `DONE` hacia Notion e iOS. |
| **Evento externo** | Instantáneo | Cambio en Notion / iCloud / WhatsApp → Kafka → Core → Propagación. |

---

## 11. Almacenamiento en la Nube (NAS)

Derivado del diagrama `cloud-architecture-4`. Describe la estructura de almacenamiento centralizado en el NAS del ecosistema.

| Área | Contenido |
| :--- | :--- |
| **Files / Educación** | Colegio, PosgradoTry, Cursos, UNAL (Google Drive), PapelesUN, Documentos. |
| **Files / Documents** | Code, Finanzas, Papeles, Otros. Sincronizado bidireccionalmente con iCloud. |
| **Multimedia** | Fotos, GU, immich (servidor de fotos), Videos. iCloud sincroniza hacia immich. |
| **Software** | Reservado para distribución de software. |
| **Backup** | Respaldo bidireccional del NAS completo. |
| **Mac Mini** | Sincronización iCloud ↔ Google Drive (`i7.danielcc`). |

---

## Fase 2: Consolidación — Casos de Uso, Modelo de Dominio e Historias de Usuario

Los siguientes artefactos se derivan de las respuestas de la Fase 1 y de los diagramas de arquitectura. Se presentan para revisión y validación del stakeholder.

### 12. Casos de Uso del MVP

#### CU-01: Generación de Agenda Dinámica

- **Actor principal:** Sistema (automático) / Usuario (manual).
- **Disparador:** Shortcut diario a las 05:30 AM o solicitud manual.
- **Precondiciones:** Existen tareas con estado `todo` o `planned` en `core_executable`. La disponibilidad horaria está registrada en iOS Calendar.
- **Flujo principal:**
    1. El Habit Generator crea las instancias diarias de hábitos y lead measures.
    2. El Prioritizer Engine calcula el `priority_score` de cada tarea pendiente.
    3. El Planner Engine lee la disponibilidad desde iOS Calendar, clasifica las tareas por bloques energéticos (Mañana, Tarde, Noche) según el `effort_score` y genera la agenda.
    4. El Sync Engine escribe la agenda resultante en Notion y Apple Reminders.
- **Postcondiciones:** Las tareas priorizadas aparecen como ejecutables con `type: agenda` en la base de datos y son visibles en todas las interfaces.

#### CU-02: Sincronización Bidireccional

- **Actor principal:** Sistema (reactivo).
- **Disparador:** Cambio detectado en cualquier satélite (Notion, iCloud, WhatsApp) o en la base de datos local.
- **Precondiciones:** El mapping `sync_mappings` existe para la entidad afectada.
- **Flujo principal (Entrada — satélite → core):**
    1. El componente receptor detecta el cambio: Sentinel (iCloud → Kafka), Gateway REST (Notion → SQS/Kafka), Webhook (WhatsApp → Gateway).
    2. El Consumer deserializa el evento y lo entrega al EventRouter.
    3. El EventRouter despacha al IEventHandler correspondiente.
    4. El IEventHandler valida el checksum contra `sync_mappings`. Si hay cambio real, persiste en PostgreSQL vía CoreRepo.
    5. El OutboxWorker detecta el nuevo registro en `outbox_events` y publica el evento de propagación en Kafka.
- **Flujo principal (Salida — core → satélite):**
    1. El SyncServ consume el evento de propagación.
    2. Verifica el `source_system` del evento para evitar rebotes (Loop Protection).
    3. Propaga el cambio al satélite correspondiente vía API REST (Notion) o eventAPI → EventKit (iCloud).
    4. Actualiza el `last_known_checksum` y `last_synced_at` en `sync_mappings`.
- **Postcondiciones:** El estado es consistente en PostgreSQL y en todos los satélites. Cero pérdida de información.

#### CU-03: Ingesta Pasiva de Telemetría

- **Actor principal:** Sistema (pasivo).
- **Disparador:** Nuevo registro de sueño, actividad digital o transacción financiera.
- **Precondiciones:** Los sensores o integraciones de datos están configurados.
- **Flujo principal:**
    1. Los datos de biometría (`tel_sleep_record`), actividad digital (`tel_activity_stream`) o transacciones financieras (`fin_transaction`) ingresan al sistema a través de webhooks, APIs o procesos batch.
    2. Se persisten en sus respectivos schemas.
    3. Se emite un evento a Kafka para notificar al iaService de la disponibilidad de datos frescos.
- **Postcondiciones:** Los datos están disponibles para el Prioritizer Engine y el iaService.

#### CU-04: Consulta Contextual Inteligente

- **Actor principal:** iaService (autónomo).
- **Disparador:** Solicitud de generación de agenda, respuesta a consulta del usuario vía WhatsApp, o decisión autónoma del agente.
- **Precondiciones:** El iaService tiene acceso de lectura a PostgreSQL (capa relacional) y al RAG (capa vectorial).
- **Flujo principal:**
    1. El iaService recibe un evento que requiere razonamiento.
    2. Consulta la capa relacional (PostgreSQL) para datos estructurados: tareas pendientes, saldos financieros, disponibilidad de agenda.
    3. Consulta la capa vectorial (RAG) para contexto histórico no estructurado: conversaciones previas, patrones de comportamiento.
    4. Enruta la solicitud a OpenClaw → OpenRouter → LLM para generar la respuesta o decisión.
    5. El resultado se persiste (si aplica) y se propaga a las interfaces correspondientes.
- **Postcondiciones:** La IA ha generado una respuesta o acción basada en contexto completo (relacional + vectorial).

> [!NOTE] Comentarios sobre Casos de Uso:
> (Espacio reservado para validación del stakeholder. Indicar si falta algún caso de uso vital para el MVP o si los propuestos requieren modificación.)

---

### 13. Catálogo de Eventos y Lógica Reactiva (Event-Driven Logic)

A medida que el ecosistema crezca, los nuevos casos de uso no se programarán como endpoints aislados, sino como lógica de dominio reactiva (IEventHandler) que escucha eventos específicos desde el bus de Kafka. Esta tabla sirve como el registro vivo de la relación entre Eventos y la Lógica Relacional que ejecutan.

| Evento (Kafka Topic/Type) | Origen | Lógica que Dispara (IEventHandler) | Impacto en Capa Relacional (PostgreSQL) |
| :--- | :--- | :--- | :--- |
| `TaskCompletedEvent` | Sentinel (iOS) / Gateway (Notion) | Valida cierre de tarea. Si es `LEAD_MEASURE`, recalcula el Lag Measure asociado. | Actualiza estado en `core_executable`. Si tiene costo asociado, puede instanciar un `fin_transaction` preliminar. |
| `FinanceTransactionLoggedEvent` | Integración bancaria / Manual | Cruza el evento financiero con los presupuestos activos. | Inserta en `fin_transaction`. Actualiza `accumulated_amount` en `fin_goal` o saldo en `fin_account`. |
| `TimeBlockFinishedEvent` | Core (Planner) | Evalúa el tiempo real invertido vs el estimado en la sesión. | Actualiza `actual_duration_minutes` en `core_time_block` y ajusta heurística de priorización para futuras tareas. |
| `DailyAgendaRequestedEvent` | Cronjob (05:30 AM) / Manual | Invoca al `Prioritizer` y `Planner Engines`. Lee telemetría de la noche anterior. | Crea o actualiza instancias de `core_executable` tipo `agenda` para el día en curso. |
| `BiometricSleepLoggedEvent` | Apple Health / Webhook | Analiza la calidad del sueño (Sleep Score). | Inserta en `tel_sleep_record`. Disminuye el tope de `energy_drain` disponible para el `Agenda Planner` del día. |
| `IdeaCapturedEvent` | WhatsApp Webhook | Llama al LLM para analizar la semántica y etiquetar. | Inserta en `brain_idea` con estado `raw` o `refining`. Ingiere el contenido en la base vectorial (RAG). |
| `LearningSessionTriggeredEvent` | Sentinel / Core | Al completarse un `core_executable` de tipo estudio, el `iaService` evalúa el historial (`LRN_TOPIC`), decide el Prompt IA (A, B, C, D, E) e inicia la sesión interactiva. | Inserta evaluación en `LRN_ASSESSMENT`, actualiza maestría en `LRN_TOPIC` y reprograma el próximo bloque de estudio (Spaced Repetition). |

> [!NOTE] Comentarios sobre el Catálogo de Eventos:
> (Añade aquí nuevos eventos que se te vayan ocurriendo o lógica adicional que deba ejecutarse cuando ocurran los eventos listados.)

---

### 14. Modelo de Dominio Consolidado

Las entidades core del sistema, agrupadas por bounded context (DDD):

| Bounded Context | Entidades | Responsabilidad |
| :--- | :--- | :--- |
| **Core (Productividad)** | `CoreExecutable`, `CoreCycle` | Modelar la ejecución de tareas, hábitos, actividades y su organización en ciclos estratégicos. |
| **Brain (Cognición)** | `BrainIdea`, `ContextEvent` | Captura de ideas en bruto y registro de eventos contextuales para alimentar la IA. |
| **Telemetry** | `TelSleepRecord`, `TelActivityStream` | Recolección pasiva de datos biométricos y de actividad digital. |
| **Finance** | `FinAccount`, `FinTransaction`, `FinCategory`, `FinBudget`, `FinGoal` | Gestión patrimonial completa: cuentas, transacciones, presupuestos y metas de ahorro. |
| **Learning** | `LrnTopic`, `LrnAssessment` | Orquestación cognitiva: gestión de sesiones de estudio, throttling y evaluación IA. |
| **Sync** | `SyncMappings` | Mapeo bidireccional de identidades entre el sistema local y los satélites externos. |
| **Common** | `OutboxEvents` | Infraestructura transversal para el patrón Transactional Outbox. |

> [!NOTE] Comentarios sobre Modelo de Dominio:
> (Espacio reservado para validación del stakeholder. Indicar si se requiere alguna otra entidad principal desde el Día 1.)

---

### 15. Historias de Usuario — Backlog MVP

#### Dominio: Agenda y Productividad

- **HU-01 — Generación Automática de Agenda:** Como usuario, necesito que el sistema genere automáticamente mis prioridades diarias basadas en mi telemetría reciente y mis tareas pendientes, para reducir la fricción en la toma de decisiones matutina.
- **HU-02 — Replanificación Dinámica:** Como usuario, necesito poder solicitar una replanificación de mi agenda en cualquier momento del día, para adaptarme a cambios imprevistos sin perder contexto.
- **HU-03 — Captura de Ideas:** Como usuario, necesito que mis mensajes de WhatsApp se capturen automáticamente como ideas en bruto (`brain_idea`), para no perder ningún pensamiento que pueda convertirse en tarea.

#### Dominio: Aprendizaje Continuo (Learning)

- **HU-04 — Evaluación Diagnóstica IA:** Como usuario, necesito que al completar una tarea de estudio, el agente de IA evalúe objetivamente mis brechas con una sesión interactiva (Prompt A, B, C).
- **HU-05 — Prevención de Saturación (Throttling):** Como usuario, necesito que el sistema me prohíba inyectar temas nuevos a mi agenda si mi deuda de repasos supera un umbral crítico.

#### Dominio: Sincronización

- **HU-06 — Sync iOS → PostgreSQL:** Como usuario, necesito que mis tareas creadas nativamente en iOS Reminders se reflejen instantáneamente en la base de datos central.
- **HU-07 — Sync PostgreSQL → Notion:** Como usuario, necesito que los cambios en la base de datos se propaguen a mis bases de datos de Notion.
- **HU-08 — Tolerancia a Fallos en Sync:** Como SysAdmin, necesito que los eventos fallidos sean retenidos en Kafka para su reprocesamiento.

#### Dominio: Finanzas

- **HU-09 — Registro de Transacciones:** Como usuario, necesito que mis transacciones se vinculen automáticamente a las tareas o ciclos para visibilidad del costo.
- **HU-10 — Control Presupuestario:** Como usuario, necesito alertas cuando exceda el presupuesto de una categoría.

#### Dominio: Infraestructura

- **HU-11 — Despliegue Híbrido:** Como SysAdmin, necesito orquestar los contenedores en Ubuntu y delegar IA al Mac Mini.
- **HU-12 — Observabilidad:** Como SysAdmin, necesito un tablero en Grafana para ver el estado de los servicios, latencias y métricas de Kafka.

> [!NOTE] Comentarios sobre Historias de Usuario:
> (Espacio reservado para validación del stakeholder. Indicar impresiones o historias que se consideren faltantes.)

---

## 16. Referencia a Diagramas de Arquitectura

Los diagramas fuente de este documento se encuentran en:

| Diagrama | Archivo | Descripción |
| :--- | :--- | :--- |
| Deployment | `Architecture-Deployment-1.eraserdiagram` | Topología de nodos, red Tailscale, servicios por máquina y conectividad iCloud. |
| Components | `Architecture-Components-2.eraserdiagram` | Componentes del sistema, protocolos de comunicación y flujos de datos. |
| Core | `Architecture-Core-3.eraserdiagram` | Arquitectura interna del módulo Core: Consumer, EventRouter, IEventHandler, SyncServ, Producer. |
| Cloud Architecture | `Architecture-cloud-architecture-4.eraserdiagram` | Estructura del NAS, sincronización iCloud y política de backups. |
| Modelo de Datos (ERD) | `Architecture-varchar-5.eraserdiagram` | Esquema completo de la base de datos: entidades, atributos, enums y relaciones. |

Editar en Eraser: [https://app.eraser.io/workspace/qufdoKZ8oNs9GJjafWv0](https://app.eraser.io/workspace/qufdoKZ8oNs9GJjafWv0)

---

*Una vez validados los Casos de Uso, el Modelo de Dominio y las Historias de Usuario, este documento queda cerrado como línea base de requerimientos para el desarrollo del MVP.*
