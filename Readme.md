# 🚀 CI/CD con Python y GitHub Actions

**Construcción de Package + Pruebas + Artefactos**

Este repositorio sirve como un ejemplo **completo y funcional** del ciclo de **Integración Continua (CI)** y **Entrega Continua (CD)** utilizando Python, `pytest`, y automatizado completamente con **GitHub Actions**.

El proyecto demuestra cómo resolver errores comunes de empaquetado cuando los módulos (`app.py`, `calculator.py`) se encuentran en la raíz del proyecto.

-----

## 📌 1. Conceptos Clave: CI/CD

### ✔️ Integración Continua (CI)

Es el proceso automático que se ejecuta cada vez que se sube código al repositorio (p. ej., un `git push`). Su objetivo es validar la funcionalidad del código lo antes posible.

**Actividades de CI en este proyecto:**

  * Descargar el repositorio.
  * Instalar dependencias (`pytest`, `build`).
  * **Ejecutar pruebas unitarias** asegurando que `PYTHONPATH` incluya el directorio raíz.

### ✔️ Entrega Continua (CD)

Consiste en generar automáticamente un *package* o *artefacto* listo para ser distribuido.

**Actividad de CD en este proyecto:**

  * Construcción del paquete utilizando la configuración de `pyproject.toml`.
  * Generación de formatos estándar (`*.tar.gz` y `*.whl`) dentro de `dist/`.

-----

## 📂 2. Estructura del Proyecto

El proyecto tiene una estructura plana (archivos en la raíz), lo cual requiere una configuración específica para ser empaquetado:

```text
ci_cd_python/
├── app.py                  # Aplicación principal
├── calculator.py           # Módulo con la lógica (suma)
├── pyproject.toml          # ⚙️ CONFIGURACIÓN CRUCIAL PARA EL BUILD
├── tests/
│   └── test_calculator.py  # Pruebas unitarias
├── .github/
│   └── workflows/
│       └── ci.yml          # Pipeline de GitHub Actions
└── Readme.md               # Documentación
```

-----

## ⚙️ 3. Configuración del Build (La Solución al Error)

Para que el comando `python -m build` funcione en esta estructura sin errores, es **obligatorio** tener un archivo `pyproject.toml`.

Dado que `app.py` y `calculator.py` están sueltos en la raíz (y no en una carpeta `src`), debemos indicarle explícitamente a `setuptools` qué módulos incluir usando `py-modules`.

### Contenido de `pyproject.toml`

```toml
[build-system]
requires = ["setuptools>=61.0"]
build-backend = "setuptools.build_meta"

[project]
name = "proyecto-ci-cd"
version = "0.1.0"
description = "Demostración de CI/CD con estructura plana"
readme = "Readme.md"
requires-python = ">=3.10"
authors = [{ name = "OmarJB20" }]
classifiers = [
    "Programming Language :: Python :: 3",
    "Operating System :: OS Independent",
]

# ⚠️ ESTA SECCIÓN ES LA CLAVE
# Indica que debe empaquetar los archivos sueltos 'app' y 'calculator'
[tool.setuptools]
py-modules = ["app", "calculator"]
```

-----

## 💻 4. Código Fuente

### ✔️ 4.1 `calculator.py`

```python
def add(a, b):
    """Suma dos números y devuelve el resultado."""
    return a + b
```

### ✔️ 4.2 `app.py`

```python
from calculator import add

if __name__ == "__main__":
    print("Suma:", add(5, 7)) 
```

### ✔️ 4.3 `tests/test_calculator.py`

```python
from calculator import add

def test_add():
    assert add(2, 3) == 5
```

-----

## 🚀 5. Pipeline CI/CD (GitHub Actions)

El flujo de trabajo se define en `.github/workflows/ci.yml`. Se han aplicado correcciones para manejar el `PYTHONPATH` y asegurar que el build encuentre el archivo de configuración.

```yaml
name: CI Pipeline

on:
  push:
  pull_request:

jobs:
  build-test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.10"

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install pytest build

      - name: Run tests 🧪 (Configurando PYTHONPATH)
        run: |
          # Exportamos el directorio actual al path para que pytest encuentre 'calculator.py'
          export PYTHONPATH=$PYTHONPATH:$(pwd)
          pytest

      - name: Build Python package
        # Esto buscará automáticamente el archivo pyproject.toml
        run: python -m build

      - name: Upload package artifacts
        uses: actions/upload-artifact@v4
        with:
          name: python-package
          path: dist/*
```

-----

## 🔄 6. Resultado del Pipeline

1.  **Tests:** Se ejecutan y validan la lógica de `calculator.py`.
2.  **Build:** Gracias a `pyproject.toml`, se genera la carpeta `dist/` con:
      * `proyecto_ci_cd-0.1.0-py3-none-any.whl`
      * `proyecto_ci_cd-0.1.0.tar.gz`
3.  **Artifacts:** Estos archivos quedan disponibles para descarga en la pestaña "Actions" de GitHub.

-----

## 💻 7. Ejecución Local

Para probar todo en tu máquina antes de subir:

```bash
# 1. Crear entorno virtual
python -m venv .venv
source .venv/bin/activate  # O .venv\Scripts\activate en Windows

# 2. Instalar herramientas
pip install pytest build

# 3. Correr tests (Linux/Mac)
export PYTHONPATH=$PYTHONPATH:$(pwd)
pytest

# 4. Construir paquete (debe existir pyproject.toml)
python -m build
```

-----

## 👤 Autor

**OmarJB20** — Proyecto CI/CD en Python
**Año:** 2025