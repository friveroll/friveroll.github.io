---
title: "Introducción al análisis de microarreglos"
date: "2012-05-20"
draft: false
tags: [R, microarreglos, bioconductor]
categories: [bioinformática]
---

Los datos que utilizaremos vienen de un estudio de Chiaretti et al.  sobre la leucemia linfoblastica aguda (ALL), la cual fue conducida con chips de microarreglos HG-U95Av2 de Aymetrix. El paquete de datos ALL contiene los datos de expresion del experimento, normalizados con rma (las intensidades estan en escala log2), junto con las anotaciones de las muestras. 

Para ello utilizaremos R /Bioconductor Si no has instalado Bioconductor previemente necesitas correr las siguientes líneas

```r
source("http://bioconductor.org/biocLite.R")
biocLite()
```
Necesitaremos instalar algunas librerías de Bioconductor

```r
biocLite(c("affy","ALL","annotate","hgu95av2.db","multtest","topGO","genefilter"))
```
Ahora, cargamos los datos de microarreglos y los asociamos a un set de expresión con

```r
library(ALL)
data(ALL)
```
Y para conocer la dimensión de la matriz de expresión

```r
dim(exprs(ALL))
```

`[1] 12625 128`

Por tanto, se trata de 128 columnas (muestras) con 12,625 filas (sondas) Nos interesa seleccionar todas las sondas de las células B (que pueden ser identicadas por la columna BT del ExpressionSet ALL).

```r
table(ALL$BT)
```

```
B B1 B2 B3 B4  T T1 T2 T3 T4 
5 19 36 23 12  5  1 15 10  2
```

Ademas nos interesa la comparacion de las sondas con la fusión del gen BCR/ABL resultante de la traslocacion de los cromosomas 9 y 22

```r
table(ALL$mol.biol)
```
```
ALL1/AF4  BCR/ABL E2A/PBX1      NEG   NUP-98  p15/p16 
      10       37        5       74        1        1 
```

Para seleccionar todos los genes de las células B con la fusión BCR/ABL y del grupo control

```r
subset <- intersect(grep("^B", as.character(ALL$BT)),
                    which(as.character(ALL$mol.biol)
                          %in% c("BCR/ABL", "NEG")))
```

Y creamos un nuevo set de expresión con los genes seleccionados previamente

```r
eset <- ALL[, subset]
```

Para poder comparar el grupo de los pacientes control con los que presentan la fusión BCR/ABL, necesitamos convertir a factores la columna `eset$mol.biol`

```r
eset$mol.biol <- factor(eset$mol.biol)
table(eset$mol.biol)
```

```
BCR/ABL     NEG 
     37      42
```

Muchos de los genes en el chip no se expresan en las celulas B estudiados aquí, o podrían solo tener una pequeña variabilidad a traves de las sondas. Trataremos de remover esos genes con un filtro de intensidad (la intensidad de un gen debe de estar arriba de 100 y en al menos el 25 porciento de las sondas);  y un filtro de varianza (el rango intercuartil de las intensidades log2 debera ser al menos de 0.5)

```r
library(genefilter)
f1 <- pOverA(0.25, log2(100))
f2 <- function(x) (IQR(x) > 0.5)
ff <- filterfun(f1, f2)
```
Cuantos genes podemos obtener despues del filtrado?

```r
selected <- genefilter(eset, ff)
sum(selected)
```

`[1] 2391`

Crearemos un nuevo ExpressionSet que contenga solo los genes que pasaron nuestro filtro.


```r
esetSub <- eset[selected, ]
```

Ahora estamos listos para analizar la expresion diferencial en los genes seleccionados entre las muetraas BRC/ABL y la normales citogenéticamente. Utilizaremos una prueba t pareada, para identicar los genes expresados de manera diferencial entre los dos grupos.


```r
library(multtest)
```

La funcion `mt.teststat` del paquete multtest nos permite calcular varios estadsticos de prueba comunmente usados para todas las las de una matriz de datos (chequen su pagina de ayuda). Primero, calcularemos los p-values nominales


