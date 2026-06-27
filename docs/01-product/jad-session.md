# Sesión JAD — HyperBrain

> **Versión:** 2.0
> **Fecha:** 2026-06-26
> **Facilitador:** JAD Facilitator (AI-Assisted)
> **Stakeholder:** Daniel Caita

---

## 1. Propósito del Documento

Este documento registra los resultados históricos de la sesión de Diseño Conjunto de Aplicaciones (JAD) para el proyecto **HyperBrain**. Su contenido ha sido formalizado y distribuido en las secciones correspondientes de la base de conocimiento:

| Contenido JAD | Ubicación Formal |
| :--- | :--- |
| Visión, pilares teóricos, MVP, roles | [Visión del Producto](vision.md) |
| Stack tecnológico | [Stack Tecnológico](../02-architecture/tech-stack.md) |
| Arquitectura de despliegue | [Despliegue](../02-architecture/deployment.md) |
| Arquitectura de componentes | [Componentes](../02-architecture/components.md) |
| Motores de ejecución | [Prioritizer](../02-architecture/engines/prioritizer.md), [Planner](../02-architecture/engines/planner.md), [Learning](../02-architecture/engines/learning.md), [Finance](../02-architecture/engines/finance.md), [Messaging](../02-architecture/engines/messaging.md) |
| Modelo de datos (ERD) | [Modelo de Datos](../02-architecture/data-model.md) |
| Bases teóricas | [4DX](../02-architecture/theory/4dx.md), [GTD](../02-architecture/theory/gtd.md), [Spaced Repetition](../02-architecture/theory/spaced-repetition.md), [Triángulo de Control](../02-architecture/theory/triangle-of-control.md) |
| Catálogo de eventos | [Messaging Engine](../02-architecture/engines/messaging.md) |
| Integraciones y ciclos | [Ecosistema](../05-ecosystem/index.md) |
| Historias de usuario | [Backlog MVP](requirements/index.md) |
| Decisión SQS vs Kafka | [ADR-001](../03-adrs/ADR-001-sqs-over-kafka.md) |

---

## 2. Registro de la Entrevista Estructurada — Fase 1

Las siguientes son las respuestas textuales del stakeholder durante la entrevista, preservadas como registro histórico.

### 2.1. ¿Quiénes son los usuarios finales y qué roles tendrán?

Personas enfocadas en su productividad — estudiantes, emprendedores, profesionales. Dos roles: Usuario Estándar (consume el sistema, crea objetivos, tareas y transacciones, monitorea dashboards 4DX) y Administrador del Sistema (despliegue, infraestructura, monitoreo).

### 2.2. ¿Cuáles son las funcionalidades que el MVP debe cubrir sin falta?

1. Generación de agenda dinámica con resolución instantánea.
2. Sincronización bidireccional con software de terceros (Notion, iOS).
3. Integración profunda de base de datos con IA para contexto personalizado.
4. Gestión financiera personal: flujo de caja, presupuestos dinámicos y fondos de amortización.
5. Motor de aprendizaje inteligente (Study Manager) con Spaced Repetition, Active Recall y Throttling cognitivo.

### 2.3. ¿Cómo se gestionará el conocimiento?

Doble capa: PostgreSQL (relacional, SSoT) para datos estructurados y RAG (vectorial) para historial de interacciones NLP y contexto no estructurado.

### 2.4. ¿Cuáles son los KPIs del éxito del MVP?

Tasa de éxito en sincronización (cero pérdida), estabilidad operativa del core, retención en alpha cerrada (entorno familiar), y precisión presupuestaria.

### 2.5. ¿Qué restricciones o preferencias existen?

Uso preferente de software open source. Seguridad MVP limitada a autenticación estándar.

---

## 3. Fuentes de la Sesión

- Respuestas del stakeholder durante la entrevista (Fase 1).
- Diagramas de arquitectura en Eraser.io: `Deployment`, `Components`, `Core`, `Modelo de Datos`.
- Documentación del ecosistema: `CORE-ARCHITECTURE.md`, `DATA-MODEL.md`, `HYPERBRAIN-PRODUCTION-ROADMAP.md`.
- Documento de diseño de finanzas: *"Arquitectura y Modelado de Sistema de Finanzas Personales"*.
- Sistema de gestión de estudio (Training) exportado desde Notion.

---

*Este documento queda cerrado como registro histórico. La fuente de verdad formal de cada área se encuentra en las secciones enlazadas arriba.*
