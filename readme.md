# Creador de Historias y Buscador de Casos

Este repositorio contiene herramientas para procesar y analizar datos relacionados con personas desaparecidas. Incluye dos scripts principales: uno para anonimizar descripciones de casos y otro para buscar similitudes en una base de datos utilizando embeddings densos.

---

## **1. creador_historias.ipynb**

### **Descripción**
Este script procesa un archivo CSV con información de personas desaparecidas, anonimiza las descripciones utilizando la API de DeepSeek y guarda los resultados en un nuevo archivo CSV.

### **Características**
- **Filtrado de Casos**: Procesa únicamente los casos donde `condicion_localizacion` es "NO APLICA".
- **Anonimización**: 
  - Reescribe las descripciones como historias breves y sensibles.
  - Elimina datos sensibles como números de casa o placas de auto.
  - Anonimiza nombres de terceros, pero mantiene el nombre de la persona desaparecida.
- **Resultados**: Guarda los casos procesados en un archivo CSV con un nombre que incluye la fecha y hora actuales.

### **Cómo Usarlo**
1. Coloca los archivos CSV en la carpeta `input_folder`.
2. Ejecuta el script.
3. Selecciona el archivo CSV (si hay varios).
4. Ingresa el número de casos a procesar.
5. Los resultados se guardarán en la carpeta `output`.

### **Requisitos**
- Clave de API válida para DeepSeek.
- Librerías necesarias: `os`, `csv`, `random`, `logging`, `datetime`, `openai`.

---

## **2. busqueda_embebidos.py**

### **Descripción**
Este script permite buscar similitudes en una base de datos de descripciones de desapariciones utilizando embeddings densos generados con el modelo `BGEM3FlagModel`.

### **Características**
- **Filtrado Opcional**: Permite filtrar la base de datos antes de generar embeddings.
- **Generación de Embeddings**: Utiliza el modelo `BGEM3FlagModel` para generar representaciones densas de las descripciones.
- **Búsqueda por Similitud**: Encuentra las descripciones más similares a una consulta utilizando similitud coseno.
- **Resultados**:
  - Genera un archivo HTML con los resultados de la búsqueda.
  - Exporta los resultados a un archivo CSV.

### **Cómo Usarlo**
1. Coloca el archivo CSV en la carpeta `data`.
2. Ejecuta el script.
3. Opcionalmente, aplica un filtro a la base de datos.
4. Ingresa una consulta y el número de resultados a mostrar.
5. Los resultados se guardarán en la carpeta `output`.

### **Requisitos**
- Librerías necesarias: `pandas`, `torch`, `sentence_transformers`, `hashlib`, `pickle`, `jinja2`.

---

## **Notas Importantes**
- **Privacidad**: Ambos scripts están diseñados para manejar datos sensibles. Asegúrate de cumplir con las normativas de privacidad aplicables.
- **Clave de API**: Reemplaza la clave de API en `creador_historias.ipynb` con una válida para DeepSeek.
- **GPU**: Si usas `busqueda_embebidos.py`, asegúrate de tener una GPU disponible para acelerar la generación de embeddings.

---

## **Estructura del Proyecto**