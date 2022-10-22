---
title: "¿Qué es la Bioinformática?"
date: "2022-10-19"
draft: false
tags: [teoría, definición]
categories: [bioinformática]
---

El término bioinformática conceptualiza la biología en términos de moléculas (proteínas, ácidos nucléicos) y la aplicación de técnicas informáticas para entender y organizar este tipo de información biológica.

Así que su meta principal tiene que ver con poder encontrar información guardada dentro de los organizamos vivos y depende de conceptos y aplicaciones de la biología, las ciencias de la computación y el análisis de datos.

## La bioinformática es predictiva

Para poder encontrar el sentido de los datos biológicos debemos hacer predicciones acerca de lo que ya conocemos, es decir obtener conocimiento biológico por comparación con lo que se conoce.

De esta forma podriamos contestar las siguientes preguntas:

- ¿Cuál es la función de una proteína?
- ¿Cómo funciona una proteína?
- ¿Que factores necesita una proteína para funcionar?
- ¿Cómo pueden interactuar múltiples proteínas una con otra para afectar sus funciones?

Pero esto solo nos brinda una predicción, en ningún momento remplaza a la experimientación.

## Escalas de conocimiento 

En sus inicios la bioinformática tenía un enfoque de solo tratar con un gen o una proteína a la vez.

Hoy en día, la tecnología permite obtener grandes cantidades de datos, lo que requiere un cinetífico de datos para:

- Transformar los datos en un entendimiento predictivo.
- Poder utilizar datos aunque estén incompletos.
- Explorar los datos para generar hipótesis que puedan ser probadas experimentalmente.

## Escalabilidad

Cuando hablamos de grandes cantidades de datos nos referimos a genomas completos, obtenidos por nuevas tecnologías de secuenciación (NGS).

Por ejemplo, estos son algunos de los recursos computacionales necesarios para grandes cantidades de datos.

- Genoma Humano - 8 GB
- Árbol de la Vida - 1 TB
- Datos crudos de un experimento de NGS - 1 TB

De tal manera que también deben de ser escaladas las técnicas y herramientas empleadas.

## ¿Cómo ha cambiado la Bioinformática?

- En los primeros días –inicio 2000s- Era sinónimo de análisis de secuencias. 
  - Se obtenía solo algunas secuencias de ADN y se analizaban por varias propiedades
- En la mitad del 2000s, disponibilidad de NGS (métodos de nueva generación para la secuenciación) se hizo
posible medir el contexto genómico de una sola célula en una sola corrida eperimental.
  - Esta nueva tecnología ha transformado a la bioinformática en una ciencia de datos que se construye
a partir del proceso de la bioinformática clásica, para investigar y resumir conjuntos masivos de datos de una
complejidad extraordinaria

## ¿Qué subcampos existen?

Actualmente la bioinformática no solo es utilizada para revelar la información contenida en el ADN de una célula,
también tiene otras aplicaciones que pueden caer en 4 categorías

- Ensamblaje: Establecer la composición de nucleótidos de los genomas
- Resecuenciación: identificar mutaciones y variantes en los genomas
- Clasificación: determinar la composición de las especies en una población de organismos
- Cuantificación: utilizar la secuenciación de ADN, para medir las características funcionales de una célula.

## ¿Qué características tiene un proyecto bioinformático?

La mayoría de los proyectos pueden comenzar con un plan estándar, sin embargo el plán no está escrito sobre la roca, puede cambiar dependiendo de las funciones, características de las observaciones y resultados de análisis, que puden derivar en tareas adicionales del plan original para considerar las variaciones en los datos

## ¿Qué tipo de software bioinformático existe?

- Más básico: Uso de herramientas basadas en aplicaciones web [NCBI, blast]
  - Solo se necesitan conocimientos de biología
- Más profesional: Uso de herramientas independientes
  - Herramientas con GUI (Interfase gráfica de usuario)
  - Herramientas sin GUI (basadas en terminal)
    - Además de los conocimientos en biología se necesita la habilidad para utilizar Unix, y escribir scripts.
    - High-throughput data: Algunas veces se necesita el entendimiento del almacenamacenamiento de datos y la escalabilidad
- Más avanzado: Desarrollo de algoritmos
  - Enfocado en ciencias de la computación, usualmente asociado con biólogos
- Hacer públicos los datos/métodos
  - Creación de bases de datos/ páginas web

{{< bibliography "bibliography/que_es_bioinformatics-bib.json" >}}