```r
library(multtest)
cl <- as.numeric(esetSub$mol.biol == "BCR/ABL")
t <- mt.teststat(
exprs(esetSub),
classlabel = cl,
test = "t.equalvar")
```
La funcion `pt` nos da la distribucion t.

```r
pt <- 2 * pt(-abs(t), df = ncol(exprs(esetSub)))
```

Podemos obtener una impresion de la cantidad de genes expresados de manera diferencial, observando en un histograma de la distribucion del valor de p.


```r
hist(pt, 50)
```

<img src="{{ root_url }}/images/histogram-pt.png" />

La función `mt.rawp2adjp` de el paquete multtest contiene distintos procedimeintos de prueba (Checa la página de ayuda para esta función). Para obtener el ajuste de p-value en términos de FDR (False Discovery Rate) utilizaremos el método de  Benjamini y Hochberg. ¿Cuántos genes obtendremos si imponemos un FDR de 0.1?

```r
pAdjusted <- mt.rawp2adjp(pt, proc = c("BH"))
sum(pAdjusted$adjp[, "BH"] < 0.1)
```

`[1] 171`

También esta función retorna los p-values ordenados de menor a mayor. Para obtenerlos ordenados utilizaremos:


```r
pBH <- pAdjusted$adjp[order(pAdjusted$index), "BH"]
names(pBH)<-featureNames(esetSub)
```
Ahora, queremos conocer cuáles genes son los más significativos, por medio de el análisis de sus valores crudos (raw) y sus p-values ajustados por  distintos métodos. Los símbolos de los genes son obtenidos por medio del paquete de anotación `hgu95av2`.

```r
library(annotate)
library(hgu95av2.db)
diff <- pAdjusted$index[1:10]
genesymbolsDiff <- unlist(mget(featureNames(esetSub)[diff], hgu95av2SYMBOL))
genesymbolsDiff
```

```
 1636_g_at   39730_at    1635_at   40202_at   37027_at 39837_s_at 
    "ABL1"     "ABL1"     "ABL1"     "KLF9"    "AHNAK"   "ZNF467" 
40480_s_at   33774_at   36591_at   37014_at 
     "FYN"    "CASP8"   "TUBA4A"      "MX1" 
```

El top 3 de las sondas representan al gen ABL1, el cual es afectado, por la traslocación caracterizada por mas sondas  BCR/ABL. Ahora queremos ver si existen otro conjunto de sondas que representen a este gen, y ver éste ha sido seleccionado por nuestro filtro no específico.


```r
geneSymbols = unlist(mget(featureNames(ALL), hgu95av2SYMBOL))
ABL1probes <- which(geneSymbols == "ABL1")
selected[ABL1probes]
```

```
  1635_at 1636_g_at 1656_s_at 2040_s_at 2041_i_at  39730_at 
     TRUE      TRUE     FALSE     FALSE     FALSE      TRUE
```

Así que los otros conjuntos de sondas en el chip representando ABL1 han sido eliminados por el filtro, debido a baja intensidad o baja varianza. Ahora queremos saber si esto indica también expresión diferencial del gen _ALB1_.

```r
tABL1 <- mt.teststat(exprs(eset)[ABL1probes, ], classlabel = cl,
test = "t.equalvar")
ptABL1 <- 2 * pt(-abs(tABL1), df = ncol(exprs(esetSub)) - 2)
sort(ptABL1)
```

```
[1] 3.762489e-14 4.791997e-13 2.445693e-10 5.486259e-02 5.842693e-01
[6] 7.570959e-01
```

Vemos ahora que solo tres de las seis sondas para _ABL1_ muestran evidencia (de hecho, evidencia muy fuerte) para expresión diferencial. Seria interesante seguir investigando si el conjunto de sondas, en lo que concierne pe. a su localización en la secuencia del transcripto _ABL1_ - en efecto la fusión de genes _BCR/ABL_ resulta de la traslocación del gen normal _ABL1_. Utilizando la ontología de genes Muchos de los efectos debidos a la traslocación _BCR/ABL_, son mediados por la actividad de tirosina cinasa. Veamos ahora los conjuntos de sondas en el chip que pertenecen a el término GO de actividad proteína tirosina cinasa, la cuál tiene el identificador _GO:0004713_.


