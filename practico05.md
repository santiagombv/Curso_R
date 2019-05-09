---
title: "Práctico 5. Manejo de gráficos y representación gráfica de modelos lineales."
output: 
  html_document:
    highlight: tango
---

##Comandos gráficos básicos
* Comandos gráficos de alto nivel: abren una ventana gráfica y crean un gráfico nuevo completo. Modificando sus opciones es posible cambiar colores, tamaños, tipo de fuente y otras características. 
* Comandos gráficos de bajo nivel: agregan más información a un gráfico ya existente, como puntos extra, líneas, funciones y leyendas. 
* Comandos gráficos interactivos: permiten agregar o extraer información de un gráfico existente, usando el mouse. 

##Uso de la función plot.
```{r, eval=FALSE}
# complete la ruta al directorio en ...
datos <- read.table(".../cyclop.txt", header = TRUE)

# UNA VARIABLE
plot(datos$nectario) # gráfico tipo Cleveland
plot(~datos$nectario)# Stripchart

# con los datos ordenados de menor a mayor
plot(sort(datos$nectario))

# cambiando el tipo de gráfico 
# probar los diferentes "type": p, l, b, o, h, s, S
plot(sort(datos$nectario), type = "h") 

# agregamos una línea horizontal señalando la posición de la media
# usando la función de bajo nivel abline
m.nec <- mean(datos$nectario, na.rm = TRUE)
abline(h = m.nec)

# DOS VARIABLES
plot(datos$nectario, datos$pol.exp)       # forma plot(x,y)
plot(pol.exp ~ nectario, data=datos)      # forma plot(y~x)

# manipulación de los PARAMETROS GRÁFICOS
plot(datos$nectario, datos$pol.exp,
     pch= 16,                             # tipo de símbolo
     col= "blue2",                        # color del símbolo
     xlab= "profundidad del nectario",    # nombre eje x
     ylab= "polinios exportados")         # nombre eje y

# agregando una línea de regresión
fit<-lm(datos$pol.exp ~ datos$nectario)
summary(fit)
abline(fit,	lty = 3, lwd = 2)
```

##Devices
Colocando ?Devices obtenemos la lista de formatos gráficos disponibles en R. No todos los formatos son accesibles en todos los sistemas.  
Los gráficos pueden guardarse (en Windows) con el botón derecho del mouse sobre el gráfico o utilizando Archivo - guardar como (la ventana gráfica debe estar en primer plano). Lo mismo puede hacerse utilizando comandos (veremos los principales) con la ventaja de un control más fino sobre la forma final del gráfico.  

```{r, eval=FALSE}
?Devices

# revisar donde se guardaran los gráficos
getwd()
# puede cambiarse con setwd()

# JPG
jpeg(file = "mi_grafico.jpg", 
     width = 12, height = 10,  # ancho y alto en lo que indique units
     units = "cm",             # unidades (pixeles por defecto)
     quality = 95,             # grado de no-compresión
     res = 300)                # resolución en puntos por pulgada 
plot(pol.exp ~ nectario, data=datos)  # comandos gráficos
dev.off()                      # cerrar el gráfico (importante!!)

# PDF
pdf(file = "mi_grafico.pdf",
    width = 7, height = 6,# ancho y alto en pulgadas (=2.54cm)
    paper = "a4")         # tamaño del papel, por defecto igual al gráfico
plot(pol.exp ~ nectario, data=datos)  # comandos gráficos
dev.off()                             # cerrar el gráfico

# EPS
postscript(file = "mi_grafico.eps",
    width = 7, height = 6,# ancho y alto en pulgadas (=2.54cm)
    paper = "a4",         # tamaño del papel, por defecto igual al gráfico
    horiz = TRUE)         # horizontal (FALSE) o vertical (TRUE)
plot(pol.exp ~ nectario, data=datos)  # comandos gráficos
dev.off()                             # cerrar el gráfico

# SVG
svg(file = "mi_grafico.svg",
    width = 7, height = 6)   # ancho y alto en pulgadas (=2.54cm)
plot(pol.exp ~ nectario, data=datos)  # comandos gráficos
dev.off()                             # cerrar el gráfico
```

