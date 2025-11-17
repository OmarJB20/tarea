🚀 Ejemplo Completo del Ciclo CI/CD con Python + GitHub Actions
Construcción de Package + Pruebas + Artefactos

Este repositorio explica paso a paso cómo funciona un pipeline CI/CD utilizando GitHub Actions, desde el push del código hasta la generación de un package de Python (sdist y wheel).
Incluye ejemplo práctico, archivo de workflow funcional, pruebas unitarias y artefactos generados automáticamente.

📌 1. ¿Qué es CI/CD?
✔ CI (Integración Continua)

Proceso automático que se ejecuta cada vez que subimos código al repositorio. Incluye actividades como:

Descargar el repositorio

Instalar dependencias

Ejecutar pruebas unitarias

Validar que el código funcione correctamente

✔ CD (Entrega Continua)

Consiste en generar automáticamente un package o artefacto listo para distribuir o desplegar.
En este proyecto, se construye el package en formato estándar de Python (*.tar.gz y *.whl) dentro de dist/.

📌 2. Estructura del Proyecto
ci_cd_python/
├── app.py
├── calculator.py
├── tests/
│   └── test_calculator.py
├── pyproject.toml
└── .github/
    └── workflows/
        └── ci.yml

📌 3. Explicación del Ejemplo Práctico

Este proyecto incluye:

Una función matemática simple

Una app principal

Pruebas automáticas con pytest

✔ 3.1 Archivo calculator.py
def add(a, b):
    """Suma dos números y devuelve el resultado."""
    return a + b

✔ 3.2 Archivo app.py
from calculator import add

if __name__ == "__main__":
    print("Suma:", add(5, 7))

✔ 3.3 Archivo de pruebas tests/test_calculator.py
from calculator import add

def test_add():
    assert add(2, 3) == 5


Comprueba que la función add() funciona correctamente

Si falla → el CI detiene el pipeline

📌 4. Pipeline CI/CD (GitHub Actions)

Archivo: .github/workflows/ci.yml

Contenido:
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

      - name: Run tests
        run: pytest

      - name: Build Python package
        run: python -m build

      - name: Upload package artifacts
        uses: actions/upload-artifact@v4
        with:
          name: python-package
          path: dist/*

📌 5. Cómo funciona el pipeline

Push al repositorio: Cada vez que haces git push, GitHub ejecuta el workflow.

Instalación del entorno: Crea una VM Ubuntu y descarga el repositorio.

Ejecución de pruebas: Corre pytest. Si falla → pipeline detenido.

Construcción del package: Ejecuta python -m build → genera dist/ con:

*.tar.gz → source distribution

*.whl → wheel

Publicación de artifacts: GitHub Actions guarda los archivos de dist/ como artifacts descargables.

📌 6. Ejecución local

Crear y activar entorno virtual:

python -m venv .venv
# Linux/macOS:
source .venv/bin/activate
# Windows PowerShell:
.venv\Scripts\activate


Instalar dependencias:

python -m pip install --upgrade pip
pip install pytest build


Ejecutar pruebas:

pytest


Construir el package local:

python -m build
# Archivos generados en dist/: .tar.gz y .whl

📌 7. Subir al repositorio
git init
git add .
git commit -m "Initial commit: CI/CD Python pipeline"
git branch -M main
git remote add origin https://github.com/TU_USUARIO/ci_cd_python.git
git push -u origin main


📌 8. Autor

Omar — Proyecto CI/CD en Python
Año: 2025