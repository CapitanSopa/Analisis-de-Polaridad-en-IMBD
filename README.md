# Análisis de Sentimiento y Polaridad en Reseñas de IMDB con VADER

Proyecto final de la asignatura APIT (Análisis y Procesamiento Inteligente de Textos), Facultad de Ingeniería, UNAM. Autores: García Lapuente Santiago y Pérez León Jesús Alexis.

## Descripción

Este proyecto implementa un pipeline modular de análisis de sentimiento sobre el corpus clásico de 50,000 reseñas de cine de IMDB, integrando los tres ejes de la asignatura: el preprocesamiento documental propio de la Recuperación de Información, el modelado léxico de polaridad con el analizador VADER y la validación supervisada contra el ground truth. El objetivo no es solo clasificar, sino evaluar críticamente hasta qué punto un modelo léxico basado en reglas resuelve el problema sobre reseñas largas y subjetivas.

El estudio aísla una única variable de diseño, el grado de preprocesamiento del texto que recibe VADER, y ejecuta dos experimentos sobre exactamente el mismo corpus depurado de 49,582 reseñas balanceadas. El enfoque A aplica preprocesamiento clásico completo (minúsculas, eliminación de puntuación, eliminación de stopwords con rescate de negaciones y lematización). El enfoque B aplica una limpieza ligera que solo retira las etiquetas HTML, preservando las mayúsculas, la puntuación y las negaciones que VADER aprovecha como señales de intensidad y polaridad. La comparación cuantifica empíricamente, en lugar de postular de forma teórica, el efecto del preprocesamiento sobre un analizador heurístico.

## Estructura del repositorio

```
analisis-sentimiento-imdb-apit/
├── README.md
├── requirements.txt
├── Analisis_Sentimiento_IMDB_V3.ipynb     # Notebook principal (núcleo del proyecto)
├── celda_mcnemar.py                       # Celda de significancia para pegar al final del notebook
├── dataset/
│   └── IMDB_Dataset.csv                    # NO incluido (66 MB). Ver "Datos" más abajo.
└── Articulo_APIT_Analisis_Sentimiento_IMDB.docx   # Artículo académico (opcional)
```

## Datos

El corpus es el dataset "IMDB Dataset of 50K Movie Reviews", publicado originalmente por Lakshmipathi N en Kaggle y construido a partir del conjunto de Maas et al. (2011). Tiene dos columnas: `review`, con el texto libre de la reseña en inglés, y `sentiment`, con la etiqueta real `positive` o `negative`.

El archivo no se incluye en el repositorio por su tamaño (66 MB) y por su licencia. Para obtenerlo basta con descargarlo a la carpeta `dataset/` con el nombre que espera el notebook. Desde un espejo público en GitHub, el comando es el siguiente.

```bash
mkdir -p dataset
curl -L "https://raw.githubusercontent.com/Ankit152/IMDB-sentiment-analysis/master/IMDB-Dataset.csv" -o dataset/IMDB_Dataset.csv
```

El notebook está diseñado para ser robusto: si no encuentra el CSV en `dataset/IMDB_Dataset.csv`, recurre a un pequeño bloque de datos simulados y avisa de ello, de modo que las celdas siguen ejecutándose aunque el dataset no esté presente.

## Cómo reproducir

Tras clonar el repositorio y situarse en su carpeta, el procedimiento completo consiste en instalar las dependencias, descargar el dataset como se indicó arriba y ejecutar el notebook de principio a fin. Los recursos lingüísticos de NLTK (el lexicón `vader_lexicon`, las `stopwords`, `wordnet`, `omw-1.4` y `punkt`) se descargan automáticamente desde la primera celda del notebook, por lo que no requieren instalación manual.

```bash
pip install -r requirements.txt
jupyter notebook Analisis_Sentimiento_IMDB_V3.ipynb
```

El procedimiento es determinista: ni VADER ni el preprocesamiento introducen aleatoriedad, de modo que no se requieren semillas y cada ejecución reproduce siempre los mismos resultados.

## Resultados esperados

Si la reproducción es correcta, las exactitudes globales sirven como prueba de cordería. El enfoque A (texto preprocesado) alcanza una exactitud de 0.6814 y el enfoque B (texto crudo) de 0.6992, una diferencia de 1.78 puntos porcentuales a favor de no preprocesar. El modelo final adoptado es el B, cuya matriz de confusión es [[13391, 11307], [3606, 21278]] con el orden de clases negativa y positiva.

La diferencia entre ambos enfoques se contrasta con la prueba de McNemar sobre las predicciones pareadas, implementada en `celda_mcnemar.py`. De las 49,582 reseñas hay 5,596 casos discordantes: el enfoque B recupera 3,240 reseñas que A clasificaba mal y solo deteriora 2,356 que A acertaba, una mejora neta de 884 reseñas. El estadístico de McNemar con corrección de continuidad es de 139.33 con un grado de libertad, lo que da un valor p de aproximadamente 3.7 × 10⁻³² (la prueba binomial exacta da 2.8 × 10⁻³²). La diferencia es, por tanto, altamente significativa y no atribuible al azar.

Para reproducir este último cálculo, basta con pegar el contenido de `celda_mcnemar.py` como una nueva celda al final del notebook, una vez que el DataFrame `df` ya contiene las columnas `sentiment`, `sent_limpia` y `pred_sentiment`.

## Entorno verificado

La reproducción se verificó con Python 3.12 y las versiones pandas 3.0.2, numpy 2.4.4, nltk 3.9.4, scikit-learn 1.8.0, scipy 1.17.1, matplotlib 3.10.8 y seaborn 0.13.2. Como los resultados numéricos dependen del lexicón `vader_lexicon` de NLTK, que es estable entre versiones, se reproducen en un rango amplio de versiones de las demás bibliotecas, incluidas las disponibles por defecto en Google Colaboratory.

## Referencias principales

Hutto, C. J., y Gilbert, E. (2014). VADER: A parsimonious rule-based model for sentiment analysis of social media text. ICWSM-14. Maas, A. L., Daly, R. E., Pham, P. T., Huang, D., Ng, A. Y., y Potts, C. (2011). Learning Word Vectors for Sentiment Analysis. ACL. La bibliografía completa se encuentra en el artículo académico que acompaña al repositorio.
