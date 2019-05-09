---
title: "Práctico 2. Modelos Lineales Simples"
output: 
  html_document:
    highlight: tango
---

##Caso 1. 
Se examina la relación entre la altura y la capacidad pulmonar. Los datos se encuentran en el archivo fumadores.txt.   
```{r, eval=FALSE} 
# complete la ruta al directorio en ...
fum <- read.table(".../fumadores.txt", header = TRUE)
fum 

# inspección de datos 1: gráfico tipo Cleveland
plot(fum$ca.pulm)
plot(fum$alt)

# inspección de datos 2: gráfico bivariado
plot(fum$alt, fum$ca.pulm)

# modelo lineal (regresión)
fit<-lm(ca.pulm ~ alt, data=fum)

# componentes del objeto fit (algunos)
fit
fit$coefficients
fit$residuals[1:20] # sólo los primeros 20
fit$fitted[1:20] # sólo los primeros 20

# resúmenes de información
summary(fit)
anova(fit)

# Diagnósticos gráficos. Layout dividirá la ventana gráfica en lo que    
# indique la matriz (4 en este caso, en 2 filas y 2 columnas). Para que la   
# ventana gráfica vuelva a la normalidad usaremos layout(1)
layout(matrix(1:4,2,2))
plot(fit)
layout(1)

# Diagnósticos numéricos
shapiro.test(fit$residuals)

# Validación cruzada
library(boot)
git1 <- glm(ca.pulm ~ alt, data=fum) #modelo generalizado normal, equivale a lm
git2 <- glm(ca.pulm ~ I(alt^2), data=fum) #modelo generalizado normal, equivale a lm
cv.err1 <- cv.glm(fum, git1, K = 10)
cv.err2 <- cv.glm(fum, git2, K = 10)
cv.err1$delta
cv.err2$delta
```

##Caso 2. 
Muertes de mosquitos en respuesta a distintos insecticidas. Datos de ejemplo cargado en R: InsectSprays.   
```{r, eval=FALSE}
data(InsectSprays)
head(InsectSprays)

plot(sqrt(count) ~ spray, data = InsectSprays)

# utilizando la function lm
fit.spray1 <- lm(sqrt(count) ~ spray, data = InsectSprays)
anova(fit.spray1)
summary(fit.spray1)
# utilizando la function aov
fit.spray2 <- aov(sqrt(count)~spray, data = InsectSprays)
summary(fit.spray2)

# diagnósticos gráficos
layout(matrix(1:4,2,2))
plot(fit.spray2)
layout(1)

# diagnósticos numéricos
shapiro.test(resid(fit.spray2))
bartlett.test(resid(fit.spray2)~InsectSprays$spray)

# Test de Tukey
# notar que trabaja sobre un objeto aov, no sobre un objeto lm
tuk<-TukeyHSD (fit.spray2)
tuk
```

##Ejercicios

1. Se intenta probar si el éxito reproductivo (medido como polinarios exportados) de la orquídea *Cyclopogon elatus* aumenta en flores con nectarios más profundos. El archivo cyclop.txt contiene las variables de interés nectario y pol.exp. Realizar gráficos exploratorios, ajustar un modelo de regresión lineal, extraer la tabla de parámetros y la tabla ANOVA y realizar gráficos de diagnóstico. ¿Qué opina de transformar los datos para satisfacer los requisitos del modelo lineal?    
     
2. Determinar si existen diferencias en el peso de pecaríes según el mes de su captura: febrero, mayo, agosto y noviembre, con los datos del archivo pecaries.txt. Explorar los datos gráficamente, realizar un análisis de la varianza, obtener la tabla de parámetros y la tabla ANOVA. Realizar diagnósticos gráficos y numéricos  (pruebas de Shapiro-Wilks y Bartlett), y realizar un test de Tukey.  
