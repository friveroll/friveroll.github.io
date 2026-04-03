---
title: "Western Blotting Parte III: Inmunodetección"
date: "2026-03-03"
draft: false
tags: [proteínas, inmunodetección, western blot, anticuerpos]
categories: [bioquímica, laboratorio]
bibFile: "bibliography/western_2-bib.json"
---

Tras separar las proteínas por SDS-PAGE y transferirlas a una membrana de nitrocelulosa o PVDF, el siguiente paso del western blot es la inmunodetección. En esta etapa, anticuerpos específicos reconocen la proteína inmovilizada, formando complejos antígeno–anticuerpo que posteriormente se hacen visibles mediante un sistema de detección quimioluminiscente, fluorescente o colorimétrico {{< cite "kurien2015western" >}}.

El objetivo es maximizar la unión específica del anticuerpo al epítopo de interés y minimizar la unión inespecífica a otras proteínas o a la propia membrana {{< cite "tie2020brief" >}}. Para lograr una buena relación señal/fondo son críticos tres elementos: un bloqueo adecuado de la membrana, anticuerpos bien validados y condiciones de incubación y lavado correctamente optimizadas {{< cite "mendez2023quantitative" "abcam2024blocking" >}}.

## Bloqueo de la membrana

### ¿Por qué bloqueamos?

Las membranas de nitrocelulosa y PVDF tienen alta capacidad de unión a proteínas, lo que incluye también a los anticuerpos si quedan sitios libres después de la transferencia {{< cite "kurien2015western" >}}. El bloqueo consiste en incubar la membrana con una solución rica en proteínas o polímeros inertes que saturan esos sitios no ocupados, reduciendo así la unión inespecífica de anticuerpos y el ruido de fondo {{< cite "tie2020brief;abcam2024blocking" >}}.

En la práctica, un buen bloqueo:

- Disminuye fondo difuso y bandas inespecíficas.
- Mejora la relación señal/fondo.
- Aumenta la reproducibilidad entre experimentos {{< cite "kurien2015western" >}}.

### Tipos de bloqueadores

Los bloqueadores más usados en western blot son:

- **Leche descremada en polvo** (3–5% en TBS o TBST): muy económica, fácil de preparar, útil para la mayoría de proteínas totales.
    - Contiene caseína y otras proteínas de la leche.
    - No recomendable para fosfoproteínas, porque la leche contiene fosfoproteínas que pueden incrementar el fondo con anticuerpos fosfo-específicos {{< cite "johnson1984improved;abcam2024blocking" >}}.
- **Albúmina sérica bovina (BSA)** (1–5% en TBS o TBST): proteína purificada, composición más constante, sin fosfatasas ni fosfoproteínas.
    - Preferida para fosfoproteínas o cuando la leche interfiere con el sistema de detección (por ejemplo, estreptavidina–biotina).
- Otros bloqueadores (gelatina de pescado, caseína purificada, bloqueadores comerciales): útiles en casos especializados, especialmente en western blots fluorescentes donde la autofluorescencia del bloqueador puede ser un problema {{< cite "cui2020optimization" >}}.

En el caso de detección fluorescente en el infrarrojo cercano, se ha visto que la elección del bloqueador influye tanto en el fondo como en la intensidad de señal, por lo que suele ser necesario optimizar específicamente para cada sistema {{< cite "cui2020optimization;licor2023optimization" >}}.

### Condiciones de bloqueo recomendadas

Protocolos estándar sugieren bloquear la membrana durante **1 hora a temperatura ambiente** con 3–5% de leche descremada o BSA en TBS/TBST, agitando suavemente {{< cite "kurien2015western;abcam2024blocking" >}}. En algunos casos se prolonga el bloqueo (por ejemplo, 2 horas o toda la noche a 4 °C) para anticuerpos problemáticos o sistemas muy sensibles {{< cite "cui2020optimization" >}}.

Un esquema práctico:

- Para proteínas totales no fosforiladas:
    - 5% leche descremada en TBST, 1 h a temperatura ambiente.
- Para fosfoproteínas o cuando se usan anticuerpos fosfo-específicos:
    - 3–5% BSA en TBST, 1 h a temperatura ambiente {{< cite "cui2020optimization;abcam2024blocking" >}}.

## Selección e incubación del anticuerpo primario

### Mono vs policlonales

El anticuerpo primario reconoce directamente el epítopo de la proteína de interés y puede ser:

