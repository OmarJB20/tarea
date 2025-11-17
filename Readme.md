¡Perfecto\! Entendido. He integrado la explicación detallada del archivo **`pyproject.toml`** (el punto clave que solucionó tu error) dentro de la estructura que me pasaste.

Lo he colocado en la sección **3.4**, explicando exactamente por qué lo configuramos así para tus archivos sueltos en la raíz.

Aquí tienes el `README.md` completo y corregido, listo para copiar y pegar:

-----

````markdown
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
* Construcción del paquete de Python en formato estándar (`*.tar.gz` y `*.whl`) dentro del directorio `dist/`, utilizando la configuración definida en `pyproject.toml`.

-----

## 📂 2. Estructura del Proyecto

El proyecto sigue una estructura plana donde los módulos residen en la raíz. Es vital tener el archivo de configuración correcto para que esto funcione:

```text
ci_cd_python/
├── app.py              # Aplicación principal que usa el módulo calculator
├── calculator.py       # Módulo con la lógica del negocio
├── tests/
│   └── test_calculator.py  # Pruebas unitarias para calculator.py
├── pyproject.toml      # ⚙️ Configuración CRUCIAL para el build (Solución de errores)
├── Readme.md           # Documentación del proyecto
└── .github/
    └── workflows/
        └── ci.yml      # Flujo de trabajo de GitHub Actions
````

-----

## 💻 3. Ejemplo Práctico

Este ejemplo se centra en un módulo simple de cálculo para demostrar el pipeline completo.

### ✔️ 3.1 `calculator.py`

Contiene la función lógica que se probará y empaquetará.

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

El archivo clave para el CI, donde se definen las pruebas unitarias con `pytest`.

```python
from calculator import add

def test_add():
    # Comprueba que la función add() funciona correctamente
    assert add(2, 3) == 5
```

### ✔️ 3.4 `pyproject.toml` (Configuración de Compilación)

Este archivo es **obligatorio** para construir paquetes modernos en Python. Sin él, el pipeline falla con el error: *`does not appear to be a Python project`*.

**¿Qué hace exactamente este archivo?**

1.  **Define el sistema de build:** Indica a Python que use `setuptools`.
2.  **Metadatos:** Establece el nombre (`proyecto-ci-cd`), versión y autores.
3.  **Mapeo de archivos (La parte clave):** Dado que nuestros archivos `.py` están sueltos en la raíz (y no en una carpeta `src`), usamos la directiva `py-modules` para indicar explícitamente qué archivos incluir.

<!-- end list -->

```toml
[project]
name = "proyecto-ci-cd"
version = "0.1.0"
# ... metadatos ...

[tool.setuptools]
# Le dice al constructor que empaquete estos archivos específicos
py-modules = ["app", "calculator"]
```

-----

## ⚙️ 4. Pipeline CI/CD (GitHub Actions)

El flujo de trabajo se define en el archivo `.github/workflows/ci.yml`. Se asegura de configurar el `PYTHONPATH` correctamente para las pruebas y usa `pyproject.toml` para la construcción.

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

      - name: Run tests 🧪 (Corregido el PYTHONPATH)
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
4.  **Construcción del Package (CD):** Si las pruebas pasan, el comando `build` lee el archivo `pyproject.toml` y genera:
      * `*.tar.gz`: Source Distribution (`sdist`)
      * `*.whl`: Built Distribution (`wheel`)
5.  **Publicación de Artifacts:** GitHub Actions sube los archivos de `dist/` como un *artifact* descargable.

-----

## 💻 6. Ejecución Local (Opcional)

Puedes replicar el entorno de CI en tu máquina local.

### 1\. Crear y activar entorno virtual

```bash
python -m venv .venv
# Windows:
.venv\Scripts\activate
# Linux/Mac:
source .venv/bin/activate
```

### 2\. Instalar dependencias

```bash
pip install pytest build
```

### 3\. Probar y Construir

```bash
# Tests
export PYTHONPATH=$PYTHONPATH:.  # (En Windows Powershell: $env:PYTHONPATH=".")
pytest

# Build (Requiere pyproject.toml)
python -m build
```

-----

## 👤 7. Autor

**OmarJB20** — Proyecto CI/CD en Python
**Año:** 2025

```
```