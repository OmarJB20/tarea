# 🚀 CI/CD con Python y GitHub Actions

**Construcción de Package + Pruebas + Artefactos**

Este repositorio sirve como un ejemplo **completo y funcional** del ciclo de **Integración Continua (CI)** y **Entrega Continua (CD)** utilizando Python, `pytest`, y automatizado completamente con **GitHub Actions**.

Aprende cómo configurar un pipeline que va desde el *push* del código hasta la generación de un paquete de Python (`sdist` y `wheel`) listo para distribuir.

-----

## 📌 1. Conceptos Clave: CI/CD

### ✔️ Integración Continua (CI)

Es el proceso automático que se ejecuta cada vez que se sube código al repositorio (p. ej., un `git push`). Su objetivo es validar la funcionalidad del código lo antes posible.

**Actividades de CI en este proyecto:**

  * Descargar el repositorio.
  * Instalar dependencias.
  * **Ejecutar pruebas unitarias** (`pytest`).
  * Validar que el código funcione correctamente.

### ✔️ Entrega Continua (CD)

Consiste en generar automáticamente un *package* o *artefacto* listo para ser distribuido o desplegado.

**Actividad de CD en este proyecto:**

  * Construcción del paquete de Python en formato estándar (`*.tar.gz` y `*.whl`) dentro del directorio `dist/`.

-----

## 📂 2. Estructura del Proyecto

El proyecto sigue una estructura modular para facilitar las pruebas y la empaquetación:

```
ci_cd_python/
├── app.py                  # Aplicación principal que usa el módulo calculator
├── calculator.py           # Módulo con la lógica del negocio
├── tests/
│   └── test_calculator.py  # Pruebas unitarias para calculator.py
├── pyproject.toml          # Configuración del proyecto para build (PEP 517/621)
└── .github/
    └── workflows/
        └── ci.yml          # Flujo de trabajo de GitHub Actions
```

-----

## 💻 3. Ejemplo Práctico

Este ejemplo se centra en un módulo simple de cálculo para demostrar el pipeline completo: una función matemática, una aplicación principal y sus pruebas automáticas con `pytest`.

### ✔️ 3.1 `calculator.py`

Contiene la función que se prueba y empaqueta.

```python
def add(a, b):
    """Suma dos números y devuelve el resultado."""
    return a + b
```

### ✔️ 3.2 `app.py`

Un ejemplo de cómo se utilizaría el módulo `calculator` localmente.

```python
from calculator import add

if __name__ == "__main__":
    print("Suma:", add(5, 7)) # Salida: Suma: 12
```

### ✔️ 3.3 `tests/test_calculator.py`

El archivo clave para el CI, donde se definen las pruebas unitarias.

```python
from calculator import add

def test_add():
    # Comprueba que la función add() funciona correctamente
    assert add(2, 3) == 5

# Si alguna prueba falla, el CI detiene el pipeline inmediatamente.
```

-----

## ⚙️ 4. Pipeline CI/CD (GitHub Actions)

El flujo de trabajo se define en el archivo `.github/workflows/ci.yml`.

> **Nota:** Se ha aplicado la corrección para el error de `ModuleNotFoundError` que tuviste, asegurando que `pytest` encuentre el módulo `calculator.py`.

### Contenido de `.github/workflows/ci.yml`

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

      - name: Run tests 🧪 (Corrección para imports)
        run: |
          export PYTHONPATH=$PYTHONPATH:$(pwd)
          pytest

      - name: Build Python package
        run: python -m build

      - name: Upload package artifacts
        uses: actions/upload-artifact@v4
        with:
          name: python-package
          path: dist/*
```

-----

## 🔄 5. Flujo de Ejecución del Pipeline

1.  **Activación:** Un evento (`push` o `pull_request`) inicia el flujo.
2.  **Instalación del entorno:** Se provisiona una VM Ubuntu y se configura Python 3.10.
3.  **Ejecución de pruebas (CI):** Se corre `pytest`. **Si las pruebas fallan, el pipeline termina con error.**
4.  **Construcción del Package (CD):** Si las pruebas son exitosas, se ejecuta `python -m build`, generando los archivos en `dist/`:
      * `*.tar.gz`: Source Distribution (`sdist`)
      * `*.whl`: Built Distribution (`wheel`)
5.  **Publicación de Artifacts:** GitHub Actions sube los archivos de `dist/` como un *artifact* descargable, llamado `python-package`, accesible desde la interfaz web de la acción completada.

-----

## 💻 6. Ejecución Local (Opcional)

Puedes replicar el entorno de CI en tu máquina local para probar el código antes de hacer un *push*.

### 1\. Crear y activar entorno virtual

```bash
python -m venv .venv

# Linux/macOS:
source .venv/bin/activate

# Windows PowerShell:
.venv\Scripts\activate
```

### 2\. Instalar dependencias

```bash
python -m pip install --upgrade pip
pip install pytest build
```

### 3\. Ejecutar pruebas

```bash
pytest
```

### 4\. Construir el package local

```bash
python -m build

```

-----

## ⬆️ 7. Subir al Repositorio

Si aún no lo has hecho, sigue estos pasos para inicializar tu repositorio y subir el código:

```bash
git init
git add .
git commit -m "Initial commit: CI/CD Python pipeline"
git branch -M main
git remote add origin https://github.com/TU_USUARIO/ci_cd_python.git
git push -u origin main
```

-----

## 👤 8. Autor

**Omar** — Proyecto CI/CD en Python
**Año:** 2025