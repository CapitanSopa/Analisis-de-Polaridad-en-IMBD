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
├── Analisis_Sentimiento_IMDB.ipynb    
├── dataset/
│   └── IMDB_Dataset.csv                    # NO incluido (66 MB). Ver "Datos" más abajo.
```

## Datos

El corpus es el dataset "IMDB Dataset of 50K Movie Reviews", publicado originalmente por Lakshmipathi N en Kaggle. Tiene dos columnas: `review`, con el texto libre de la reseña en inglés, y `sentiment`, con la etiqueta real `positive` o `negative`.

El archivo no se incluye en el repositorio por su tamaño (66 MB) y por su licencia. Para obtenerlo basta con descargarlo a la carpeta `dataset/` con el nombre que espera el notebook. 

El notebook está diseñado para ser robusto: si no encuentra el CSV en `dataset/IMDB_Dataset.csv`, recurre a un pequeño bloque de datos simulados y avisa de ello, de modo que las celdas siguen ejecutándose aunque el dataset no esté presente.

## Entorno verificado

La reproducción se verificó con Python 3.12 y las versiones pandas 3.0.2, numpy 2.4.4, nltk 3.9.4, scikit-learn 1.8.0, scipy 1.17.1, matplotlib 3.10.8 y seaborn 0.13.2. Como los resultados numéricos dependen del lexicón `vader_lexicon` de NLTK, que es estable entre versiones, se reproducen en un rango amplio de versiones de las demás bibliotecas, incluidas las disponibles por defecto en Google Colaboratory.

## Referencias principales

Hutto, C. J., y Gilbert, E. (2014). VADER: A parsimonious rule-based model for sentiment analysis of social media text. ICWSM-14. Maas, A. L., Daly, R. E., Pham, P. T., Huang, D., Ng, A. Y., y Potts, C. (2011). Learning Word Vectors for Sentiment Analysis. ACL. La bibliografía completa se encuentra en el artículo académico que acompaña al repositorio.
