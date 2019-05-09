---
title: "Práctico 1. Introducción al Lenguaje R"
output:
  html_document:
    highlight: tango
---


##Creación de objetos
R es un lenguaje orientado a objetos: cada comando crea un objeto que debe ser "nombrado" para que permanezca en la memoria utilizando una ```<-```  y su nombre debe ser "invocado" para que aparezca en pantalla. A pesar de ser un lenguaje de programación, R es comparativamente simple e intuitivo.

##Objetos comunes en R.
--------------------- ------------------------------------------------------------------------------------
**data.frame**        Objeto que contiene datos. Habitualmente se crea al importar datos 
                      externos, aunque pueden ser creados dentro del mismo R. 
                      Consiste en una serie de variables (*vectors*) de igual longitud y que pueden 
                      ser del mismo tipo o no.  

**vectors**           Colección de datos del mismo tipo (números, letras, etc.) que puede ser creado 
                      dentro de R o importado como una columna de un *data.frame*. Existen muchos 
                      tipos de vectores, entre ellos:  
                      *numeric* consiste de números reales;  
                      *integer* consiste de números enteros;  
                      *character* contiene letras, nombres, etc. (notar que cada elemento se encuentra entre
                      comillas, por ej. "A1"); 
                      *factor* sirve para representar variables categóricas, porta información sobre los niveles
                      del factor (*levels*) y sobre si estos niveles siguen un orden o no.  

**matrix**            Matriz formada por la unión de vectores de un mismo tipo y largo, por un solo vector 
                      que es partido en columnas y filas o (más habitualmente) producto de ciertas funciones, 
                      por ejemplo *cor*, que construye matrices de correlación. 
                      
**list**              Objeto que compuesto de objetos de distinto tipo y largo.
--------------------- ------------------------------------------------------------------------------------

\pagebreak

##Cuatro símbolos básicos: # <- ? c

```{r, eval=FALSE}
# El símbolo # desactiva el espacio a su derecha.
# Es útil para poner aclaraciones en nuestras rutinas.

# El comando más básico: Asignar crea objetos.
n <- 15  
n   #escribir el nombre de un objeto es "invocarlo"

# al usar de nuevo el mismo nombre el objeto anterior se pierde ("pisar")
n <- 46 + 12  				 
n

# los caracteres categóricos necesitan ser ingresados con comillas
di <- "A"
di

# Obtener ayudas, el símbolo ?
# si tenemos dudas sobre el funcionamiento de una función
? mean
? lm

# Si desconocemos el nombre de una función para realizar determinada
# tarea, puede realizarse una búsqueda con ?? (en los paquetes activos)
?? "linear models"

# crear vectores. La función concatenar c
vector <- c(1, 2, 3, 4)
vector <- c(1:4)
vector <- c("a", "b", "c", "d")
vector <- c("1", "a", "2", "b") 
```


##Funciones básicas: creación de vectores, secuencias y matrices
Las funciones de esta sección son las más sencillas de R. Como regla general las funciones siempre deben escribirse acompañadas de paréntesis, que contienen los elementos sobre los cuales la función actuará. Si no se colocan los paréntesis el programa mostrará el código mismo de la función.

```{r, eval=FALSE}
### vectores ###
# Ya vimos c concatenar
vec <- c(1, 4, 6, 3, 7)
vec

# vectores de repetición. rep(lo_que_queremos_repetir, cuántas veces)
# esta función tiene dos argumentos, separados por una coma
xx <- rep("A", 50) 
xx

# secuencias seq()
seq1 <- c(1:50)  # el símbolo : indica desde:hasta
seq1

seq2 <- seq(50) # estructura: desde uno hasta (...)
seq2

seq3 <- seq(8,50) # estructura: (desde..., hasta...)
seq3

seq4 <- seq(from = 0, to = 0.99, by = 0.01) # argumentos explícitos
seq4

seq5 <- seq(0, 0.99, 0.01) # argumentos implícitos
seq5

seq6 <- seq(5, 10, length.out=20)
seq6

# combinando vectores de a pares: cbind y rbind
az <- cbind(seq1, xx)
az

za <- rbind(c(1:25), rnorm(25))	
za

# construyendo una matriz: matrix()
m <- c(1:20)
matriz1 <- matrix(m, 4, 5) # (vector a usar, filas, columnas) 
matriz1

# notar que es exactamente igual a 
matriz2 <- matrix(c(1:20), 4, 5)
matriz2
```

