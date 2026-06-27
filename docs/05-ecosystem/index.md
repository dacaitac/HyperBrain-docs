# Ecosistema

Registro de componentes, repositorios e integraciones del proyecto HyperBrain.

## Repositorios

| Repositorio | Descripción | Tecnología |
| :--- | :--- | :--- |
| `HyperBrain-docs` | Base de conocimiento de ingeniería (este sitio). | MkDocs Material, Mermaid. |
| `HyperBrain-be` | Backend monolítico modular (DDD). | Spring Boot, Java, PostgreSQL. |
| `HyperBrain-Infra` | Infraestructura como código y despliegue. | Docker, Kubernetes, Terraform. |
| `conductor` | Orquestación de workflows. | — |

## Integraciones Día 1

| Sistema Externo | Tipo | Descripción |
| :--- | :--- | :--- |
| **Notion** | Frontend bidireccional | Bases de datos de Notion como interfaz de gestión. Sincronización REST a través del Gateway. |
| **Anytype** | Frontend bidireccional | Alternativa a Notion como interfaz de gestión. |
| **iOS Reminders / Calendar** | Frontend bidireccional | Sentinel detecta cambios → SQS → Backend. Backend → eventAPI → iCloud para escritura. |
| **WhatsApp** | Entrada unidireccional | Webhook hacia el Backend para captura de interacciones NLP e ingesta a la capa vectorial. |
| **LLMs (tier gratuito)** | Procesamiento cognitivo | Consumo de endpoints de LLM en capa gratuita vía OpenRouter. |

## Ciclos de Automatización

| Disparador | Hora | Acción |
| :--- | :--- | :--- |
| **Shortcut Diario** | 05:30 AM | Habit Generator → Prioritizer → Planner → Sync Engine. |
| **Manual (Rutina)** | Dinámico | Replanificación basada en la hora actual. |
| **Cierre de Tarea** | Instantáneo | Sync de estado `DONE` hacia Notion e iOS. |
| **Evento externo** | Instantáneo | Cambio en Notion / iCloud / WhatsApp → SQS → Core → Propagación. |
| **Ciclo presupuestario** | Configurable | Generación automática de presupuestos del nuevo período. |
