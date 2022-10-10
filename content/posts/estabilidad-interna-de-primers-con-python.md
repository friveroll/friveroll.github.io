---
title: "Estabilidad interna de primers con python"
date: 2022-10-09
draft: false
tags: [primers, cebadores, PCR]
categories: [bioinformática]
math: true
---

Para evitar los sitios de union falsos del primer a la secuencia blanco (_mispriming_) hay que evitar que el extremo 3' sea muy pegajoso (o estable), una forma de obtener un extremo pegajoso es si éste contiene un alto contenido de GC. El centro del primer y el extremo 5' deberán de ser más pegajosos, para una mejor estabilidad.

En otras palabras buscamos que el primer sea estable en el extremo 5' y un poco inestable para el extremo 3'.

La estabilidad podemos predecirla a partir de una gráfica del valor de $\Delta{G}$ de los pentámeros del primer.

Por ejemplo si tenemos esta secuencia para un primer ``ACTTGGGATTGGGCT`` podemos obtener 11 pentámeros.

1. ``ACTTG``
2. ``CTTGG``
3. ``TTGGG``
4. ``TGGGA``
5. ``GGGAT``
6. ``GGATT``
7. ``GATTG``
8. ``ATTGG``
9. ``TTGGG``
10. ``TGGGC``
11. ``GGGCT``

Si calculamos el $\Delta{G}$ para cada uno de ellos y lo graficamos podemos obtener la siguiente figura

![Gráfica estabilidad interna](/img/grafica-estabilidad-interna.png#center)

Como podemos ver en la gráfica este primer tiene una pobre estabilidad interna con un extremo 5' con baja estabilidad y el 3' con una alta estabilidad, caso contrario a lo que se busca, esto podría hacer que ste primer pueda dar varios productos de distinto tamaño debido al _mispriming_.

Uno de los programas que nos brinda la opción de conocer esta gráfica es Oligo 7, pero también podemos obtener esta gráfica con python utilzando el siguiente código.

```python
def calc_dG(seq):
    NearPairsPlus = ['AA','AC','AG','AT',
                     'CA','CC','CG','CT',
                     'GA','GC','GG','GT',
                     'TA','TC','TG','TT']
    DeltaG =        [-1.9, -1.3, -1.6, -1.5,
                     -1.9, -3.1, -3.6, -1.6,
                     -1.6, -3.1, -3.1, -1.3,
                     -1.0, -1.6, -1.9, -1.9 ]
    c = []
    seq = seq+ ' '
    a = 0
    while a < len(seq):
        b = 0
        nuc = []
        while b < len(NearPairsPlus):
            if seq[a-2:a] == NearPairsPlus[b]:
                c.append(DeltaG[b])
            b = b + 1
        a = a + 1
    return c

sequence = "ACTTGGGATTGGGCT"
dG_values = calc_dG(sequence)
y = dG_values
x = list(sequence)[:-1]

dG_pentamers = []
for i in range(0, len(y)):
    dG_pentamers.append(sum(y[i:i+4]))
    if i+4 == len(y):
        break

import matplotlib.pyplot as plt

y_labels = list(sequence[0:-5])
y_labels.append(sequence[len(sequence)-5:len(sequence)])

plt.plot(dG_pentamers, marker='o', linestyle=':', color='b')

plt.ylabel(r'$\sum \Delta$ G $\left(\frac{kcal}{mol}\right)$ pentamers')
plt.xticks(range(len(y_labels)), y_labels)
plt.gca().axes.set_ylim([-12,-5])
plt.gca().invert_yaxis()
plt.show()
```

{{< bibliography "bibliography/estabilidad_interna-bib.json" >}}