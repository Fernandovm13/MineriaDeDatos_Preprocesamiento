# Declaración de Uso de IA (AI_USAGE.md)

En cumplimiento con las reglas de entrega de la materia, se detalla el uso de herramientas de Inteligencia Artificial para la resolución del **Lab 1**, del **Lab 2**, del **Lab 3** y del **Lab 4**.

## Herramienta Utilizada
- **Modelo:** Gemini 3.5

## Uso de la IA por Lab y Sección

### Lab 1 — Pipeline de preprocesamiento de texto

1. **Sección 1.a (Estadísticas del Corpus)**
   - **Consulta:** Se le preguntó a la IA cómo obtener la longitud promedio en palabras usando la sintaxis de Pandas con un split ingenuo por espacios.
   - **Resultado de la IA:** Sugirió el uso de `.str.split().str.len().mean()`.
   - **Cambios aplicados:** Se implementó tal cual en la celda correspondiente para calcular la longitud de palabras y caracteres.

2. **Sección 1.b (Detección de Ruido)**
   - **Consulta:** Se consultó sobre una expresión regular óptima en Python para detectar emojis (incluyendo caracteres unicode en planos superiores) y etiquetas HTML sin falsos positivos.
   - **Resultado de la IA:** Proporcionó los patrones `r'<[^>]+>'` y `r'[\U00010000-\U0010FFFF\u2600-\u27BF]'`.
   - **Cambios aplicados:** Se adaptaron los patrones en la celda de detección para recorrer e imprimir el ruido por cada documento.

3. **Sección 2.a (Comparación de Tokenización)**
   - **Consulta:** Se consultó la forma recomendada en spaCy para extraer el texto de los tokens a partir del objeto `Doc`.
   - **Resultado de la IA:** Indicó iterar sobre `doc` y usar `token.text`.
   - **Cambios aplicados:** Se utilizó para comparar el output con el split ingenuo y se redactaron de forma manual las diferencias observadas en puntuación y espaciado.

4. **Sección 2.b y 4 (Normalización, Stemming y Lemmatización)**
   - **Consulta:** Se solicitó a la IA la estructura típica para eliminar diacríticos (acentos) usando `unicodedata` en Python (NFD + filtrar marcas `Mn`). También se consultó cómo aplicar el lematizador de spaCy (`token.lemma_`) y el filtrado simultáneo de stopwords y signos de puntuación.
   - **Resultado de la IA:** Explicó el filtrado de caracteres usando `unicodedata.category(c) != 'Mn'` y el chequeo de stopwords.
   - **Cambios aplicados:** Se integró este comportamiento en las funciones `normalizar`, `tokens_stemming` y `tokens_lemma`.

5. **Sección 5 (Pipeline Final)**
   - **Consulta:** Se discutió conceptualmente si la lematización era mejor que el stemming para este tamaño de corpus.
   - **Resultado de la IA:** Aportó argumentos sobre la diferencia cualitativa en el idioma español.
   - **Cambios aplicados:** Se redactó la justificación en base a los números del vocabulario y se seleccionó la lematización para el pipeline `preprocesar`.

---

### Lab 2 — Su primer motor de búsqueda

1. **Sección 1 y 3 (Algoritmos núcleo TF-IDF y Similitud Coseno)**
   - **Consulta:** Se consultó a la IA por el cálculo de similitud coseno optimizado para vectores dispersos (representados en Python como diccionarios `dict`).
   - **Resultado de la IA:** Sugirió iterar únicamente sobre las claves del diccionario más pequeño para mejorar la eficiencia del producto punto.
   - **Cambios aplicados:** Se implementó esta lógica de optimización en la función `coseno(v1, v2)`.

2. **Sección 4.a (Consultas Fallidas)**
   - **Consulta:** Se discutió qué tipo de consultas darían un resultado nulo (score de 0.0) a pesar de ser semánticamente relevantes en el corpus de noticias.
   - **Resultado de la IA:** Sugirió usar "temblor en el mar" (para contrastar con "sismo" y "costa") y "computadoras rápidas" (para contrastar con "laboratorio de IA" y "GPUs").
   - **Cambios aplicados:** Se incorporaron estas consultas en el notebook y se redactaron manualmente las causas de la falla (sinonimia y brecha de abstracción conceptual).