```r
gN <- featureNames(esetSub)
tykin <- unique(unlist(lookUp("GO:0004713",
"hgu95av2", "GO2ALLPROBES")))
str(tykin)
sel <- (gN %in% tykin)
```

Ahora podermos checar si los genes entre las tirisina cinasas son expresados de manera diferencial, respecto a los otros genes. Utilizaremos una prueba exacta de Fisher, para tablas de contingencias, para revisar las proporciones de los genes expresados diferencialmente son significativas entre dos grupos de genes.

```r
tab <- table(pt < 0.05, sel, dnn = c("p < 0.05", "tykin"))
print(tab)
```

```
        tykin
p < 0.05 FALSE TRUE
   FALSE  1914   26
   TRUE    434   17
```

```r
fisher.test(tab)
```

```
	Fisher's Exact Test for Count Data

data:  tab
p-value = 0.001297
alternative hypothesis: true odds ratio is not equal to 1
95 percent confidence interval:
 1.453631 5.573555
sample estimates:
odds ratio 
  2.881814 
```

__Análisis para el enriquecimiento de conjunto de genes__. Ahora queremos buscar el enriquecimiento en la expresión diferencial en todas las categorías GO, no solo para la actividad de tirosina cinasa. Utilizaremos el paquete TopGO para este fin.  Y necesitaremos definir una función que seleccione los genes expresados de manera diferencial.

```r
library(topGO) #load the package
topDiffGenes <- function(allScore) return(allScore < 0.05)
```

Ahora estamos listos para hecer un nuevo objeto _topGO_. Buscaremos en las ontologías en el nivel de función molecular MF.

```r
GOdataMF <- new("topGOdata", ontology = "MF",
allGenes = pBH, geneSel = topDiffGenes, annot = annFUN.db,
affyLib = "hgu95av2.db")
```
Ahora podremos correr una prueba de _fisher_, para todos los procesos biológicos o utilizar algunos otros métodos (Bioinformatics, 2006, 22(13):1600-1607)

```r
resultFisher <- runTest(GOdataMF, algorithm = "classic", statistic = "fisher")
resultKS<- runTest(GOdataMF, algorithm = "classic", statistic = "ks")
resultFisher.elim<- runTest(GOdataMF, algorithm = "elim", statistic = "fisher")
resultKS.elim<- runTest(GOdataMF, algorithm = "elim", statistic = "ks")
```

Para mostrar los resultados

