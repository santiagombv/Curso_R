---
title: 'Práctico 6. Otros paquetes gráficos: *lattice* y *ggplot2*.'
output: 
  html_document:
    highlight: tango
---

##Gráficos del paquete *lattice*.
El paquete lattice ofrece flexibilidades especiales para representar interacciones y particularmente datos multivariados. Sus funciones son independientes de las funciones gráficas principales y se caracterizan por tener una gramática ligeramente distinta.   

```{r, eval=FALSE}
library (lattice)

# complete la ruta al directorio en ...
fum <- read.table(".../fumadores.txt", header=T)

# gráficos bivariados
# Notar que ingresar los datos como una fórmula es más eficiente
xyplot(ca.pulm ~ alt, data = fum)
xyplot(ca.pulm ~ alt | edad, data = fum)
xyplot(ca.pulm ~ alt | fuma, data = fum)
xyplot(ca.pulm ~ alt | fuma*sexo, data = fum)
xyplot(ca.pulm ~ alt, groups = fuma, data = fum)

# ayuda de la funcion básica xyplot
?xyplot

# manipular alguna de sus opciones...
xyplot(ca.pulm ~ alt, groups = fuma, data = fum, xlab = "altura", 
       ylab = "capacidad\npulmonar", col = c("red", "black"), pch = 19, 
       lwd = 2, key = list(text = list(c("no fumadores", "fumadores")), 
                           space = "bottom", 
                           points = list(pch = 19, col = c("red", "black"))))

# GRÁFICOS DE CAJAS
bwplot(ca.pulm ~ fuma | sexo, data = fum)

# HISTOGRAMAS Y DIAGRAMAS DE DENSIDAD
histogram(~ ca.pulm, data = fum)
densityplot(~ ca.pulm | fuma, data = fum)
```

##Gráficos del paquete *ggplot2*
Este paquete ofrece gráficos elegantes e intuitivos. Sin embargo, su escritura posee una gramática propia (una especie de "dialecto" dentro de R), por lo que requiere cierto tiempo hasta lograr su aprendizaje. La ayuda básica para *ggplot2* pude hallarse en la página http://ggplot2.tidyverse.org/reference/  
Un gráfico de *ggplot2* debe contar de al menos estos tres elementos:  

* ggplot: función principal donde se especifica el set de datos y se crea el gráfico.   
* aes: Especifica las variables y además  colores, forma, transparencia, tipo de línea, etiquetas de los ejes, etc.  
* geoms: objetos geométricos como puntos, barras, líneas, etc. En la práctica indica el tipo de gráfico a realizar.  

```{r, eval=FALSE}
library(ggplot2)

# complete la ruta al directorio en ...
fum <- read.table(".../fumadores.txt", header=T)

ggplot(data = fum, aes(x = edad, y = ca.pulm)) + geom_point()

# notar como los gráficos se van acumulando
g1 <- ggplot(data = fum, aes(x = edad, y = ca.pulm))
g2 <- g1 + geom_point()
g2

# modificando algunos detalles comunes
g1 <- ggplot(data = fum, aes(x = edad, y = ca.pulm, color = fuma)) 
g2 <- g1 + geom_point(size=3, aes(shape=sexo)) 
g3 <- g2 + xlab("Edad")
g4 <- g3 + ylab("Capacidad Pulmonar")
g5 <- g4 + theme_bw()
g5

# modificando los colores 
g1 <- ggplot(data = fum, aes(x = edad, y = ca.pulm, color = fuma)) 
g2 <- g1 + scale_colour_manual(values=c("red", "black"))
g3 <- g2 + geom_point(size=3) 
g3

# modificando los colores con una escala continua
g1 <- ggplot(data = fum, aes(x = edad, y = ca.pulm, color = alt)) 
g2 <- g1 + geom_point(size=3) 
g2

# modificando tamaños con una escala continua
g1 <- ggplot(data = fum, aes(x = edad, y = ca.pulm, size = alt, color = fuma)) 
g2 <- g1 + geom_point() 
g2

# Faceting (división del la ventana gráfica como en lattice)
g1 <- ggplot(data = fum, aes(x = edad, y = ca.pulm, color = fuma))
g2 <- g1 + geom_point(size=3) + theme_bw()
g3 <- g2 + facet_grid(. ~ sexo)
g3

g1 <- ggplot(data = fum, aes(x = edad, y = ca.pulm, color = fuma))
g2 <- g1 + geom_point(size = 3) + theme_bw()
g3 <- g2 + facet_grid(fuma ~ .) + scale_color_manual(values = c("red", "blue"))
g3

# agregar líneas de regresión
g1 <- ggplot(data = fum, aes(x = edad, y = ca.pulm, color = fuma))
g2 <- g1 + geom_point(size = 3) + theme_bw()
g3 <- g2 + geom_smooth(method = "lm")
g3

# gráficos de cajas
g1 <- ggplot(data = fum, aes(x = fuma, y = ca.pulm))
g2 <- g1 + geom_boxplot()
g2

# histogramas
g1 <- ggplot(data = fum, aes(x = ca.pulm))
g2 <- g1 + geom_histogram(fill="red")
g2
```

##Ejercicios
1. Utilizar los datos de turnera.txt para  hacer una regresión con dummy para el polen.tot en función del morfo y del área del pétalo. Representar los resultados utilizando lattice (sin líneas de regresión).  
2. Utilizando los mismos datos realice gráficos de densidad para la altura del estambre (alt.estambre) según el morfo, utilizando lattice y ggplot2.  
3. Utilizar los datos de Bahamas2.txt para construir una superficie en la cual el logaritmo de la densidad del pez loro (Parrot) se encuentre en función de la riqueza de especies de coral (CoralRichness) y de la cobertura de coral (CoralTotal).  
