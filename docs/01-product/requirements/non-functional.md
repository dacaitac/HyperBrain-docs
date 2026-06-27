# Requerimientos No Funcionales (RNF)

Los Requerimientos No Funcionales (RNF) establecen los atributos de calidad, restricciones técnicas y estándares de operación que el ecosistema HyperBrain debe cumplir, garantizando su estabilidad, escalabilidad y usabilidad.

---

## 1. Disponibilidad y Confiabilidad

| ID | Requerimiento No Funcional | Descripción |
| :--- | :--- | :--- |
| **RNF-01** | **Tolerancia a fallos de sincronización** | Si un servicio externo (Notion, iCloud API) cae, el sistema debe encolar los mensajes en SQS y reintentar con backoff exponencial. |
| **RNF-02** | **Protección contra pérdida de eventos (DLQ)** | Cualquier evento que agote los reintentos permitidos (maxReceives) debe moverse obligatoriamente a una Dead Letter Queue (DLQ) para reprocesamiento manual, garantizando 0% de pérdida de datos. |
| **RNF-03** | **Single Source of Truth (SSoT)** | PostgreSQL será la única fuente de verdad. En caso de conflicto de estado entre un satélite (ej. Notion) y PostgreSQL, prevalece el estado de PostgreSQL, usando el checksum para determinar el estado más reciente. |

## 2. Rendimiento (Performance)

| ID | Requerimiento No Funcional | Descripción |
| :--- | :--- | :--- |
| **RNF-04** | **Latencia de propagación de eventos** | El tiempo transcurrido desde que un satélite reporta un cambio (ej. tarea completada) hasta que se persiste en PostgreSQL debe ser menor a 2 segundos (P95). |
| **RNF-05** | **Latencia de IA y Enrutamiento** | El cálculo de la agenda diaria (Prioritizer + Planner) no debe bloquear el loop de eventos; debe resolverse de manera asíncrona en menos de 10 segundos. |
| **RNF-06** | **Tasa de Inferencia Local** | El servicio `Ollama-MLX` (corriendo en el Mac Mini M4) debe priorizarse para modelos que quepan en memoria unificada (ej. 8B-14B parámetros), delegando tareas pesadas a OpenRouter (API externa). |

## 3. Seguridad y Privacidad

| ID | Requerimiento No Funcional | Descripción |
| :--- | :--- | :--- |
| **RNF-07** | **Segregación de Credenciales** | Los tokens de API de Notion, Apple y OpenRouter no deben guardarse en texto plano en repositorios. Deben gestionarse mediante Kubernetes Secrets o variables de entorno `.env` no commiteadas. |
| **RNF-08** | **Autenticación (MVP)** | Aunque el sistema es de uso personal (single-tenant en esta fase), la API y el portal (Appsmith) deben requerir autenticación JWT / Basic Auth para evitar exposición de red local (Tailscale). |
| **RNF-09** | **Aislamiento de Red** | Todo el tráfico entre nodos (Ubuntu ↔ Mac Mini) debe enrutarse exclusivamente de forma encriptada a través de la red mesh privada de Tailscale. No se deben exponer puertos al internet público, excepto Webhooks asegurados. |

## 4. Despliegue y Operación

| ID | Requerimiento No Funcional | Descripción |
| :--- | :--- | :--- |
| **RNF-10** | **Contenerización Estándar** | Todos los servicios de backend (Spring Boot), bases de datos y colas deben empaquetarse en imágenes Docker compatibles con arquitecturas ARM64 y AMD64. |
| **RNF-11** | **Orquestación Híbrida** | El sistema debe soportar un despliegue donde el plano de control (Kubernetes en OCI) supervise nodos *on-premise* y delegue cargas de trabajo a través del plugin o agente de Tailscale. |
| **RNF-12** | **Observabilidad Centralizada** | Todos los logs de los servicios (Java, Node, Python) deben inyectarse en salida estándar (`stdout`) para su ingesta por una solución central (Grafana/Prometheus) que monitoree el estado de colas SQS, uso de CPU/RAM y latencias. |

## 5. Mantenibilidad

| ID | Requerimiento No Funcional | Descripción |
| :--- | :--- | :--- |
| **RNF-13** | **Arquitectura de Dominio (DDD)** | El código base (`HyperBrain-be`) debe respetar los límites de los subdominios (Productividad, Finanzas, Aprendizaje, Sincronización), de modo que un cambio en finanzas no rompa la sincronización. |
| **RNF-14** | **Código Abierto (Open Source)** | En la medida de lo posible, se debe dar preferencia al uso de herramientas y librerías Open Source que reduzcan el vendor lock-in comercial (ej. usar ElasticMQ/LocalStack para SQS en desarrollo local). |
