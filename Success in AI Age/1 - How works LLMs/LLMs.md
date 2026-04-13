
2026-02-17 22:30

status: #baby 

tags: [[English]] [[Machine Learning]]


# LLMs

Los _Large Language Models_ (LLMs) surgen de la necesidad de interactuar con las computadoras mediante lenguaje natural, permitiendo comunicarnos con la máquina de forma directa, similar a una conversación humana. En lugar de utilizar exclusivamente lenguajes de programación, estos modelos permiten solicitar tareas mediante texto común, reduciendo la barrera técnica entre el usuario y el sistema computacional.

Inicialmente, esta tecnología comenzó como sistemas avanzados de autocompletado de texto. Los primeros modelos eran relativamente pequeños y se basaban en predecir la siguiente palabra a partir del contexto disponible. Con el tiempo, estos sistemas evolucionaron gracias al uso de redes neuronales más profundas, el acceso a grandes volúmenes de datos, la integración de herramientas externas, mecanismos de recuperación de información en tiempo real y arquitecturas que ampliaron significativamente su capacidad de razonamiento y uso práctico.

El avance clave llegó con los GPT (_Generative Pre-Trained Transformers_). Estos modelos generan texto produciendo un token —una unidad mínima de lenguaje que puede ser una palabra o fragmento de ella— en cada paso de inferencia. El proceso de _pre-training_ consiste en entrenar la red neuronal con grandes cantidades de texto para que aprenda patrones estadísticos del lenguaje. Posteriormente, mediante la arquitectura _Transformer_, el modelo puede procesar secuencias completas de texto simultáneamente, en lugar de analizar palabra por palabra de forma estrictamente secuencial.

La innovación principal de los transformers es el mecanismo de _attention_ (atención), que permite al modelo asignar diferentes niveles de importancia a las palabras dentro de una oración o un contexto más amplio. Gracias a este mecanismo, el modelo puede relacionar conceptos distantes dentro del texto y construir respuestas coherentes basadas en múltiples partes del contexto al mismo tiempo.

Este proceso ocurre dentro de una _context window_ o ventana de contexto, que representa la cantidad máxima de tokens que el modelo puede considerar simultáneamente. Cada modelo posee un límite específico de tokens; por ello, tanto el prompt como la conversación deben mantenerse dentro de ese rango. Cuando la información excede esta ventana, partes del contexto dejan de ser consideradas por el modelo, lo que puede provocar respuestas menos coherentes o desconectadas de la conversación previa.

### Lecturas
https://aws.amazon.com/es/what-is/large-language-model/


