# 🌟 Proyecto Final: Sistema de Búsqueda Semántica para Artículos Científicos

## 📚 Introducción

Este proyecto implementa un sistema de búsqueda semántica para artículos científicos utilizando el dataset de arXiv. El sistema permite realizar consultas en lenguaje natural y obtener los artículos más relevantes basados en la similitud de embeddings generados por un modelo de lenguaje preentrenado. Además, se incluyen métricas para evaluar la calidad de las búsquedas y se visualizan los embeddings utilizando PCA.

## 🛠️ Instrucciones de instalación

### Prerrequisitos

1. 🐍 Python 3.6 o superior
2. 🔑 Una cuenta en Kaggle para descargar el dataset de arXiv
3. 📝 Google Colab (opcional, pero recomendado)

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

## 📖 Guía de uso

1. Abre el notebook `notebook.ipynb` en Google Colab o en tu entorno de Jupyter Notebook preferido.

2. Ejecuta cada celda del notebook en orden. El código está dividido en las siguientes secciones:
    - **Recolección de Datos**: Descarga y carga del dataset de arXiv.
    - **Preprocesamiento**: Limpieza y normalización del texto, y tokenización de los artículos.
    - **Generación de Embeddings**: Uso de un modelo preentrenado para generar embeddings de los artículos.
    - **Búsqueda Semántica**: Implementación de una función de búsqueda que utiliza similitud coseno para encontrar los artículos más relevantes.
    - **Visualización**: Uso de PCA para reducir la dimensionalidad de los embeddings y visualizarlos en 2D y 3D.

3. Para realizar una búsqueda, modifica la variable `query` en la celda correspondiente y ejecuta la celda para ver los resultados.

4. **Interfaz gráfica para buscar resultados**: Una interfaz gráfica que permite al usuario elegir entre la función de búsqueda normal y la búsqueda con FAISS.
   ```python
   import ipywidgets as widgets
   from IPython.display import display

   # Cargar los embeddings (si no están cargados)
   if 'embeddings' not in df.columns:
       df = pd.read_pickle('arxiv_embeddings.pkl')

   # Función de búsqueda y similitud
   def search(query, df, top_k=5):
       query_embedding = get_embeddings(query)  # Obtener el embedding de la consulta
       similarities = cosine_similarity([query_embedding], list(df['embeddings']))[0]
       df['similarity'] = similarities  # similitud por coseno
       df['arxiv_url'] = 'https://arxiv.org/abs/0' + df['id'].astype(str)
       results = df.sort_values(by='similarity', ascending=False).head(top_k)
       return results

   # Función de búsqueda FAISS
   def busqueda_faiss(consulta, df, top_k=5):
       # Asegurarse de que los embeddings son arrays de NumPy
       if not isinstance(df['embeddings'].iloc[0], np.ndarray):
           df['embeddings'] = df['embeddings'].apply(np.array)

       # Crear el índice FAISS
       index = faiss.IndexFlatIP(df['embeddings'].iloc[0].shape[0])
       index.add(np.stack(df['embeddings']))

       # Obtener el embedding de la consulta
       consulta_embedding = get_embeddings(consulta)

       # Realizar la búsqueda
       distances, indices = index.search(np.array([consulta_embedding]), k=top_k)

       # Obtener los resultados
       resultados = df.iloc[indices[0]]
       resultados['distancia_faiss'] = distances[0]
       resultados = resultados.sort_values('distancia_faiss', ascending=True)

       return resultados

   # Widget para la entrada de texto
   text_input = widgets.Text(
       value='',
       placeholder='Ingrese su consulta',
       description='Consulta:',
       disabled=False
   )

   # Widget para seleccionar el método de búsqueda
   dropdown = widgets.Dropdown(
       options=['Cosine Similarity', 'FAISS'],
       value='Cosine Similarity',
       description='Método:',
       disabled=False,
   )

   # Widget para el botón de búsqueda
   button = widgets.Button(
       description='Buscar',
       disabled=False,
       button_style='info',
       tooltip='Click para buscar',
       icon='search'
   )

   # Función para manejar el evento de clic en el botón
   def on_button_clicked(b):
       query = text_input.value
       if dropdown.value == 'Cosine Similarity':
           results = search(query, df)
       else:
           results = busqueda_faiss(query, df)
       display(results)

   # Asignar la función al evento de clic del botón
   button.on_click(on_button_clicked)

   # Mostrar los widgets
   display(text_input, dropdown, button)
   ```
5. **Visualización**: Utiliza PCA para visualizar los embeddings en 2D y 3D.


## 📂 Carpeta `data`

La carpeta `data` contiene un fragmento del dataset para facilitar pruebas y desarrollo. Asegúrate de cargar este fragmento en tu entorno de trabajo para ejecutar el notebook si no deseas descargar el dataset completo.

## 🔗 Enlace al notebook en Google Colab

[![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/JoseADios/SemanticSearchPython/blob/main/SemanticSearch.ipynb)

## 🎥 Enlace al video explicativo

[Enlace al video](https://drive.google.com/file/d/19G0y2eF87CUkK0CcZR_EhdTF2H2NJFHP/view?usp=sharing)

## 📂 Recursos Adicionales
- [Carpeta de Google Drive con todos los ejercicios de la asignatura](https://drive.google.com/drive/folders/1bObllwqdMiCxQxY-UkQ8R4dOYcY_V6kv?usp=sharing)


---

Este proyecto fue desarrollado como parte del curso de Inteligencia Artificial, dirigido por el Profesor Lizandro Ramírez.

