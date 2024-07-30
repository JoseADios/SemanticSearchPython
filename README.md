# Proyecto Final: Sistema de Búsqueda Semántica para Artículos Científicos

## Introducción

Este proyecto implementa un sistema de búsqueda semántica para artículos científicos utilizando el dataset de arXiv. El sistema permite realizar consultas en lenguaje natural y obtener los artículos más relevantes basados en la similitud de embeddings generados por un modelo de lenguaje preentrenado. Además, se visualizan los embeddings utilizando PCA.

## Instrucciones de instalación

### Prerrequisitos

1. Python 3.6 o superior
2. Una cuenta en Kaggle para descargar el dataset de arXiv
3. Google Colab (opcional, pero recomendado)

### Instalación

1. Clonar el repositorio:
    ```bash
    git clone <URL_DEL_REPOSITORIO>
    cd <NOMBRE_DEL_REPOSITORIO>
    ```

2. Instalar las dependencias necesarias:
    ```bash
    pip install -r requirements.txt
    ```

3. Descargar el dataset de arXiv desde Kaggle en Google Colab:
    ```python
    from google.colab import userdata
    userdata.get('KAGGLE_API_KEY')

    !mkdir ~/.kaggle
    !echo userdata.get('KAGGLE_API_KEY') > ~/.kaggle/kaggle.json
    !chmod 600 ~/.kaggle/kaggle.json
    !kaggle datasets download -d Cornell-University/arxiv
    !echo "Y" | unzip arxiv.zip
    ```

## Guía de uso

1. Abre el notebook `notebook.ipynb` en Google Colab o en tu entorno de Jupyter Notebook preferido.

2. Ejecuta cada celda del notebook en orden. El código está dividido en las siguientes secciones:
    - **Recolección de Datos**: Descarga y carga del dataset de arXiv.
    - **Preprocesamiento**: Limpieza y normalización del texto, y tokenización de los artículos.
    - **Generación de Embeddings**: Uso de un modelo preentrenado para generar embeddings de los artículos.
    - **Búsqueda Semántica**: Implementación de una función de búsqueda que utiliza similitud coseno para encontrar los artículos más relevantes.
    - **Visualización**: Uso de PCA para reducir la dimensionalidad de los embeddings y visualizarlos en 2D y 3D.

3. Para realizar una búsqueda, modifica la variable `query` en la celda correspondiente y ejecuta la celda para ver los resultados.

4. Incluye métricas para evaluar la calidad de las búsquedas, como precisión y recuperación, y realiza pruebas con diferentes consultas para demostrar la efectividad del sistema.

## Enlace al notebook en Google Colab

[Abrir en Colab](https://colab.research.google.com/github/JoseADios/SemanticSearchPython/blob/main/SemanticSearch.ipynb)

## Enlace al video explicativo

[Enlace al video](https://www.youtube.com/watch?v=dQw4w9WgXcQ)

---

Este proyecto fue desarrollado como parte de la asignatura de Inteligencia Artificial, dirigido por el Profesor Lizandro Ramírez.