- **Monoclonal**: reconoce un solo epítopo; ofrece mayor reproducibilidad y menor variación lote a lote.
- **Policlonal**: mezcla de inmunoglobulinas que reconocen varios epítopos; proporciona señal más intensa, pero con mayor riesgo de reactividad cruzada y bandas inespecíficas {{< cite "tie2020brief" >}}.

Las guías modernas de western blot cuantitativo recomiendan usar anticuerpos bien validados, con datos que demuestren especificidad (por ejemplo, ausencia de señal en líneas knockout), linealidad de la señal con la cantidad de proteína y buena caracterización del epítopo {{< cite "mendez2023quantitative;ghosh2014validating" >}}.

### Cómo leer la ficha técnica y validar un anticuerpo

Antes de incorporar un anticuerpo a tu rutina conviene revisar cuidadosamente la ficha del proveedor:

- Especie de origen (ratón, conejo, cabra, etc.) e isotipo.
- Aplicaciones validadas (WB, IHC, IF…).
- Dilución recomendada para western blot.
- Tipo de bloqueador sugerido (leche vs BSA).
- Tipo de muestra con la que se probó (lisado total, fracción nuclear, etc.).
- Evidencia de validación (knockout, controles de absorción, datos publicados).

Las buenas prácticas actuales sugieren **validar internamente** el anticuerpo, al menos con:

- Control positivo: lisado de una célula o tejido que exprese claramente la proteína (o proteína recombinante).
- Control negativo: línea o tejido knockout/knockdown o uno conocido por no expresar el antígeno.
- Curva de dosis: variar cantidad de proteína y/o dilución del anticuerpo para comprobar que la señal crece de forma aproximadamente lineal en el rango usado.

### Condiciones de incubación del primario

Protocolos clásicos recomiendan incubar la membrana con el anticuerpo primario:

- **1 hora a temperatura ambiente**, o
- **toda la noche a 4 °C**, para mejorar especificidad {{< cite "kurien2015western;jackson2024western" >}}.

La concentración óptima debe determinarse por **titulación** alrededor de la recomendación del fabricante (por ejemplo, 1:500, 1:1000, 1:2000, etc.). Usar concentraciones demasiado altas suele producir fondo elevado y bandas inespecíficas, mientras que concentraciones muy bajas resultan en bandas débiles o ausentes {{< cite "nordic2023tips" >}}.

El primario se diluye habitualmente en TBS o PBS con 0.05–0.1% de Tween 20 y 1–5% de bloqueador (leche o BSA). En algunos protocolos se recomienda incubar el primario en BSA incluso cuando el bloqueo se hizo con leche, lo que permite recuperar la solución de primario para reutilizarla {{< cite "kurien2015western" >}}.

## Selección e incubación del anticuerpo secundario

### Rol del secundario

El anticuerpo secundario reconoce al anticuerpo primario y suele estar conjugado con:

- Una enzima (HRP, fosfatasa alcalina) para detección quimioluminiscente o colorimétrica.
- Un fluoróforo (por ejemplo, IRDye) para detección fluorescente en infrarrojo cercano.

Esto genera una amplificación de señal, ya que varios secundarios pueden unirse a un solo primario {{< cite "abcam2024quantification;azure2021handbook" >}}. Es fundamental que el secundario sea específico para la **especie e isotipo** del primario (por ejemplo, anti-IgG de conejo si el primario es de conejo) y compatible con el sistema de detección.

### Condiciones de incubación

El secundario se incuba típicamente:

- **30–60 minutos a temperatura ambiente**, con agitación suave,
- En buffer con bloqueador (por ejemplo, 1–5% leche o BSA en TBST).

Las diluciones comunes son de **1:5000 a 1:20 000**, dependiendo de la afinidad y del sistema de detección. Usar demasiado secundario provoca fondo alto y bandas inespecíficas; si el secundario es fluorescente, la membrana debe protegerse de la luz para evitar fotoblanqueo antes de la adquisición de la imagen {{< cite "cytiva2025protocols;fredhutch2024odyssey" >}}.

## Esquema práctico de inmunodetección

Un flujo de trabajo típico para la etapa de inmunodetección es:

1. **Bloqueo**
    - 3–5% leche descremada o BSA en TBS/TBST.
    - 1 hora a temperatura ambiente con agitación.
2. **Incubación con anticuerpo primario**
    - Primario diluido en buffer con bloqueador.
    - 1 hora a temperatura ambiente o toda la noche a 4 °C.
3. **Lavados tras primario**
    - 3 × 5–10 min en TBS/TBST, con agitación.
4. **Incubación con anticuerpo secundario**
    - Secundario conjugado, 1:5000–1:20 000.
    - 30–60 min a temperatura ambiente con agitación.
