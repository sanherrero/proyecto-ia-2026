# Proyecto IA 2026

Proyecto de Aprendizaje Automático con Árboles de Decisión y Redes Neuronales.
Trabajo de Lautaro Coñoen y Santiago Herrero.

Descripción General del Proyecto
Este proyecto es un sistema de Aprendizaje Automático (Machine Learning) enfocado en la clasificación de películas en tres categorías de calidad: "Mala", "Buena" y "Muy buena". El objetivo principal es entrenar y comparar dos arquitecturas de modelos distintas: un Árbol de Decisión y una Red Neuronal Artificial (RNA), utilizando un conjunto de datos con características diversas de las películas.

Etapas Clave del Proyecto
Carga y Exploración de Datos:

Se cargan dos conjuntos de datos: Entrenamiento (movies_train.csv de 12,500 filas) y Prueba (movies_test_actualizado.csv de 500 filas).
El dataset cuenta con 25 columnas que incluyen información técnica, financiera e histórica de los actores principales (popularity_p1, number_movies_p1, etc.) y el director.
Se analiza la distribución de las clases, identificando que la categoría más común es "Buena" (46.15%), seguida de "Mala" (38.46%) y "Muy buena" (~15.38%).
Procesamiento y Limpieza de Datos:

Eliminación de Columnas Irrelevantes o Ruidosas: Se quitan identificadores únicos y nombres textuales como movieId, title, y nombres de actores/directores para que no afecten al entrenamiento.
Tratamiento de Valores Nulos: Los datos faltantes en duración (runtime) y popularidad del director se rellenan con sus respectivos promedios, mientras que los ingresos (revenue) se completan con 0 y los géneros faltantes con 'Unknown'.
Conversión de Atributos Categóricos:
belong_to_collection se convierte a un formato binario entero (0 o 1).
La clase objetivo (class) se mapea ordinalmente: Mala -> 0, Buena -> 1 y Muy buena -> 2.
El género (genre) se transforma mediante One-Hot Encoding en columnas binarias individuales por cada género posible.
Modelo 1: Red Neuronal Artificial (Keras/TensorFlow):

Los datos numéricos se estandarizan usando MinMaxScaler.
Se diseña una red neuronal secuencial con:
Una capa de entrada de 128 neuronas (activación ReLU).
Capas ocultas de 64 y 32 neuronas con regularización de tipo Dropout (30%) para evitar que el modelo memorice los datos en exceso (sobreajuste).
Una capa de salida con 3 neuronas utilizando la función de activación Softmax para clasificar probabilísticamente en las tres categorías.
Se aplican pesos de clase balanceados para compensar el desequilibrio de la categoría "Muy buena".
Se implementa Early Stopping para detener el entrenamiento cuando el error de validación deja de mejorar.
Rendimiento de la RNA: Alcanza un 71% de precisión (Accuracy) global sobre el conjunto de validación.
Modelo 2: Árbol de Decisión (Scikit-Learn):

Se entrena un clasificador basado en un árbol de decisión (DecisionTreeClassifier).
Se realiza una optimización paso a paso de sus hiperparámetros mediante validación cruzada simulada:
Profundidad Máxima (max_depth): Se evalúan profundidades de 1 a 50, concluyendo que la mejor profundidad es 8 (alcanzando 70% de precisión).
Criterio de Partición (criterion): Se determina que el criterio Gini ofrece mejor rendimiento que la entropía.
Tamaño mínimo de división (min_samples_split): El óptimo es de 50 muestras por nodo.
Rendimiento del Árbol: Consigue un 70% de precisión (Accuracy) global sobre el conjunto de test (holdout).
El modelo permite extraer la importancia de las variables, identificando las 10 características que más influyen en el éxito de una película (ej. presupuesto, ingresos, popularidad).
Predicción Final y Exportación:

Ambos modelos ya optimizados generan predicciones de las 500 películas nuevas del conjunto de test.
Estas predicciones se convierten de nuevo a etiquetas de texto y se guardan como columnas adicionales en el DataFrame de salida.