## Otros gráficos sencillos con variables continuas.
```{r, eval=FALSE}
# histogramas
hist(datos$nectario, col = "orange1", main= "histograma",
     xlab = "nectario", ylab = "frecuencia")

# QQplots
qqnorm(datos$nectario)
```

##División de la ventana gráfica.
```{r, eval=FALSE}
# Básica con la función layout
layout(matrix(1:4, 2, 2))
hist(datos$nectario, col= "#CAFF70")
hist(datos$flores, col= "#BCEE68")
hist(datos$frutos, col= "#A2CD5A")
hist(datos$pol.exp, col= "#6E8B3D")
layout (1)

# Por defecto, layout() divide el dispositivo en dimensiones regulares: 
# esto se puede modificar con las opciones widths y heights.
m <- matrix(1:4, 2, 2)
layout(m, widths=c(1, 2),
heights = c(2, 1))
layout.show(4)
hist(datos$nectario, col = "#CAFF70")
hist(datos$flores, col = "#BCEE68")
hist(datos$frutos, col = "#A2CD5A")
hist(datos$pol.exp, col = "#6E8B3D")
layout(1)

# división de la ventana usando las opciones de par
par(mfrow = c(2, 2))
hist(datos$nectario, col = "#CAFF70")
hist(datos$flores, col = "#BCEE68")
hist(datos$frutos, col = "#A2CD5A")
hist(datos$pol.exp, col = "#6E8B3D")

par(mfcol = c(1, 4))
hist(datos$nectario, col = "#CAFF70")
hist(datos$flores, col = "#BCEE68")
hist(datos$frutos, col = "#A2CD5A")
hist(datos$pol.exp, col = "#6E8B3D")

# cerrar la ventana gráfica para desactivar par o
par(mfcol=c(1,1)) # deshacer lo anterior

# revisar las opciones gráficas de par
?par
```

##Gráficos de cajas.
```{r, eval=FALSE}
# complete la ruta al directorio en ...
peces <- read.table(".../peces.txt", header = TRUE)
peces$Species <- as.factor(peces$Species)

# dos formas alternativas de hacer box-plots
plot(peces$Species, peces$Weight)
boxplot(Weight ~ Species, data = peces, col = "light blue")
```

##Interacciones entre variables categóricas.
```{r, eval=FALSE}
# complete la ruta al directorio en ...
bival <- read.table(".../bivalvos.txt", header = TRUE)

# modelo 
fit <- lm(huevos ~ densidad*estacion, data = bival)
anova(fit)

library(sciplot)

# GRÁFICOS DE BARRAS
bargraph.CI(x.factor = densidad, # factor (eje x)
            response = huevos,   # respuesta (eje y)
            group = estacion,    # factor optativo (distintos colores)
            legend = TRUE,
            data = bival)

# GRAFICOS DE INTERACCIÓN
lineplot.CI(x.factor = densidad, response = huevos, group = estacion,
            data = bival, legend = TRUE, 
            col = c("red", "black"),  # modificación de varios parámetros
            pch = c(1,20),
            xlab = "densidad", 
            ylab = "número de huevos",
            trace.label = "estación")
```

##Gráficos a partir de predichos.
```{r, eval=FALSE}
# complete la ruta al directorio en ...
datos <- read.table(".../cyclop.txt", header = TRUE)

fit.c <- lm(pol.exp ~ flores + I(flores^2), data = datos)
summary(fit.c)

# A partir de los coeficientes
B <- coef(fit.c)
o <- order(datos$flores) # obtenemos el orden de x
Y1 <- B[1] + B[2]*datos$flores[o] + B[3]*datos$flores[o]^2
plot(datos$flores, datos$pol.exp)
points(datos$flores[o], Y1, type = "l")
# A partir de los predichos. 
Y2 <- predict(fit.c, datos[o, ])# los datos originales pueden reemplazarse
plot(datos$flores, datos$pol.exp)
points(datos$flores[o], Y2, type = "l")

# A partir de los predichos, con un set de datos nuevo y "suave"
FLO2<-seq(min(datos$flores, na.rm = TRUE),  # desde el mínimo
          max(datos$flores, na.rm = TRUE),  # hasta el máximo
          length.out = 200)                 # nueva longitud
Y3 <- predict(fit.c, newdata = data.frame(flores=FLO2))

plot(datos$flores, datos$pol.exp)
points(FLO2, Y3, type = "l")
```

