# Contribuir

¡Gracias por tu interés en contribuir a HyperBrain! 🎉

## ¿Cómo contribuir?

### 1. Fork y Clone

```bash
# Fork el repositorio desde GitHub, luego:
git clone https://github.com/<tu-usuario>/HyperBrain-docs.git
cd HyperBrain-docs
```

### 2. Crear una rama

```bash
git checkout -b feature/mi-mejora
```

### 3. Realizar cambios

Haz tus cambios y asegúrate de que la documentación se construye correctamente:

```bash
mkdocs serve
```

### 4. Commit y Push

```bash
git add .
git commit -m "docs: descripción de los cambios"
git push origin feature/mi-mejora
```

### 5. Pull Request

Abre un Pull Request desde tu rama hacia `main`.

## Convenciones

- **Commits**: Usa [Conventional Commits](https://www.conventionalcommits.org/es/)
- **Idioma**: La documentación se escribe en español
- **Formato**: Usa Markdown siguiendo las convenciones de MkDocs Material

## Estructura del proyecto

```
HyperBrain-docs/
├── docs/
│   ├── index.md
│   ├── getting-started/
│   │   ├── installation.md
│   │   └── configuration.md
│   ├── architecture/
│   │   └── overview.md
│   ├── api/
│   │   └── reference.md
│   └── contributing.md
├── mkdocs.yml
└── .github/
    └── workflows/
        └── deploy.yml
```

!!! question "¿Tienes dudas?"
    Abre un [Issue](https://github.com/dacaitac/HyperBrain-docs/issues) en el repositorio.
