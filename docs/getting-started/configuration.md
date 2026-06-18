# Configuración

## Variables de Entorno

Crea un archivo `.env` en la raíz del proyecto con las siguientes variables:

```bash
# Configuración básica
HYPERBRAIN_ENV=development
HYPERBRAIN_DEBUG=true
HYPERBRAIN_PORT=8000
```

## Archivo de Configuración

El archivo principal de configuración es `config.yml`:

```yaml
app:
  name: HyperBrain
  version: "1.0.0"
  debug: true

server:
  host: 0.0.0.0
  port: 8000
```

!!! warning "Variables sensibles"
    Nunca subas el archivo `.env` al repositorio. Asegúrate de que esté incluido en `.gitignore`.

## Siguiente paso

Revisa la [Arquitectura](../architecture/overview.md) para entender el diseño del sistema.