---

### Lab 3 — BM25 y evaluación de búsqueda

1. **Parte A — Implementación de BM25 (celdas `85371372` y `5c0638ea`)**
   - **Consulta:** Se consultó a la IA la fórmula IDF suavizada de BM25 (`ln(1 + (N - df + 0.5)/(df + 0.5))`) y cómo difiere conceptualmente del IDF clásico.
   - **Resultado de la IA:** Explicó que la variante suavizada garantiza que el IDF nunca sea negativo o cero, incluso para términos que aparecen en todos los documentos.
   - **Cambios aplicados:** Se implementó `idf_bm25` siguiendo esta fórmula y se verificó manualmente que los valores fueran no-negativos para todos los términos del corpus.

2. **Parte A — Comparación lado a lado y barrido de parámetros (celdas `e2f130ee` y `0a4a48c0`)**
   - **Consulta:** Se preguntó a la IA cómo mostrar de forma clara una comparación en paralelo de dos rankings en la terminal, sin usar librerías adicionales.
   - **Resultado de la IA:** Sugirió formatear el output usando f-strings con ancho fijo (`:30`).
   - **Cambios aplicados:** Se aplicó ese formato en el bucle de impresión del top-5.

3. **Parte B — Métricas de IR desde cero (celda `25975a15`)**
   - **Consulta:** Se consultó a la IA la diferencia conceptual entre MAP y nDCG y cuándo cada una es más adecuada para evaluar un sistema de recuperación de información.
   - **Resultado de la IA:** Explicó que MAP es más simple y adecuada cuando la relevancia es binaria, mientras que nDCG es más precisa cuando la relevancia es graduada (0-3) porque pondera más los documentos altamente relevantes que aparecen en primeras posiciones.
   - **Cambios aplicados:** Se implementaron ambas métricas desde cero con las fórmulas correspondientes y se redactó la decisión final basada en nDCG@5 y MAP.

---

### Lab 4 — Descubrimiento de temas con K-Means

1. **Sección 1.a (Matriz TF-IDF + Normalización L2, celda `cacd9163`)**
   - **Consulta:** Se preguntó a la IA la razón matemática por la que se normaliza la matriz TF-IDF a norma L2 antes de aplicar K-Means.
   - **Resultado de la IA:** Explicó que la normalización L2 convierte la distancia euclidiana en equivalente a la similitud coseno, que es la métrica correcta para comparar documentos independientemente de su longitud.
   - **Cambios aplicados:** Se implementó la normalización con `np.linalg.norm` sobre las filas de la matriz y se redactó la justificación en el markdown correspondiente.

2. **Sección 2.a (K-Means desde cero + elección de k, celda `542f10c3`)**
   - **Consulta:** Se consultó a la IA cómo implementar de forma eficiente el paso de asignación de K-Means usando operaciones matriciales de NumPy (broadcasting) en lugar de bucles Python.
   - **Resultado de la IA:** Sugirió usar `X[:, np.newaxis] - centroids[np.newaxis, :]` para calcular todas las distancias en una sola operación.
   - **Cambios aplicados:** Se adoptó ese enfoque matricial en la función `kmeans`, mejorando la claridad y eficiencia del código.

3. **Sección 4.a (Análisis del documento mal agrupado, celda `be7bd5e2` y markdown `6af60cf4`)**
   - **Consulta:** Se consultó a la IA por qué documentos semánticamente similares (d02 y d13) pueden quedar en clusters distintos cuando usan distintas palabras para el mismo concepto.
   - **Resultado de la IA:** Explicó la limitación de TF-IDF como representación de bolsa de palabras sin semántica, contrastándola con embeddings como Word2Vec o BERT.
   - **Cambios aplicados:** Se redactó manualmente el análisis conectando la falla de clustering con la brecha de sinonimia y la Sesión 2 del curso.
