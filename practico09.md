---
title: 'Práctico 9: Modelos Randomizados y Modelos Nulos.'
output: 
  html_document:
    highlight: tango
---

##Caso 1. Modelos lineales randomizados.
Se intenta conocer si la cantidad de grasa en la leche de vaca es influida por su raza (Ayrshire, Canadian, Guernsey, Holstein-Fresian o Jersey). Se plantea en primer lugar un modelo lineal general. Ante la violación de los supuestos del modelo se realizará un modelo randomizado construyendo una tabla ad-hoc de pseudo-F.

```{r, eval=FALSE}
# complete la ruta al directorio en ...
vacas <- read.table(".../vacas.txt", header = TRUE)

fit <- lm(fat ~ breed, data = vacas)
summary(fit)
anova(fit)
layout(matrix(1:4, 2, 2))
plot(fit)  #examinar violación de supuestos
layout(1)

#Guardar las F observadas
Fobs <- anova(fit)[1, 4]
Fobs

#construir una function que repita los pasos del modelo
rd.aov <- function(Y, X){
  s.Y <- sample(x = Y, size = length(Y), replace = F)   #"reshuffling"
  fit <- lm(s.Y ~ X)                   #modelo lineal
  res <- anova(fit)[1, 4]              #extracción de las F
  c(res)                               #mostrar: un vector de F
}
					
#Probar la función varias veces (debe dar distinto resultado)
rd.aov(Y = vacas$fat, X = vacas$breed)
rd.aov(Y = vacas$fat, X = vacas$breed)
#Realizar réplicas para obtener la distribución de pseudo F
pseudoF <- replicate(1000, rd.aov(Y = vacas$fat, X = vacas$breed))
PF <- t(pseudoF)	#transposición para que cada F quede en una columna

#histograma para ver los resultados simulados y los observados
hist(PF)
abline(v = Fobs, col = "red")

hist(PF, xlim=c(0, 51))
abline(v = Fobs, col = "red")

#valores P
P.breed <- length(PF[PF >= Fobs]) / length(PF)
P.breed
#como nuestra simulación llegó hasta mil 
#no podemos asegurar que sea 0, sino P<0.001
```

##Caso 2: Un Monte Carlo sencillo.
Se intenta probar si la distribución espacial de árboles en un área de 4 km2 se aparta de una distribución aleatoria uniforme.
```{r, eval=FALSE}
# complete la ruta al directorio en ...
esp <- read.table(".../espMC.txt", header = TRUE)
plot(y ~ x, data=esp)

#calcular las distancias euclídeas entre las filas de la base de datos
#como parámetro observado se elige la media  de todas las distancias 

Dobs<-mean(dist(esp))		#revisar la ayuda de la función dist
Dobs

#Realizamos una simulación de la distribución de los árboles según una #distribución uniforme
X.azar <- runif(n = nrow(esp), min = 0, max = 2)
Y.azar <- runif(n = nrow(esp), min = 0, max = 2)
plot(X.azar, Y.azar)
Dazar <- mean(dist(cbind(X.azar, Y.azar)))
Dazar

#Repetimos la simulación 1000 veces y guardamos los resultados
Ds <- numeric(length(1000))
for (i in 1:1000) {
  X.azar <- runif(n=24, min=0, max=2)
  Y.azar <- runif(n=24, min=0, max=2)
  Ds[i] <- mean(dist(cbind(X.azar, Y.azar)))
}

#visualizamos los resultados con un histograma
hist(Ds)
abline(v = Dobs, col = "red")
#calcular el valor P
P <- length(Ds[Ds < Dobs]) / length(Ds)
P
```

##Ejercicios

1. Los datos del archivo nidos.txt contienen la cantidad de nidos de cierta especie de ave (nidos) presentes en árboles de diferentes diámetros (diametros) en 3 sitios diferentes (sitio).   
   + Realizar el modelo lineal correspondiente para probar la existencia de un efecto del sitio y del diámetro del árbol sobre la cantidad de nidos (añadir interacción).   
   + Realizar diagnósticos y discutir si se cumplen los supuestos de un modelo lineal.   
   + Aplicar un modelo lineal randomizado.   

2. El test de Mantel es utilizado para evaluar la asociación entre dos matrices de distancia (por ejemplo distancia genética y distancia geográfica) calculando la asociación entre dos matrices y comparando esta medida observada con la esperada por azar. La asociación entre matrices se calcula utilizando la correlación pareada entre los elementos de las matrices. Dado que las matrices X e Y son simétricas se utiliza solamente la mitad de ellas (arriba o abajo de la diagonal), esta mitad puede obtenerse por indexación.  

```{r, eval=FALSE}
# la mitad superior de la matriz X es 
X[col(X) > row(X)]
```

Obtener la asociación observada robs entre las dos matrices de distancia de los archivos yanomamaGEN y yanomamaGEO, correspondientes a las matrices de distancia geográfica y genética entre 19 aldeas Yanomama en Brasil. Realizar una randomización para obtener pseudo-valores de $r$ y obtener su distribución. Probar si $r_{obs}$ es significativa.   
Nota 1. Existen funciones para realizar el test de Mantel en el paquete *vegan*, puede confirmar su resultado consultándolas).   
Nota 2: evite usar el test de Mantel en la vida real, hay alternativas más adecuadas, por ejemplo protest en el mismo paquete *vegan*.  

3. Hall et al. (Evolution (2008) 62-9: 2305–2315) proponen un test mediante permutaciones para conocer si existen diferencias significativas entre la oportunidad para la selección (I) masculina y femenina que actúan en una población.
Hacer este test utilizando los datos de cyclopogon sabiendo que:  
   + La oportunidad de la selección (I) se define como la varianza del éxito reproductivo.  
   + Las medidas de éxito femenino y masculino son frutos y pol.exp.  
   
Para realizar este análisis la base de datos debe ser reordenada para agregar una variable de clasificación en masculino y femenino. Luego calcular la diferencia observada entre $I_{masc}$ e $I_{fem}$. Realizar una randomización para obtener 1000 pseudo-valores de esta diferencia y probar la significancia de la diferencia observada respecto a la distribución obtenida.  
