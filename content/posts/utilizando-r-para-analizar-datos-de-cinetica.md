---
title: "Utilizando R para analizar datos de cinética"
date: "2012-05-23"
draft: false
tags: [cinética enzimática, R]
categories: [bioquímica]
math: true
---

R es un lenguaje de programación estadístico y gráfico, que através del proyecto bioconductor puede ser usado para analisis bioinformáticos, en esta ocasión lo utilizaremos para graficar y analizar datos cinéticos.

El siguiente tutorial, está basado en el problema 1 del capítulo 4 del libro _Biochemical Calculations_ de _Irwin H Segel_,  y se mostrará como obtener $V_{max}$ y $Km$ a partir del análisis de los datos cinéticos por la gráfica de Lineweaver-Burk.

Primero generamos los vectores $S$ y $v$ para los datos del problema

```r
S <- c(2.50e-06, 3.33e-06, 4.00e-06, 5.00e-06,
       1.00e-05, 2.00e-05, 4.00e-05, 1.00e-04,
       2.00e-03, 1.00e-02)
v <- c(24, 30, 34, 40, 60, 80, 96, 109, 119, 120)
```

Podemos visualizarlos en una tabla de la siguiente manera:

```r
data.frame(S, v) -> datos.cinetica
datos.cinetica

         S  v
1  2.50e-06  24
2  3.33e-06  30
3  4.00e-06  34
4  5.00e-06  40
5  1.00e-05  60
6  2.00e-05  80
7  4.00e-05  96
8  1.00e-04 109
9  2.00e-03 119
10 1.00e-02 120
```

Para poder obtener la grafica a partir de los datos anteriores

```r
plot(datos.cinetica, main="V Vs. S", xmain="S", ymain="v")
```
Ahora necesitamos calcular los recíprocos $(r)$ para $S$ y $v$.

```r
r.S <- 1/S
r.v <- 1/v
```

Graficando los recíprocos

```r
plot(r.S, r.v, main="Lineweaver-Burk", xlab="1/[S]",  ylab="1/v", pch=20, col="blue")
```

Ahora necesitamos hacer la regresion lineal de estos datos y añadir la linea correspondiente a la regresión a nuestra gráfica

```r
lm(r.v ~ r.S) -> lineweaver.reg
abline(lineweaver.reg, col="red")
```

Y la grafica final queda de la siguiente forma:

![Modelo Lineweaver](/img/lineweaver.png#center)

Cuando calculamos los datos para la regresión lineal, guardamos los resultados en la siguiente variable:

```r
lineweaver.reg

Call:
lm(formula = r.v ~ r.S)

Coefficients:
(Intercept)          r.S
  8.343e-03    8.344e-08
```

Intercept, corresponde a la intersección en $Y$, que para este caso es $\frac{1}{V_{max}}$.

```r
interseccion.y <- coef(lineweaver.reg)[1]
r.Vmax <- interseccion.y
```

Así que para calcular $V_{max}$

```r
Vmax <- 1/r.Vmax
```
La pendiente está en _r.S_ por lo tanto.

```r
pendiente <- coef(lineweaver.reg)[2]
```

Para calcular la interseccion en $x$, necesitamos encontrar el valor de $x$ cuando $y = 0$, para la ecuación de la linea recta

$$y = (pendiente)x + interseccion.y$$

$$0 = (pendiente)x + interseccion.y$$

$$x=-\frac{interseccion.y}{pendiente}$$

```r
interseccion.x <- -(interseccion.y/pendiente)
```

Y como la intersección en $x$ es $\frac{1}{km}$ en este modelo

```r
r.km <- interseccion.x
```

Ahora podemos calcular $km$ de la siguiente forma

```r
km <- -1/r.km
```

Ahora guardamos los datos de $Km$ y $V_{max}$ calculados previamente en un vector

```r
Lineweaver.Burk <- c(Vmax=as.numeric(Vmax), Km=as.numeric(km))
```

Podemos comparar nuestros resultados, con los que se obtienen de la ecuación de Michaelis Menten, por medio de un ajuste no lineal

```r
Mfit <- nls(v~(Vmax*S)/(Km+S), datos.cinetica, start=list(Vmax=1, Km=0)) 
```

Para poder comparar estos resultados los guardamos en un vector

```r
Michaelis.Menten <- c(Vmax=as.numeric(coef(Mfit)[1]), Km=as.numeric(coef(Mfit)[2]))
```

Y utilizamos el comando rbind, para crear un data.frame con los resultados para los dos modelos

```r
rbind(Lineweaver.Burk, Michaelis.Menten) 
```

Ahora podemos comparar los resultados por ambos modelos

```
                     Vmax           Km
Lineweaver.Burk  119.8564 1.000130e-05
Michaelis.Menten 119.8967 9.997717e-06
```

Y finalmente si queremos graficar los datos obtenidos del modelo de Michaelis Menten podemos utilizar

```r
plot(S, predict(Mfit), type="l", main="V Vs. S", xlab="S", ylab="v")
```
![Modelo Michaelis Menten](/img/michaelis.png#center)

Código completo

{{< gist friveroll 2779025 >}}

{{< bibliography "bibliography\enzimas-bib.json" >}}