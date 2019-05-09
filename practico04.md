---
title: "Práctico 4. Modelos Lineales Múltiples II."
output: 
  html_document:
    highlight: tango
---

##Caso 1.
Se intenta realizar un modelo que prediga el contenido de azúcar en el néctar en flores de Salvia polystachia, a partir de 7 variables morfológicas medidas. Los datos se encuentran en el archivo  s_poly.txt.

```{r, eval=FALSE}
# complete la ruta al directorio en ...
dat <- read.table(".../s_poly.txt", header = TRUE)

# exploración de los datos
layout(matrix(1:8,2,4))
plot(dat$azucar); plot(dat$largo.l.tot); plot(dat$largo.tubo) 
plot(dat$caliz.sup); plot(dat$caliz.med); plot(dat$caliz.inf) 
plot(dat$labio.sup); plot(dat$labio.inf)
layout(1)

which.max(dat$azucar)
dat <- dat[-101, ]

# exploración bivariada de los datos
pairs(dat)

# Detección de la colinealidad
library(car)

# A) Gráficos SPLOM
scatterplotMatrix(~ largo.l.tot + largo.tubo + caliz.sup + caliz.med +
   caliz.inf + labio.sup + labio.inf, #variables a graficar
   reg.line=lm,            #añadir rectas de regresión a los gráficos
   smooth=TRUE,            #añadir curvas suavizadas a los gráficos
   diagonal = 'density',   #gráficos de la diagonal
   data = dat)             #conjunto de datos 

# B) Matrices de correlación
CORR <- cor(dat[, c("largo.l.tot", "largo.tubo", "caliz.sup",
   "caliz.med", "caliz.inf", "labio.sup", "labio.inf")], 
   use = "complete.obs") 
CORR

# C) Factores de inflación de la varianza
fit <- lm(azucar ~ largo.l.tot + largo.tubo + caliz.sup + caliz.med +
          caliz.inf + labio.sup + labio.inf, data = dat)
vif(fit)	

# Decisión: usar el criterio de r < 0.7 y parcialmente los vif,
# descartamos el cáliz inferior y construimos un nuevo modelo

fit2 <- lm(azucar ~ largo.l.tot + largo.tubo + caliz.sup + caliz.med +
           labio.sup + labio.inf, data = dat)
summary(fit2)
anova (fit2)

layout(matrix(1:4, 2, 2))
plot(fit2)
layout(1)

# revisando su colinealidad nuevamente
vif(fit2)

# Selección de modelos. Best subset selection.
# Construyendo todos los modelos posibles
# (pueden utilizarse forward o backward cambiando  el argumento method)

library(leaps)
BS.fit <- regsubsets(azucar ~ largo.l.tot + largo.tubo + caliz.sup +
                     caliz.med + labio.sup + labio.inf, data = dat, 
                     method = "exhaustive")
BS.summary <- summary(BS.fit)
BS.summary

plot(BS.summary$cp, xlab = "número de variables", ylab = "Cp", type ="l")
plot(BS.summary$bic, xlab = "número de variables", ylab = "BIC", type ="l")

plot(BS.fit, scale = "Cp")
plot(BS.fit, scale = "bic")

coef(BS.fit, 3)
coef(BS.fit, 1)

# stepAIC (MASS) realiza una búsqueda automática del mejor modelo 
library(MASS) 
stepAIC(fit2, direction ="both") #para usar AIC

#para usar BIC cambio los grados de libertad de la penalización
n<-nrow(dat)
stepAIC(fit2, direction="both", k=log(n))#ajustar el modelo final

# Model averaging. Uso básico de MuMIn

library(arm)
library(MuMIn)

global <- lm(azucar ~ largo.l.tot + largo.tubo + caliz.sup + caliz.med +
             labio.sup + labio.inf, data = dat, na.action = na.fail)

std.model <- standardize(global, standardize.y = FALSE)

set <- dredge(std.model)
set

top.mod <-get.models(set, subset = delta<2)
top.mod

AVG<-model.avg(top.mod)
summary(AVG)

# Selección de Modelos. Shrinkage
# Lasso

library(glmnet)
x <- model.matrix(azucar ~ largo.l.tot + largo.tubo + caliz.sup + caliz.med +
                  labio.sup + labio.inf, data = dat)[,-1]
y <- dat$azucar

lasso.mod <- glmnet(x, y, alpha = 1)
plot(lasso.mod)
coef(lasso.mod, s = 0.1) # ejemplo

# Cross validation.
grid <- 10^seq(10, -10, length = 100)
cv.out <- cv.glmnet(x, y, alpha = 1, nfolds = 10, lambda = grid)
plot(cv.out)

grid <- 10^seq(0.5, -4, length = 100)
cv.out <- cv.glmnet(x, y, alpha = 1, nfolds = 10, lambda = grid)
plot(cv.out)

cv.out$lambda.min
coef(cv.out, s = "lambda.min")
```

##Ejercicios.
1. Los datos del archivo Loyn.txt corresponden a un estudio donde la densidad de aves (ABUND) se midió en 56 parches del sur de Victoria, Australia. El objetivo del estudio es determinar cuáles características del hábitat explican esa abundancia. Para ello se midió: tamaño del parche (AREA), distancia al parche más cercano (DIST), distancia al parche grande más cercano (LDIST), altura s.n.m. (ALT), años de aislamiento (YR.ISOL) y un índice de pastoreo (GRAZE). Explorar gráficamente los datos y examinar si las variables AREA, DIST Y LDIST requieren una transformación. Examinar la colinealidad de las variables y seleccionar un modelo adecuado.   
2. Los datos del archivo fumadores.txt corresponden a un estudio donde se intenta explicar la capacidad pulmonar en función de dos variables continuas (edad y altura) y dos variables discretas (fumar = si/no; sexo = m/f). Observar cuidadosamente la colinealidad entre las variables (incluyendo las variables discretas).   
