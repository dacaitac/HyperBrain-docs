# Stack Tecnológico

Definido durante la sesión [JAD](../01-product/jad-session.md) y validado contra los diagramas de despliegue y componentes.

| Dominio | Tecnología | Justificación |
| :--- | :--- | :--- |
| **Core y Backend** | Spring Boot (Java) | Solidez y escalabilidad para la lógica de negocio. Arquitectura modular DDD: `core`, `sync`, `finance`, `learning`, `cognitive`, `prioritizer`, `planner`. |
| **Persistencia Relacional** | PostgreSQL + Supabase | PostgreSQL como SSoT (Single Source of Truth). Supabase para aceleración de acceso a datos y autenticación. |
| **Persistencia Vectorial** | RAG (Base de datos vectorial) | Almacenamiento de embeddings de interacciones NLP y metadatos contextuales. |
| **Mensajería y Eventos** | Amazon SQS + DLQ | Cola de mensajes administrada para el patrón Event-Driven Architecture. Dead Letter Queue (DLQ) para tolerancia a fallos y reprocesamiento. |
| **Contenerización** | Docker | Empaquetado de todos los servicios del ecosistema. |
| **Orquestación** | Kubernetes (K8s) | Escalado y gestión de ciclo de vida de contenedores. |
| **Automatización de Workflows** | n8n | Integración de flujos de trabajo de terceros (low-code). |
| **Paneles Internos** | Appsmith | Desarrollo rápido de herramientas internas, dashboards 4DX y paneles de control. Acceso directo a PostgreSQL. |
| **IA — Enrutamiento LLM** | OpenRouter | Pasarela de enrutamiento unificada para múltiples proveedores de modelos de lenguaje. |
| **IA — Agentes/Workflows** | OpenClaw | Orquestación de tareas de IA pesadas. Consume OpenRouter. |
| **IA — Ejecución Local** | Ollama-MLX | Ejecución de modelos de IA en hardware local (Mac Mini M4 Pro). |
| **Ecosistema Apple** | Swift (EventKit, Vapor) | **Sentinel:** Demonio macOS para monitoreo de cambios nativos en iCloud. **eventAPI:** Servicio REST para escritura en EventKit. |
| **Red Privada** | Tailscale | VPN mesh que interconecta todos los nodos de la infraestructura distribuida. |
