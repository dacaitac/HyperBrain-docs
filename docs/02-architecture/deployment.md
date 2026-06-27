# Arquitectura de Despliegue

Modelo híbrido distribuido (Cloud / On-Premise) compuesto por tres nodos interconectados mediante Tailscale.

Para los diagramas visuales completos, consultar: [Diagramas (Eraser.io)](diagrams.md).

---

## Topología de Nodos

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
│  └────────┬────────┘  │  │  • Backend       • SQS (LocalSt.) │  │  │
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

## Descripción de Nodos

| Nodo | Ubicación | Función |
| :--- | :--- | :--- |
| **daniel-ubuntu** | Servidor local | Core de HyperBrain contenerizado: Backend Spring Boot, PostgreSQL, SQS (LocalStack o ElasticMQ), RAG, Appsmith, OpenRouter, OpenClaw. |
| **Mac Mini M4 Pro** | On-premise | Servicios nativos de Apple: **Sentinel** (demonio de monitoreo iCloud vía EventKit → SQS), **eventAPI** (escritura REST → iCloud) y **Ollama-MLX** (inferencia local de modelos de IA). |
| **Control Plane (OCI)** | Oracle Cloud Infrastructure | Orquestación de Kubernetes y observabilidad integral mediante Grafana. |
| **Apple Device** | Dispositivo del usuario | Punto de interacción del usuario. Accede a Appsmith (WebUI) y sincroniza bidireccionalmente con iCloud. |
