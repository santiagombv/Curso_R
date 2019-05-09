---
title: "Práctico 3. Modelos Lineales Múltiples I."
output: 
  html_document:
    highlight: tango
---

##Caso 1. 
Se estudió el efecto del número de parejas sexuales sobre la longevidad de moscas de la fruta macho. Los tratamientos fueron: A = control (sin hembras), B = control (una hembra no receptiva al día), C = control (8 hembras no receptivas al día), D = una hembra virgen al día, E = 8 hembras vírgenes al día. Ya que la longevidad puede estar relacionada con el tamaño del insecto se tomó el diámetro del tórax como covariable. Los datos se encuentran en el archivo moscas.txt.  

```{r, eval=FALSE}
# complete la ruta al directorio en ...
dat <- read.table(".../moscas.txt", header = TRUE)

plot(dat$torax)
plot(dat$vida)
plot(dat$trat,dat$vida)
plot(dat$torax,dat$vida)

#diferentes sumas de cuadrados
fit1.a <- lm(vida ~ trat*torax, data = dat)
fit1.b <- lm(vida ~ torax*trat, data = dat)
anova(fit1.a) #tipo I
anova(fit1.b)

library(car)
Anova(fit1.a, type = "II")
Anova(fit1.b, type = "II")
Anova(fit1.a, type = "III")
Anova(fit1.b, type = "III")

summary(fit1.a)

#selección del modelo
fit2 <- lm(vida ~ trat + torax, data = dat)
Anova(fit2, type = "II")
fit3 <- lm(vida ~ trat, data =dat)
Anova(fit3, type = "II")

layout(matrix(1:4, 2, 2))
plot(fit3)
layout(1)
```

##Ejercicios
1. Se busca un modelo que prediga la biomasa (peso.seco) de las almejas en función de su largo máximo (largo). Debido al ciclo de vida de las almejas, la relación biomasa-largo puede variar a lo largo del año, por lo que se ha tomado el mes de colecta (mes) como covariable. Los datos para el ejercicio se encuentran en el archivo almejas.txt.   

2. Se intenta determinar el efecto de la estación (IP= invierno/primavera vs. VO= verano/otoño) y la densidad de adultos (A=8, B=15, C=30, D=45 en 225 cm2) sobre la producción de huevos de bivalvos. Los datos para el ejercicio se encuentran en el archivo bivalvos.txt.   
