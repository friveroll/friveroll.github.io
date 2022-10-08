---
title: "Visualizando resultados para Blast en Mobyle@Pasteur"
date: "2011-05-30"
draft: false
tags: [proteínas, blast]
categories: [bioquímica, bioinformática]
---

**Nota:** Actualmente ya no existe el servicio de Mobyle@pasteur, se ha cambiado por un servidor de Galaxy, el siguiente artículo muestra el procedimiento para mandar los resultados de blast a mview un programa que puede dar formato como alineamiento listo para ser presentado en una publicación. (Actualizado el 8 de septiembre de 2022)

Brevemente, BLAST (**B**asic **L**ocal **A**lignment **S**earch **T**ool) es un método de búsqueda por homología, en el cuál se reta a la base de datos con una secuencia problema (query), para que encuentre secuencias relacionadas a nuestra secuencia problema.

Los resultados que BLAST arroja, son alineamientos pareados junto con los datos de la posición de la secuencia problema (*query*) con la secuencia que el programa encontró de la base de datos.

Uno de los problemas para hacer un alineamiento múltiple de las secuencias que encontramos en BLAST, es que los resultados solo arrojan links a las secuencias que nos interesan, de modo que hay que recuperar las secuencias una por una y además al hacer esto obtenemos la secuencia completa con residuos adicionales que no corresponden a la región que nos interesa (*match*) y por tanto obtenemos secuencias con una longitud distinta, lo cuál no es muy deseable para un alineamiento múltiple.

Así que si queremos visualizar los datos en forma de un alineamiento múltiple o generar archivos fasta con las secuencias alineadas, podemos utilizar MView el cual podemos descargar e instalar localmente en una maquina con linux, o también lo podemos utilizar en el portal [Mobyle@pasteur](http://mobyle.pasteur.fr).

Para hacer este análisis pueden seguirse los siguientes pasos:

- Ir a [Mobyle@pasteur](http://mobyle.pasteur.fr).
- En la pestaña program seleccionamos, database>homology>blast2
![Blast homología](/img/blast_1.png#center)
- Ahora en la interfase web de blast2, seleccionamos como en la imagen: 
![Blast selección](/img/blast_2.png#center)- Blast program: __blastp__
- Proteín db: __uniprot_sb__
- En la sección _query sequence/query_, para cargar la secuencia de ejemplo en la base de datos seleccionamos DB, después en la lista desplegable de abajo seleccionamos la base de datos uniprot y en el cuadro diálogo escribimos P25044 y damos click en _Select_.
![Blast query](/img/blast_3.png#center)
- Ahora el portal nos mostrará esta secuencia en el cuadro de diálogo más grande.
![Blast sequencia](/img/blast_4.png#center)
- Ahora, vamos hacia la parte de arriba de la ventana y damos click en _Run_.
![Blast Run](/img/blast_5.png#center)
- Y ahora el portal nos muestra los resultados.
![Blast resultados](/img/blast_6.png#center)
- Podemos ver que existen tres opciones para visualizar los resultados, como texto, html y de manera visual.
- Seguido de donde están los resultados en texto, hay una lista desplegable seguida del botón _further analysis_.
- Ahora seleccionamos de la lista mview(blast)>Further analysis.
![Blast mview](/img/blast_7.png#center)
- Enseguida nos aparecerá la interfase web para mview con los resultados de blast, obtenidos anteriormente
![Resultados mview](/img/blast_8.png#center)
- Hay dos opciones que me parecen útiles para dar formato a los resultados de blast, por defecto está seleccionado html, lo que nos mostrará un alineamiento coloreado de la secuencias alineadas en blast. Pero si deseamos seguir trabajando con la secuencia, tenemos que obtener un archivo fasta, con todas las secuencias alineadas. Claro que también como podrán darse cuenta, hay muchas opciones, para poder filtrar las secuancias que queremos obtener, de momento solo vamos a usar otro programa para ver los resultados.
- Seleccionamos _Pearson/FASTA_ en el menú desplegable de la sección _Output format (-out)_ 
![fasta](/img/blast_9.png#center)
- Y damos click en _Run_.
- Ahora podemos ver como resultado un archivo con formato fasta de las secuencias.
![2 secuencias](/img/blast_10.png#center)
- De aqui, podemos seguir haciendo análisis filogenéticos u obtener estadísticas sobre el alineamiento, pero en este ejemplo, vamos a reformatear estos resultados con un programa de la suite __EMBOSS__, llamado __showalign__.
- Similar a lo que hicimos para mandar los resultados de blast a mview, ahora vamos a seleccionar showalign(sequence).
![Mandar a showalign](/img/blast_11.png#center)
![Showalign](/img/blast_12.png#center)
- Utilizando __advanced options__ podemos seleccionar varias opciones para como queremos visualizar el alineamiento, como por ejemplo la scoring matrix y como queremos ver los residuos que no son idénticos
![Showalign advanced](/img/blast_13.png#center)
![Showalign advanced 2](/img/blast_14.png#center)
- Por ejemplo podemos mostrar en los resultados los residuos similares y también ordenarlos de acuerdo a su similaridad con estas opciones
![Mostrar secuencias similares](/img/blast_15.png#center)
- Para obtener el siguiente resultado:
![Resultado alineamiento](/img/blast_16.png#center)

{{< bibliography "bibliography\Blast_mobyle-bib.json" >}}
