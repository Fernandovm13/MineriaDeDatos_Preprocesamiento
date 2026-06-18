# Declaración de Uso de IA (AI_USAGE.md)

En cumplimiento con las reglas de entrega de la materia, se detalla el uso de herramientas de Inteligencia Artificial para la resolución del **Lab 1** y del **Lab 2**.

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
