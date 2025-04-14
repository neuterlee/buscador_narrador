# Buscador y Narrador de Casos de Desaparecidos

Este repositorio contiene un conjunto de herramientas desarrolladas en Python (utilizando Jupyter Notebooks) para procesar, anonimizar y buscar información relacionada con casos de personas desaparecidas. Las herramientas principales se enfocan en:

1.  **Creación de Narrativas:** Transformar descripciones fácticas de desapariciones en historias breves y anonimizadas utilizando IA (DeepSeek).
2.  **Búsqueda por Similitud:** Encontrar casos similares dentro de una base de datos utilizando embeddings de texto y búsqueda semántica.

---

## 📂 Estructura del Repositorio
buscador_narrador/
├── creador_historias/
│   ├── input_folder/       # <- Coloca aquí los CSV de entrada para crear historias
│   ├── output/             # <- Aquí se guardan los CSV/HTML generados por creador_historias y tets_historias
│   ├── creador_historias.ipynb # Script principal para anonimizar y crear una historia por caso
│   └── tets_historias.ipynb    # Script para generar MÚLTIPLES variaciones de historias por caso y visualización
├── busqueda_embebidos/
│   ├── data/               # <- Coloca aquí el CSV principal para la búsqueda y se guardan cachés
│   ├── output/             # <- Aquí se guardan los resultados de búsqueda (HTML/CSV)
│   └── busqueda_embebidos.ipynb # Script para buscar casos por similitud semántica
└── readme.md               # Este archivo

---

## 🛠️ Componentes Principales

### 1. Creador de Historias (`creador_historias/`)

Esta sección contiene scripts para procesar archivos CSV con datos de personas desaparecidas y generar narrativas anonimizadas.

#### 1.1. `creador_historias.ipynb`

* **Propósito**: Lee un archivo CSV, filtra casos específicos (condición "NO APLICA"), y utiliza la API de DeepSeek para reescribir la descripción de la desaparición como una historia breve, sensible y anonimizada.
* **Características**:
    * **Filtrado**: Procesa solo casos donde `condicion_localizacion` es "NO APLICA".
    * **Anonimización**:
        * Reformula la descripción como una narrativa corta.
        * Intenta eliminar datos sensibles (direcciones, placas).
        * Anonimiza nombres de terceros, pero **conserva** el nombre de la persona desaparecida.
    * **Salida**: Guarda los casos procesados (con la nueva columna `descripcion_anonimizada`) en un nuevo archivo CSV dentro de la carpeta `output/`, con un nombre que incluye fecha y hora.
* **Cómo Usarlo**:
    1.  Coloca tu archivo(s) CSV fuente en la carpeta `creador_historias/input_folder/`.
    2.  Ejecuta el notebook `creador_historias.ipynb`.
    3.  Si hay varios CSV, te pedirá seleccionar uno.
    4.  Ingresa el número de casos que deseas procesar (seleccionará aleatoriamente si pides menos del total filtrado).
    5.  El archivo resultante se guardará en `creador_historias/output/`.
* **Requisitos**:
    * Librerías Python: `openai`, `os`, `csv`, `random`, `logging`, `datetime`.
    * Una clave de API válida para DeepSeek (reemplaza `"sk-xxx"` en el código).

#### 1.2. `tets_historias.ipynb`

* **Propósito**: Similar a `creador_historias.ipynb`, pero genera **múltiples** variaciones narrativas (9 estilos diferentes predefinidos, como melancólico, estilo Rulfo, poético, etc.) para cada caso procesado. También genera una visualización HTML.
* **Características**:
    * **Filtrado**: Igual que `creador_historias.ipynb`.
    * **Generación Múltiple**: Aplica 9 prompts distintos a la API de DeepSeek para cada caso, obteniendo diversas narrativas.
    * **Salida**:
        * Guarda un archivo CSV en `creador_historias/output/` (`processed_cases_multi_prompt_*.csv`) con columnas para cada estilo de historia.
        * Genera un archivo HTML (`visualizacion_casos_*.html`) en `creador_historias/output/` para comparar fácilmente las diferentes narrativas generadas para cada caso.
* **Cómo Usarlo**:
    1.  Coloca tu archivo(s) CSV fuente en `creador_historias/input_folder/`.
    2.  Ejecuta el notebook `tets_historias.ipynb`.
    3.  Selecciona el archivo CSV si es necesario.
    4.  Ingresa el número de casos a procesar.
    5.  Los archivos CSV y HTML resultantes se guardarán en `creador_historias/output/`.
* **Requisitos**:
    * Librerías Python: `openai`, `pandas`, `os`, `csv`, `random`, `logging`, `datetime`, `time`, `html`.
    * Una clave de API válida para DeepSeek (reemplaza `"sk-xxx"` en el código).

### 2. Búsqueda por Embeddings (`busqueda_embebidos/`)

Esta sección permite buscar casos similares basados en el contenido semántico de sus descripciones.

#### 2.1. `busqueda_embebidos.ipynb`

* **Propósito**: Implementa un sistema de búsqueda que utiliza embeddings densos (generados con el modelo `BAAI/bge-m3`) para encontrar las descripciones más similares a una consulta dada por el usuario.
* **Características**:
    * **Filtrado Opcional**: Permite filtrar la base de datos inicial (por ejemplo, solo casos "NO APLICA") antes de generar los embeddings.
    * **Generación de Embeddings**: Crea representaciones vectoriales (embeddings) de las descripciones. Utiliza caché (`.pkl`) para evitar recalcular si el archivo fuente no ha cambiado.
    * **Búsqueda Semántica**: El usuario introduce una consulta en lenguaje natural. El script calcula la similitud coseno entre el embedding de la consulta y los embeddings de la base de datos.
    * **Salida**:
        * Genera un archivo HTML en `busqueda_embebidos/output/` (`search_results_*.html`) mostrando los resultados más relevantes (top-k) con su puntuación de similitud.
        * Exporta las filas completas correspondientes a los resultados encontrados a un archivo CSV en `busqueda_embebidos/output/` (`filtered_results_*.csv`).
* **Cómo Usarlo**:
    1.  Coloca tu archivo CSV principal (ej: `repd_vp_cedulas_principal.csv`) en la carpeta `busqueda_embebidos/data/`.
    2.  Ejecuta el notebook `busqueda_embebidos.ipynb`.
    3.  Te preguntará si deseas aplicar un filtro inicial (responde `yes` o `no`).
    4.  Una vez generados o cargados los embeddings, te pedirá ingresar tu consulta de búsqueda y cuántos resultados deseas ver.
    5.  Introduce tu consulta (o escribe `exit` para salir).
    6.  Los archivos HTML y CSV con los resultados se guardarán en `busqueda_embebidos/output/` con nombres que incluyen fecha y hora.
* **Requisitos**:
    * Librerías Python: `pandas`, `torch`, `sentence-transformers`, `FlagEmbedding`, `hashlib`, `pickle`, `jinja2`, `tqdm`, `numpy`, `csv`, `datetime`.
    * **GPU (Recomendado)**: La generación de embeddings es computacionalmente intensiva y se beneficia enormemente de una GPU compatible con CUDA. El script intenta usarla si está disponible.

---