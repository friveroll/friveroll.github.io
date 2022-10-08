---
title: "Western Blotting Parte II: Blotting"
date: "2016-06-06"
draft: false
tags: [proteínas, electroforesis, western blot]
categories: [bioquímica, laboratorio]
math: true
---

La transferencia de proteínas o ácidos nucléicos a una membrana inmovilizante, es referida como "*blotting*", la transferencia puede ser realizada por difusión molecular, por un flujo de buffer inducido por la succión (microfiltración) o acción capilar, el método utilizado de manera más frecuente es la electrotransferencia.

La _difusión_ es un método simple pero muy ineficiente para aplicar a la mayoría de las transferencias, exepto para las transferencias de geles de agarosa. Generalmente las proteínas separadas en geles de poliacrlilamida se procesan por electroblotting, que utiliza la furza motriz de un campo eléctrico para que las proteínas eluyan desde el gel para inmovilizarlos en una matriz. Este método es rápido, eficiente y mantiene una alta resolución del patrón de bandas de las proteínas separadas por electroforesis.

Existen dos principales configuraciones para este método:

- [Transferencia húmeda](http://www.benchfly.com/video/34/western-blot-transfer-wet-apparatus/): En la transferencia húmeda, el gel y la membrana se ponen en forma de sandwich entre la esponja y el papel (esponja/papel filtro/membrana/gel/papel filtro/esponja) y todo está sujeto firmemente, después de asegurarse que no existen burbujas de aire entre el gel y la membrana. El sandwich se sumerge en el buffer de transferencia al cuál se le aplica un campo eléctrico. Las proteínas cargadas negativamente viajan a través del electrodo cargado positivamente, pero la membrana las detiene, al unirlas previniendo que sigan avanzando.

![Transferencia húmeda](/img/transferencia_humeda.png#center)

- [Transferencia semi-seca](http://www.benchfly.com/video/95/how-to-use-a-semi-dry-transfer-apparatus/): Para este tipo de transferencia, el gel y la membrana se colocan en forma de sandwich horizontalmente entre dos papeles filtro gruesos mojados con buffer (placa del ánodo/papel filtro/membrana/gel/papel filtro/placa del cátodo), los cuales están en contacto directo con dos placas sólidas de electrodos que están muy cerca una de la otra (la distancia entre ellas se limita solamente por el grosor del sandwich), también debe cuidarse en este método que no se generen burbujas entre el gel y la membrana, esto se hace después de colocar el segundo papel filtro haciendo rodar un pequeño tubo de ensaye por la superficie del filtro. El término semi-seca se refiere a la limitada cantidad de buffer.

![Transferencia semi-seca](/img/transferencia_semi-seca.png#center)

En la siguiente tabla podemos ver una comparación del buffer de transferencia para ambos métodos:

|           | Húmeda | Semi-Seca    |
|----------:|:------:|:------------:|
| Tris base | 25 mM  | 50 mM        |
| Glicina   | 192 mM | 40 mM        |
| pH        | 8.2    | N/D          |
| SDS       | N/D    | 0.04 % (p/v) |
| Metanol   | 20 %   | 20 %         |

Como puede notarse la transferencia Semiseca utiliza menos glicina y permite una transferencia más rápida pero no es muy efectiva para proteínas mayores a 120 KDa.

En la siguiente tabla podemos ver una comparación entre ambos sistemas



|                             | *Transferencia Húmeda*                                      | *Transferencia Semi-seca*                            |
|:----------------------------|:----------------------------------------------------------|:---------------------------------------------------|
| Flexibilidad                | Condiciones flexibles de voltaje, tiempo de blotting y requerimentos de enfriamiento del buffer; posiciones flexibles para los electrodos     | Dedicada a la transferencia rápida con un volúmen mínimo de buffer, sin necesidad de enfriamiento.  |
| Resultados cuantitativos Vs. cualitativos | Transferencia cuantitativa de proteínas de bajo peso molecular bajo condiciones que permitan la unión eficiente a la membrana. | Algunas de las moléculas de bajo peso molecular pueden ser transferidas a través de la membrana sin unirse cuantitativamente. |
| Rango de peso molecular | Amplio rango de peso molecular | Eficiencia variable de transferencia para proteínas mayores a 120 KDa (puede ser mejorado con un sistema de buffer discontinuo); las proteínas de bajo peso molecular pueden ser transferidas a través de la membrana. |
| Tiempo de transferencia  | Tiempo extendido (hasta 24 hr) posible sin la depleción del buffer; pueden obenerse transferencias rápidas (15 a 60 min) bajo condiciones de alta intensidad. | Transferencias rápidas; no puede haber transferencias extendidas debido a la depleción del buffer |  
| Control de temperatura | Regulación específica de la temperatura con un refrigerante y agua refrigerada a través de un recirculador; que permite transferencias a bajas temperaturas ( $4$ a $10^o C$ ). | No es posible la regulación de la temperatura por enfriamiento externo. |
| Capacidad amortiguadora | Hasta 10 a 12 L o tan bajo como 450 mL; la duración del tiempo para la transferencia no está restringida por una capacidad de buffer limitada. | Mínimo 250 ml por experimento; se reduce el costo de los reactivos y el tiempo del experimento |


Para la transferencia _semi-seca_, una concentración elevada de SDS, puede bloquear la unión de las proteínas a las membranas, pero el 0.01% facilita la transferencia, especialmente de proteínas grandes. La adición del 20% de metanol contraresta el hinchamiento del gel e incrementa la unión de proteínas obteniendo una buena resolución de bandas, además el metanol disminuye la posibilidad de que las proteínas se salgan del gel e incrementa su exposición a la membrana al incrementar la capacidad para que se peguen quitando el SDS de la superficie (esto beneficia a las proteínas más pequeñas) de la membrana, sin embargo, también impide la transferencia y deben ser balanceadas las concentraciones para esto. También deben ser equilibradas la corriente y el tiempo de transferencia dado que geles gruesos de poliacrilamida y moléculas hidrofóbicas y grandes (como los receptores) toman más tiempo de elución, mientras que, contrariamente las proteínas pequeñas transferidas a membranas para blotting con un tamaño grande del poro pueden pasar a través de la membrana si existe una saturación local para la unión de proteínas, causando su pérdida. 

Además de lo mencionado anteriormente los siguientes factores determinan si una proteína se unirá durante la transferencia se enumeran a continuación.

- Velocidad de elución desde el gel
  - Masa
  - Valores de corriente/voltaje
  - Propiedades fisicoquímicas de la proteína
      - hidrofobicidad
      - punto isoeléctrico
  - Modificaciones post-traduccionales, como por ejemplo el grado de glicosilación
  - Cantidad de SDS asociado a las proteínas: el SDS cubre a la proteína y le da una densidad de carga negativa
    - Gracias a esto la proteína se une rápidamente por la membrana
    - Limita la oportunidad para interactuar entre la membrana y las proteínas para su unión.
    - Permite que se solubilizen las proteínas y esto ayuda a las proteínas grandes como las proteínas de membrana.

Se utilizan dos tipos de membranas, las de nitrocelulosa, con tamaño de poro de 0.45 $\mu  m$ y floruro de polivinilidieno ó PVDF (polyvinylidene difluoride), poseen un tamaño de poro de 0.45 y 0.22 $\mu  m$  las cuales presentan solo pequeñas diferencias respecto a su capacidad de unión y la compatibilidad para el teñido. Su capacidad de unión a proteínas está al rededor de 100-200 $\frac{\mu g}{cm^{-2}}$.

Una vez que las proteínas se han transferido a la membarana, son más accesibles para varios ligandos que cuando estaban en el gel. En el blot ( la matriz inmovilizante que contiene las proteínas transferidas) se hace reaccionar con distintas sondas, tales como anticuerpos para la identificación del antígeno correspondiente. El blot generalmente requiere una cantidad pequeña de reactivos, las proteínas transferidas en las membranas pueden almacenarse por varias semanas después de su uso y también pueden ser utilizadas para análisis sucesivos.


{{< bibliography "bibliography/western_1-bib.json" >}}