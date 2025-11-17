🚀 Ejemplo Completo del Ciclo CI/CD con Python + GitHub Actions
Construcción de Package + Pruebas + Artefactos

Este repositorio explica paso a paso cómo funciona un pipeline CI/CD utilizando GitHub Actions, desde el push del código hasta la generación de un package (.zip).
Incluye ejemplo práctico, archivo de workflow funcional, pruebas unitarias y artefactos generados automáticamente.

📌 1. ¿Qué es CI/CD?
✔ CI (Integración Continua)

Es el proceso automático que se ejecuta cada vez que subimos código al repositorio. Incluye actividades como:

Descargar el repositorio

Instalar dependencias

Ejecutar pruebas

Validar que el código esté correcto

✔ CD (Entrega Continua)

Consiste en generar automáticamente un package/artefacto listo para distribuir o desplegar.

En este ejemplo, el pipeline genera un archivo:

build.zip


Este archivo contiene el proyecto empaquetado como salida final del pipeline.

📌 2. Estructura del Proyecto
ci_cd_python/
├── app.py
├── calculator.py
├── tests/
│   └── test_calculator.py
└── .github/
    └── workflows/
        └── ci.yml

📌 3. Explicación del Ejemplo Práctico

Este proyecto incluye una pequeña función matemática, una app principal y pruebas automáticas.

✔ 3.1 Archivo calculator.py
def add(a, b):
    return a + b


Función simple usada como ejemplo para el pipeline CI/CD.

✔ 3.2 Archivo app.py
from calculator import add

if __name__ == "__main__":
    print("Suma:", add(5, 7))


Programa principal que usa la función add().

✔ 3.3 Archivo de pruebas tests/test_calculator.py
from calculator import add

def test_add():
    assert add(2, 3) == 5


Prueba unitaria que:

Llama a la función add()

Verifica que la función retorna el resultado correcto

Si esta prueba falla → el CI detiene el pipeline.

📌 4. Pipeline CI/CD (GitHub Actions)

El archivo del workflow se encuentra en:

.github/workflows/ci.yml


Este archivo ejecuta:

Descarga del repositorio

Instalación de Python

Instalación de dependencias

Ejecución de pruebas

Construcción del package (.zip)

Publicación del artefacto

✔ 4.1 Contenido del workflow ci.yml
name: CI Pipeline

on:
  push:
  pull_request:

jobs:
  build-test:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3

    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.10'

    - name: Install dependencies
      run: |
        pip install pytest

    - name: Run tests
      run: pytest

    - name: Build package artifact
      run: zip -r build.zip .
      shell: bash

    - name: Upload artifact
      uses: actions/upload-artifact@v3
      with:
        name: build
        path: build.zip

📌 5. ¿Cómo Funciona el Pipeline?
🔹 1. Push al repositorio

Cada vez que haces:

git add .
git commit -m "mensaje"
git push


GitHub ejecuta automáticamente el workflow.

🔹 2. Instalación del entorno

El job crea una máquina virtual Ubuntu y descarga el código del repositorio.

🔹 3. Ejecución de pruebas

El workflow ejecuta:

pytest


✔ Si las pruebas pasan → el pipeline continúa
✘ Si una prueba falla → el pipeline se detiene

🔹 4. Construcción del package

El pipeline genera un archivo comprimido:

build.zip


Que incluye TODO el proyecto.

🔹 5. Publicación del artefacto

El archivo generado se sube automáticamente a GitHub Actions → Artifacts
Desde ahí puedes descargarlo.

📌 6. Ejecución local
python app.py
pytest

📌 7. ¿Qué debes subir a tu repositorio?

Debes crear un repositorio en GitHub y subir:

✔ README.md
✔ calculator.py
✔ app.py
✔ tests/test_calculator.py
✔ .github/workflows/ci.yml



📌 8. Autor

Omar — Proyecto CI/CD en Python
Año: 2025