```r 
allRes <- GenTable(GOdataMF, classicFisher = resultFisher,
classicKS = resultKS,elimFisher = resultFisher.elim, elimKS = resultKS.elim,
orderBy = "elimKS", ranksOf = "classicFisher", topNodes = 20)
allRes
```
```
        GO.ID                                        Term Annotated
1  GO:0005509                         calcium ion binding        78
2  GO:0004714 transmembrane receptor protein tyrosine ...         7
3  GO:0003785                       actin monomer binding         3
4  GO:0004515 nicotinate-nucleotide adenylyltransferas...         3
5  GO:0051019    mitogen-activated protein kinase binding        10
6  GO:0004871                  signal transducer activity       188
7  GO:0042379                  chemokine receptor binding        10
8  GO:0038023                 signaling receptor activity        91
9  GO:0005516                          calmodulin binding        25
10 GO:0008201                             heparin binding        16
11 GO:0005088 Ras guanyl-nucleotide exchange factor ac...        26
12 GO:0015276           ligand-gated ion channel activity         9
13 GO:0005520          insulin-like growth factor binding         4
14 GO:0031730             CCR5 chemokine receptor binding         8
15 GO:0048020              CCR chemokine receptor binding         8
16 GO:0044325                         ion channel binding        27
17 GO:0030515                              snoRNA binding         4
18 GO:0003735          structural constituent of ribosome        25
19 GO:0050699                           WW domain binding         5
20 GO:0017112 Rab guanyl-nucleotide exchange factor ac...         6
   Significant Expected Rank in classicFisher classicFisher classicKS
1            9     3.57                    20       0.00802   0.00017
2            1     0.32                   191       0.27977   0.00028
3            3     0.14                     3       9.3e-05   0.00250
4            3     0.14                     4       9.3e-05   0.00250
5            3     0.46                    21       0.00882   0.00354
6           12     8.60                   147       0.14534   4.2e-06
7            1     0.46                   235       0.37447   0.00364
8            9     4.16                    38       0.02102   0.00050
9            1     1.14                   320       0.69179   0.00747
10           1     0.73                   279       0.52843   0.00801
11           5     1.19                    15       0.00554   0.00919
12           0     0.41                   388       1.00000   0.00925
13           1     0.18                   152       0.17089   0.00944
14           1     0.37                   204       0.31282   0.01124
15           1     0.37                   205       0.31282   0.01124
16           7     1.23                     6       0.00014   0.01204
17           0     0.18                   389       1.00000   0.01243
18           0     1.14                   390       1.00000   0.01295
19           0     0.23                   391       1.00000   0.01346
20           2     0.27                    47       0.02755   0.01445
   elimFisher  elimKS
1     0.00802 0.00017
2     0.27977 0.00028
3     9.3e-05 0.00250
4     9.3e-05 0.00250
5     0.00882 0.00354
6     0.52856 0.00360
7     0.37447 0.00364
8     0.27691 0.00717
9     0.69179 0.00747
10    0.52843 0.00801
11    0.00554 0.00919
12    1.00000 0.00925
13    0.17089 0.00944
14    0.31282 0.01124
15    0.31282 0.01124
16    0.00014 0.01204
17    1.00000 0.01243
18    1.00000 0.01295
19    1.00000 0.01346
20    0.02755 0.01445
```

Por último queremos buscar genes en una de las categorías GO. La función printGenes, está hecha para esa tarea. Hagámoslo para la actividad proteína tirosina cinasa (_GO:0004713_) como un ejemplo.

```r
gt <- printGenes(GOdataMF,
whichTerms = "GO:0004713", chip = "hgu95av2.db",
numChar = 40)
gt
```