##Interacciones de variables continuas: superficies.
Se necesitan datos uniformemente espaciados para construir superficies. Existen alternativas no paramétricas que "suavizan" los datos para darnos una idea mejor de la forma "real" de una superficie, en este caso lo haremos "a mano" usando un modelo lineal.   
```{r, eval=FALSE}
# complete la ruta al directorio en ...
datos <- read.table(".../cyclop.txt", header = TRUE)

fit <- lm(formula = pol.exp ~ flores * nectario + I(flores^2) + I(nectario^2), data = datos)

r.nec <- range(datos$nectario, na.rm = TRUE)
NEC <- seq(r.nec[1], r.nec[2], length.out = 100)
r.flo <- range(datos$flores, na.rm=T)
FLO <- seq(r.flo[1], r.flo[2], length.out = 100)

# Construcción de una grilla
grilla <- expand.grid(flores=FLO, nectario=NEC)

# predicción del eje Z
Z1<-predict(fit, newdata = grilla)
Z2<-matrix(Z1, 100, 100)
# GRAFICOS 3 D: DISTINTAS OPCIONES

# 2D, con curvas de nivel (tipo "mapa topografico")
contour(x=FLO, y=NEC, z=Z2, col = "Black")

# 2D, con un gradiente de color y curvas de nivel 
filled.contour(x=FLO, y=NEC, z=Z2, color = rainbow)

# crear paletas de color
grises <- grey(seq(0, 1, 0.1)) # 10 tonos de gris 
varios <- rainbow(10, start=0.1, end=0.5)

# 2D, con un gradiente de color y  sin curvas de nivel 
image(x = FLO, y = NEC, z = Z2, col = grises)

# Superposición de image, contour y points
image(x=FLO, y=NEC, z=Z2, col = varios, xlab = "número de flores", ylab = "nectario")
contour(x = FLO, y = NEC, z = Z2, col = "red", add = TRUE)
points(x = datos$flores, y = datos$nectario, col = "red", pch = 19) 

# 3D, con rejilla y en perspectiva
persp(x = FLO, y = NEC, z = Z2)

# manejo de los parámetros gráficos (sólo algunos)
persp(x = FLO, y = NEC, z = Z2, 
      border = "slateblue4",       # border: color de la rejilla
      col = "aquamarine2",         # col: color de la superficie
      xlab = "nectario",           # xlab, ylab, zlab: etiquetas ejes
      ylab = "número de flores", 
      zlab = "polinarios",
      theta = -45,                 # theta: ángulo de giro horizontal
      phi = 20,                    # phi: ángulo de rotación vertical
      ticktype = "detailed"        # para que ponga el valor de los ejes
) -> res                      # guardo la "forma" del gráfico 

# agregar de los puntos observados con la funcion trans3d
pru<-trans3d(x = datos$flores, y = datos$nectario, z = datos$pol.exp, pmat = res)
points(pru, col = "royalblue4", pch = 20)
```

##Ejercicios.
1. Usar los datos de  s_poly.txt para hacer gráficos bivariados, utilizar comandos gráficos de bajo nivel para darle colores y cambiar los símbolos y agregar una recta de regresión.
Guarde el gráfico obtenido como pdf.   
2. Represente gráficamente el resultado del ejercicio 2 del práctico 2:Determinar si existen diferencias en el peso de pecaríes según el mes de su captura: febrero, mayo, agosto y noviembre, con los datos del archivo pecaries.txt.   