5. **Lavados tras secundario**
    - 3–4 × 5–10 min en TBS/TBST.
6. **Preparación para detección**
    - Cambio a buffer adecuado y aplicación de sustrato quimioluminiscente o preparación para imagen fluorescente {{< cite "abcam2024protocol;merck2024western" >}}.

## Controles en la etapa de inmunodetección

### Control sin anticuerpo primario

Un control básico consiste en omitir el anticuerpo primario y mantener todo lo demás igual (bloqueo, secundario, lavados, detección). Si en este control aparecen bandas o fondo significativo, la señal se debe a la unión inespecífica del secundario o al propio sistema de detección, no al reconocimiento del antígeno {{< cite "abcam2024protocol" >}}.

Este control ayuda a decidir si es necesario cambiar de secundario, ajustar su dilución, modificar el bloqueador o mejorar los lavados.

### Controles de isotipo

Los controles de isotipo utilizan un anticuerpo del mismo isotipo, especie y conjugado que el primario, pero dirigido contra un antígeno irrelevante. En condiciones ideales, este anticuerpo debe producir solo fondo bajo y uniforme; cualquier banda específica sugiere reactividad cruzada del primario o problemas de unión inespecífica relacionados con el isotipo {{< cite "abcam2024protocol" >}}.

Aunque se emplean más en citometría e IHC, pueden ser útiles en western blot cuando se trabaja con anticuerpos nuevos o poco caracterizados {{< cite "tie2020brief" >}}.

### Controles positivos y negativos de expresión

Además, es importante incluir controles de expresión en los carriles:

- **Control positivo**: lisado de células o tejido que expresen claramente la proteína, o proteína recombinante purificada.
- **Control negativo**: lisado de línea/animal knockout o tejido sin expresión del antígeno.

Si la muestra experimental no muestra banda, pero el control positivo sí, el resultado negativo puede ser real. Si tampoco se observa banda en el control positivo, el problema probablemente está en el anticuerpo o en las condiciones del blot {{< cite "ghosh2014validating" >}}.

## Problemas frecuentes y cómo abordarlos

A continuación se resumen problemas típicos atribuibles principalmente a la etapa de inmunodetección, con causas probables y posibles soluciones {{< cite "azure2021handbook;nordic2023tips" >}}.

### Fondo muy alto en toda la membrana

Posibles causas:

- Bloqueo insuficiente o bloqueador inadecuado.
- Concentración excesiva de primario y/o secundario.
- Lavados insuficientes.

Estrategias:

- Aumentar el tiempo o la concentración del bloqueador, o cambiar de leche a BSA (o viceversa).
- Reducir las concentraciones de primario y secundario mediante titulación.
- Incrementar número o duración de los lavados y preparar buffers frescos.

### Bandas inespecíficas adicionales

Posibles causas:

- Anticuerpo primario poco específico o muy concentrado.
- Bloqueador que no cubre adecuadamente sitios de unión.
- Incubaciones demasiado largas o a alta temperatura.

Estrategias:

- Titrar el primario con varias diluciones alrededor de la recomendada.
- Probar un bloqueador diferente o añadir una pequeña cantidad de SDS (0.005–0.02%) al buffer de lavado con precaución {{< cite "cui2020optimization" >}}.
- Incubar el primario toda la noche a 4 °C en vez de 1 hora a temperatura ambiente.

### Señal muy débil o ausente

Posibles causas:

- Concentración de primario demasiado baja.
- Tiempo de incubación insuficiente.
- Epítopo parcialmente enmascarado por el bloqueador.
- Degradación del anticuerpo o uso de un secundario incorrecto.

Estrategias:

- Aumentar concentración o tiempo de incubación del primario.
- Reducir la concentración del bloqueador o cambiar de leche a BSA.
- Verificar que el secundario reconoce correctamente la especie del primario.
- Comprobar que los anticuerpos se han almacenado en alícuotas y sin ciclos excesivos de congelación/descongelación.

### Resultados poco reproducibles entre réplicas

Posibles causas:

- Variaciones en bloqueo, diluciones de anticuerpos o tiempos de incubación.
- Cambios de lote de anticuerpo sin reoptimización.
- Diferencias en agitación o temperatura durante incubaciones.

Estrategias:

- Estandarizar recetas, tiempos y condiciones (llevar registro detallado de cada blot).
- Preparar soluciones madre de anticuerpo y mantener el mismo lote para una serie de experimentos.
- Usar controles internos (controles de carga, proteína total) para monitorizar variaciones entre membranas.

{{< bibliography "bibliography/western_2-bib.json" >}}