```
              Chip ID LL.id Symbol.id
1636_g_at   1636_g_at    25      ABL1
39730_at     39730_at    25      ABL1
1635_at       1635_at    25      ABL1
40480_s_at 40480_s_at  2534       FYN
2039_s_at   2039_s_at  2534       FYN
754_s_at     754_s_at   613       BCR
36643_at     36643_at    NA      <NA>
38662_at     38662_at  5610   EIF2AK2
34877_at     34877_at  3716      JAK1
537_f_at     537_f_at   613       BCR
2057_g_at   2057_g_at  2260     FGFR1
1007_s_at   1007_s_at    NA      <NA>
760_at         760_at  8445     DYRK2
1333_f_at   1333_f_at   613       BCR
2056_at       2056_at  2260     FGFR1
854_at         854_at   640       BLK
41594_at     41594_at  3716      JAK1
1065_at       1065_at  2322      FLT3
34583_at     34583_at  2322      FLT3
33804_at     33804_at  2185     PTK2B
40742_at     40742_at  3055       HCK
1498_at       1498_at  7535     ZAP70
40936_at     40936_at 51232     CRIM1
36264_at     36264_at  4145      MATK
1844_s_at   1844_s_at  5604    MAP2K1
1810_s_at   1810_s_at  5580     PRKCD
36909_at     36909_at  7465      WEE1
993_at         993_at  7297      TYK2
39931_at     39931_at  8444     DYRK3
36115_at     36115_at  1198      CLK3
2059_s_at   2059_s_at  3932       LCK
2075_s_at   2075_s_at    NA      <NA>
33238_at     33238_at  3932       LCK
292_s_at     292_s_at  1195      CLK1
32833_at     32833_at  1195      CLK1
1622_at       1622_at  5606    MAP2K3
1768_s_at   1768_s_at  1445       CSK
38118_at     38118_at  6464      SHC1
1008_f_at   1008_f_at  5610   EIF2AK2
1512_at       1512_at  1859    DYRK1A
36946_at     36946_at  1859    DYRK1A
1630_s_at   1630_s_at  6850       SYK
1130_at       1130_at  5604    MAP2K1
1402_at       1402_at  4067       LYN
32616_at     32616_at  4067       LYN
36885_at     36885_at  6850       SYK
                                             Gene name raw p-value
1636_g_at  c-abl oncogene 1, non-receptor tyrosine ...    9.00e-11
39730_at   c-abl oncogene 1, non-receptor tyrosine ...    5.73e-10
1635_at    c-abl oncogene 1, non-receptor tyrosine ...    1.95e-07
40480_s_at       FYN oncogene related to SRC, FGR, YES    0.000341
2039_s_at        FYN oncogene related to SRC, FGR, YES    0.002151
754_s_at                     breakpoint cluster region    0.027205
36643_at                                            NA    0.028651
38662_at   eukaryotic translation initiation factor...    0.036347
34877_at                                Janus kinase 1    0.048349
537_f_at                     breakpoint cluster region    0.065386
2057_g_at          fibroblast growth factor receptor 1    0.065447
1007_s_at                                           NA    0.080622
760_at     dual-specificity tyrosine-(Y)-phosphoryl...    0.084581
1333_f_at                    breakpoint cluster region    0.116256
2056_at            fibroblast growth factor receptor 1    0.136657
854_at                      B lymphoid tyrosine kinase    0.142854
41594_at                                Janus kinase 1    0.158560
1065_at                  fms-related tyrosine kinase 3    0.224691
34583_at                 fms-related tyrosine kinase 3    0.225452
33804_at                protein tyrosine kinase 2 beta    0.288513
40742_at                       hemopoietic cell kinase    0.328005
1498_at    zeta-chain (TCR) associated protein kina...    0.339735
40936_at   cysteine rich transmembrane BMP regulato...    0.340416
36264_at      megakaryocyte-associated tyrosine kinase    0.439155
1844_s_at  mitogen-activated protein kinase kinase ...    0.486525
1810_s_at                      protein kinase C, delta    0.502015
36909_at                     WEE1 G2 checkpoint kinase    0.577787
993_at                               tyrosine kinase 2    0.577787
39931_at   dual-specificity tyrosine-(Y)-phosphoryl...    0.659157
36115_at                             CDC-like kinase 3    0.710543
2059_s_at  lymphocyte-specific protein tyrosine kin...    0.727480
2075_s_at                                           NA    0.754511
33238_at   lymphocyte-specific protein tyrosine kin...    0.768520
292_s_at                             CDC-like kinase 1    0.786830
32833_at                             CDC-like kinase 1    0.799613
1622_at    mitogen-activated protein kinase kinase ...    0.807841
1768_s_at                        c-src tyrosine kinase    0.888941
38118_at   SHC (Src homology 2 domain containing) t...    0.896863
1008_f_at  eukaryotic translation initiation factor...    0.909363
1512_at    dual-specificity tyrosine-(Y)-phosphoryl...    0.918548
36946_at   dual-specificity tyrosine-(Y)-phosphoryl...    0.924559
1630_s_at                       spleen tyrosine kinase    0.929048
1130_at    mitogen-activated protein kinase kinase ...    0.932251
1402_at    v-yes-1 Yamaguchi sarcoma viral related ...    0.969757
32616_at   v-yes-1 Yamaguchi sarcoma viral related ...    0.981146
36885_at                        spleen tyrosine kinase    0.999958
```

Este ejercicio está basado y adaptado en una práctica de Stale Nygard de Bioinformatics core facility, OUS/UiO.

Código Completo:

{{< gist friveroll 2691965 >}}

{{< bibliography "bibliography\microarreglos-bib.json" >}}
