# Evaluación de Modelos de Lenguaje en MMLU Pro

## Introducción

Esta notebook tiene como objetivo evaluar el rendimiento del modelo Gemini en la tarea MMLU Pro. Este es un benchmark, que es una versión mejorada y más robusta que su antecesor MMLU, nos permite medir la comprensión y el razonamiento de los modelos de lenguaje en diferentes dominios profesionales, como lo son la medicina, derecho, ingeniería, entre otros. 

Para este análisis utilizamos dos estrategias de evaluación: zero-shot y few-shot con chain-of-thought, y se comparan los resultados de Gemini con los de un modelo aleatorio.

## Metodología

### Datos Utilizados

Utilizamos el conjunto de datos MMLU Pro, el cual se compone de preguntas multiple-choice extraídas de exámenes profesionales, y agrupadas por categoría. Cada pregunta tiene hasta diez opciones posibles, con una única respuesta correcta.

Para la evaluación se utilizó un subconjunto representativo del conjunto completo, manteniendo una distribución balanceada en cuanto a las categorías profesionales.

### Modelos Evaluados

El modelo principal que evaluamos en este trabajo es Gemini, desarrollado por Google (en particular, utilizamos la versión 2.0 Flash Light). Como modelo baseline para la comparación utilizamos un modelo que selecciona las respuestas de forma aleatoria.

### Técnicas de Evaluación
Se aplicaron dos configuraciones principales para la evaluación:
- Zero-shot: El modelo (Gemini) responde a cada pregunta sin recibir ejemplos previos. La pregunta se presenta, y el modelo debe responder sin recibir contexto adicional.
- Few-shot con Chain-of-Thought (CoT): Aquí se incluyen ejemplos previos que le muestran al modelo cómo razonar paso a paso antes de llegar a una respuesta, busca estimular el razonamiento del modelo.

En ambos casos, medimos el accuracy como métrica principal, es decir, la proporción de respuestas correctas sobre el total.

## Resultados

### Zero-shot

El modelo principal (Gemini) obtuvo un accuracy del 57%, lo que demuestra que tiene mucha capacidad para seleccionar la respuesta correcta sin tener contexto adicional. Por otro lado, el modelo baseline (aleatorio), obtuvo un accuracy del 12% en esta tarea.

### Few-shot + Chain-of-Thought

El modelo principal mejora (marginalmente) su performance con respecto a la metodología de zero-shot con un accuracy del 58%. Esto demuestra que incorporar ejemplos y razonamiento mejora la performance, aunque esa mejora es marginal. En este caso, se obtuvo una accuracy de 12.40% para el modelo baseline, consistente con el azar y con el resultado previo.

## Conclusiones

Los resultados que obtuvimos con nuesto análisis indican que Gemini tiene una gran capacidad para resolver tareas de razonamiento, superando ampliamente al modelo aleatorio. Aunque la mejora entre el enfoque zero-shot y el few-shot con chain-of-thought sea marginal (1 punto porcentual), se encuentra en línea con los resultados de [Wang et al](https://arxiv.org/abs/2406.01574), donde obtienen mejores resultados al agregar chain-of-thought.