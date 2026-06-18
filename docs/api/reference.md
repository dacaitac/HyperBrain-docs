# Referencia de la API

## Base URL

```
https://api.hyperbrain.dev/v1
```

## Autenticación

Todas las peticiones requieren un token Bearer en el header:

```bash
Authorization: Bearer <tu-token>
```

---

## Endpoints

### Usuarios

#### Obtener usuario actual

```http
GET /users/me
```

**Respuesta exitosa (200):**

```json
{
  "id": "usr_123",
  "email": "usuario@ejemplo.com",
  "name": "Daniel",
  "created_at": "2026-01-01T00:00:00Z"
}
```

#### Actualizar perfil

```http
PUT /users/me
Content-Type: application/json
```

**Body:**

```json
{
  "name": "Nuevo Nombre",
  "email": "nuevo@ejemplo.com"
}
```

---

### Recursos

#### Listar recursos

```http
GET /resources?page=1&limit=20
```

**Parámetros de consulta:**

| Parámetro | Tipo | Descripción |
|---|---|---|
| `page` | `int` | Número de página (default: 1) |
| `limit` | `int` | Elementos por página (default: 20) |
| `search` | `string` | Filtro de búsqueda |

**Respuesta exitosa (200):**

```json
{
  "data": [],
  "total": 0,
  "page": 1,
  "limit": 20
}
```

#### Crear recurso

```http
POST /resources
Content-Type: application/json
```

**Body:**

```json
{
  "title": "Mi Recurso",
  "description": "Descripción del recurso",
  "type": "document"
}
```

---

## Códigos de Estado

| Código | Descripción |
|---|---|
| `200` | Éxito |
| `201` | Recurso creado |
| `400` | Petición inválida |
| `401` | No autenticado |
| `403` | No autorizado |
| `404` | No encontrado |
| `500` | Error del servidor |

!!! note "Rate Limiting"
    La API tiene un límite de 100 peticiones por minuto por token.
