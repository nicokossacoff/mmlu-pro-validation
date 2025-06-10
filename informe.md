# Evaluación de Modelos de Lenguaje en MMLU Pro

## Introducción

Esta notebook tiene como objetivo evaluar el rendimiento del modelo Gemini en la tarea MMLU Pro. Este benchmark, que es una versión mejorada y más robusta que su antecesor MMLU, nos permite medir la comprensión y el razonamiento de los modelos de lenguaje en diferentes dominios profesionales, como lo son la medicina, derecho, ingeniería, entre otros. 

Para este análisis utilizamos dos estrategias de evaluación: zero-shot y few-shot con chain-of-thought, y se comparan los resultados de Gemini con los de un modelo aleatorio.

## Metodología

### Datos Utilizados

Utilizamos el conjunto de datos MMLU Pro, el cual se compone de preguntas multiple-choice extraídas de exámenes profesionales, y que se encuentran agrupadas por categoría. Cada pregunta tiene hasta diez opciones posibles, con una única respuesta correcta.

Para la evaluación se utilizó un subconjunto representativo del conjunto completo, manteniendo una distribución balanceada en cuanto a las categorías profesionales.

### Modelos Evaluados

El modelo principal que evaluamos en este trabajo es Gemini, desarrollado por Google (en particular, utilizamos la versión 2.0 Flash Light). Como modelo baseline para la comparación utilizamos un modelo que selecciona las respuestas de forma aleatoria.

### Técnicas de Evaluación
Se aplicaron dos configuraciones principales para la evaluación:
- Zero-shot: El modelo (Gemini) responde a cada pregunta sin recibir ejemplos previos. La pregunta se presenta, y el modelo debe responder sin recibir contexto adicional.
- Few-shot con Chain-of-Thought (CoT): Aquí se incluyen ejemplos previos que le muestran al modelo cómo razonar paso a paso antes de llegar a una respuesta, con el objetivo de estimular el razonamiento del modelo. Para generar este CoT, se generó un diccionario que contenía 5 ejemplos CoT (provistos del validation set) por categoría. Para cada nueva pregunta, se busca la categoría a la que pertenece para así agregar al contexto los ejemplos de razonamiento.

En ambos casos, utilizamos el accuracy como métrica principal, es decir, la proporción de respuestas correctas sobre el total.

## Resultados

### Zero-shot

El modelo principal (Gemini) obtuvo un accuracy del 57%, lo que demuestra que tiene mucha capacidad para seleccionar la respuesta correcta incluso sin tener contexto adicional. Por otro lado, el modelo baseline (aleatorio), obtuvo un accuracy del 12% en esta tarea.

### Few-shot + Chain-of-Thought

El modelo principal mejora (marginalmente) su performance con respecto a la metodología de zero-shot con un accuracy del 58%. Esto demuestra que incorporar ejemplos y razonamiento mejora la performance, aunque esa mejora sea marginal. En este caso, se obtuvo una accuracy de 12.40% para el modelo baseline, consistente con el azar y con el resultado previo.

### Comparación contra el paper

Comparando nuestros resultados con los de [Wang et al](https://arxiv.org/abs/2406.01574), podemos observar que, en línea con lo reportado, la performance del enfoque Change-of-Thought es consistentemente superior a la de Zero-shot. Si nos enfocamos en los resultados de los modelos desarrollados por Google, se destaca que el rendimiento global de Gemini-1.5 Flash es muy cercano al rendimiento obtenido en nuestros experimentos, con un accuracy promedio de 59.1% en el paper frente a un 58% en nuestro experimento.

Sin embargo, al desagregar los resultados por categoría, surgen diferencias relevantes. En áreas como biología y psicología, los resultados obtenidos incluso superan o igualan a los reportados en [Wang et al](https://arxiv.org/abs/2406.01574), mostrando una mayor precisión en dominios relacionados con las ciencias de la vida y la conducta. No obstante, en disciplinas como leyes, negocios o física, se evidencia una brecha importante, alcanzando diferencias de hasta 20-30 puntos porcentuales.

![Accuracy por categoría](https://github.com/nicokossacoff/mmlu-pro-validation/raw/main/data/Accuracy_by_cat.png)


## Conclusiones

Los resultados que obtuvimos con nuesto análisis indican que Gemini tiene una gran capacidad para resolver tareas de razonamiento, superando ampliamente al modelo aleatorio con el que compitió. 

En [Wang et al](https://arxiv.org/abs/2406.01574) los autores utilizan un enfoque similar al nuestro: la mayoría de los modelos fueron evaluados con 5-shot + CoT, aunque algunos (como Gemini-1.5 Flash) fueron también evaluados con zero-shot. Aunque, en nuestro caso, la mejora entre el enfoque zero-shot y few-shot con chain-of-thought sea marginal (1 punto porcentual), se encuentra en línea con los resultados de [Wang et al](https://arxiv.org/abs/2406.01574), donde también se observaron mejoras en los resultados al agregar chain-of-thought.

En futuras iteraciones, sería recomendable no sólo incrementar la cantidad de modelos con los que comparar la performance, sino también afinar los prompts en aquellas categorías en las que el modelo no está respondiendo correctamente.