##Preparación e ingreso de datos: las funciones *read.table* y *read.csv*.
Los datos pueden prepararse con cualquier software para planillas de datos como Excel o LibreOffice Calc. 
Se recomienda:  
- Que los nombres de las columnas no tengan espacios ni comiencen con números.    
- Evitar los símbolos extraños como %, &, ^, ~, ñ, etc.  
- Utilizar nombres cortos.   
- Los datos faltantes no deben dejarse en blanco, sino señalarse con NA.    

Una vez lista la planilla se recomienda guardarla en formato .txt o .csv (si bien R admite otros formatos). Pueden guardarse en cualquier carpeta del equipo. En el caso de que se escriba cada vez la ruta para encontrar ese archivo, es recomendable ubicar la carpeta cerca de la raíz del equipo (normalmente C:/ en Windows o /home en Ubuntu). En este manual se supondrá que los archivos de datos se han guardado en C:/RD. Puede evitarse escribir la ruta si al abrir R seleccionamos la carpeta que donde los archivos están guardados con la opción Archivo - cambiar dir o usando la función *setwd* (para examinar cual es la carpeta en uso, utilizar *getwd*). Esta opción es muy útil si vamos a abrir varios archivos de datos guardados en el mismo directorio. Finalmente, puede abrirse una ventana de búsqueda para seleccionar el archivo con la opción *file.choose()*.
   
\pagebreak

```{r, eval=FALSE}
# la función read.table
# opción 1 con ruta completa (notar orientación de las barras /)
# en este caso asumimos que se encuentra en el directorio RD
datos <- read.table("C:/RD/peces1.txt", header=TRUE)

# opción 2 seleccionando el directorio en uso previamente desde "Archivo"
# o con setwd
setwd("C:/RD/")
datos <- read.table("peces1.txt", header=TRUE)

# opción 3 Abre una ventana de búsqueda 
# desventaja: requiere que el humano piense!
datos <- read.table(file.choose(), header=TRUE)

# opción 4 la función read.csv
datos <- read.csv("C:/RD/peces1.csv", header=TRUE)

# los datos ya están guardados, pero si queremos verlos...
datos    # no siempre es buena idea, sobre todo si son muchos

# una forma práctica de ver solo las primeras filas es con head 
head(datos)

# o echar un vistazo a la estructura de los datos
# str informa sobre la estructura de cualquier objeto
str(datos)

# Información básica del set de datos
nrow(datos)     #Número de filas
ncol(datos)     #Número de columnas
names(datos)    #Nombre de las columnas
```

##Indexación
Estas operaciones se realizan para extraer de un objeto la parte que nos interesa, por ejemplo una columna con una variable de un marco de datos.   
**No usar attach(),** esta función aumenta las probabilidades de confundirse e introducir errores.

```{r, eval=FALSE}
# Para seleccionar una columna del marco de datos utilizamos $
datos$grupo

# los corchetes indican el contenido de un conjunto de datos, 
# matriz o vector. La coma separa filas de columnas

#columnas
datos[, 1]                           
datos[, "grupo"]

# grupos de columnas
datos[, 1:3]                         
datos[, c("grupo", "largo.a")]
names <- c("grupo", "largo.b")
datos[, names]

# filas y datos individuales
datos[2, ]                           
datos[2, 3]
datos[-2, 2]

# indexando por una condición
datos[datos$largo.a > 15, ]
datos[datos$largo.a > 15 & datos$grupo == "A", ]

# Indexación de vectores
vec <- datos$largo.a                 
vec[1]
vec[-1]
vec[vec > 15]

# omitir todos los NA de una base de datos
datos2 <- na.omit(datos)
datos2

# La mayoría de las funciones cuentan con dos modos alternativos de 
# escribirse. En el modo "fórmula" existe un argumento "data" que evita  
# indexar los nombres de las columnas usadas.

plot(datos$largo.a, datos$largo.b)      #modo por default: plot(x,y)

plot(largo.b ~ largo.a, data = datos)   #modo fórmula: plot(y~x, data)
```

##Modificación y creación de columnas.
```{r, eval=FALSE}
# reemplazo (al usar el mismo nombre) una variable numérica por un factor
datos$trat
datos$trat <- as.factor(datos$trat) 
datos$trat    # notar la lista de niveles del factor

# crear una nueva columna en una base de datos ya existentes
datos$pob <- c(rep("pob1", 15), rep("pob2", 15))
datos$pob <- as.factor(datos$pob) 
datos$log.largo.a <- log(datos$largo.a)
head(datos)

# crear una nueva columna uniendo clasificadores
datos$clave <- paste(datos$pob, datos$trat, sep = ".")
head(datos)

# Usando factor para arreglos más complicados
datos$pais <- factor(datos$pob, levels = c("pob2", "pob1"), 
                     labels = c("Bolivia", "Chile"))
head(datos)
```

\pagebreak

##Subdivisión de conjuntos de datos
```{r, eval=FALSE}
# Subdivisión de un conjunto de datos
# Símbolos lógicos: 
# == (igual)
# != (distinto), 
# > (mayor), 
# < (menor)
# >= (mayor o igual), 
# <= (menor o igual).

dat1 <- subset(datos, datos$grupo == "A")
dat1

dat2 <- subset(datos, datos$grupo == "A" & datos$trat == 2)
dat2

# Crear una lista de bases de datos
ldat <- split(datos, f = datos$grupo) 
ldat
ldat[["B"]] # indexacion especial
```

##Funciones para extraer información de una columna o de un vector
```{r, eval=FALSE}
class(xx)
length(seq1)
dim(matriz1)
max(datos$largo.a)
which.max(datos$largo.a)
min(datos$largo.a)
which.min(datos$largo.a)
```


##Funciones estadísticas básicas
Se detallan abajo algunas como la media, varianza, desvío estándar, etc. En general si cualquiera de ellas se aplica sobre datos que contienen NA el resultado será NA también, por lo cual hay indicarle a la función que los elimine. Una alternativa es eliminar previamente todas las filas con datos faltantes usando la función na.omit.
```{r, eval=FALSE}
# media
M <- mean(datos$largo.a, na.rm = TRUE)
M

# desvío estándar
S <- sd(datos$largo.a, na.rm = TRUE)
S

# varianza
V <- var(datos$largo.a, na.rm = TRUE)
V

# sobre un conjunto de datos, se obtiene una matriz de varianza-covarianza
VC <- var(datos[,c(3,4)], na.rm = TRUE)
VC

# de forma parecida, puede obtenerse una matriz de correlación
CO <- cor(datos[, c(3, 4)], use="complete.obs")  #complete.obs elimina NA
CO

# en cambio, para realizar un test de correlación
cor.test(datos$largo.a, datos$largo.b, method = "pearson")

# resumen de datos
summary(datos)

# una función útil para aplicar una función a un subconjunto de los datos 
# es aggregate, con una estructura ligeramente más complicada

MG<-aggregate(datos$largo.a, by=list(datos$grupo), FUN=mean, na.rm=TRUE)
MG
```


Otras funciones útiles y miscelánea    

* Editar - Limpiar Consola (Ctrl+L): Borra la consola de R, pero no remueve los objetos de la memoria.    

* Detener el cálculo actual. STOP o presionar Esc.     

* Misc - Listar Objetos: Muestra los objetos guardados en la memoria.  

```{r, eval=FALSE}
ls()
```

* Misc - Remover todos los objetos: Elimina todos los objetos guardados en la memoria.    

```{r, eval=FALSE} 
rm(list = ls(all = TRUE))
```

--------------------------------------------------------------------------------------------------------------------
   
> **QUÉ SIGUE:** Manipular bases de datos es un campo en desarrollo. El paquete *dplyr* de Hadley Wickham es altamente recomendable (https://cran.r-project.org/web/packages/dplyr/). Para una excelente introducción a *dplyr* consultar http://www.dataschool.io/dplyr-tutorial-part-2/ .   
   
---------------------------------------------------------------------------------------------------------------------
