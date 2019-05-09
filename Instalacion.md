---
title: "Instalación"
output:
  html_document:
    highlight: tango
---

##R
R es un lenguaje utilizado principalmente para realizar análisis estadísticos, gráficos y para programación. Puede ser copiado y distribuido de forma gratuita y permite que los usuarios estudien como funciona, lo modifiquen de acuerdo a sus necesidades y publiquen sus mejoras y extensiones para ser compartidas con otros usuarios. Presenta además numerosas ventajas para su uso en la investigación científica.   

1. Es actualmente el software que ofrece mayor número de funciones estadísticas y aplicaciones para la creación de gráficos. Colaboradores de todo el mundo producen paquetes destinados a resolver problemas particulares o desarrollan determinadas técnicas estadísticas. Cada usuario puede, además, crear sus propias funciones y rutinas combinando las funciones existentes.   
2. Existe una amplia comunidad de usuarios y por lo tanto, una gran oferta de ayudas para aquellos que se inician en el uso del programa, incluyendo foros de discusión en internet y una creciente bibliografía.   
3. R es una *lingua franca* para el intercambio de ideas en el ámbito científico. De la misma manera en que se escribe un artículo para comunicar ideas, pueden escribirse rutinas para comunicar un análisis. Un número creciente de trabajos citan este software o presentan rutinas escritas en este lenguaje publicadas.   

##¿Qué es una rutina, qué es un código?   
Las rutinas utilizadas por los científicos pueden ser separadas en dos grandes categorías ([Mislan *et al.* 2016](https://www.sciencedirect.com/science/article/pii/S0169534715002906)).   

1. Las rutinas de análisis (*analysis code*) se utilizan para ensamblar bases de datos y corregir errores, simular el resultado de modelos, realizar análisis estadísticos y crear figuras. Poner a disposición de otros investigadores las rutinas de análisis es necesario para que los resultados de un estudio puedan ser reproducidos.   
2. El software científico (como puede ser un paquete de R o Python) es diseñado para poder ser utilizado en muchos proyectos diferentes. Puede ser  el producto mismo de una investigación.   

##Sobre las rutinas de este manual.
En este manual las rutinas aparecen incrustadas en el pdf o en la página web y pueden reconocerse por su distinta tipografía y por estar en recuadros. Sin embargo, por comodidad es recomendable utilizar block de notas, gedit, R scripts, RStudio o TinnR para realizarlas (evitar los procesadores de texto como Word o Writer). A partir de 2015 todo el material de este curso también está disponibles como documentos de R Markdown en el repo https://github.com/santiagombv/cursoR y en la página http://santiagombv.github.io/CursoR/   
El objetivo de este curso es capacitar al alumno para realizar una rutina de análisis por sí mismo, aplicando conocimientos de modelos lineales, manejos de gráficos y programación. 

\pagebreak

##Instalación de R y de sus paquetes en Windows.
###Instalación de R.
Entrar a http://cran.r-project.org/   
En download R for Windows - base, seleccionar la última versión disponible de R (3.4.4 "Someone to lean on" actualmente). Guardar el archivo R-3.4.4-win.exe en cualquier parte de la computadora, ejecutarlo y seguir las instrucciones de instalación. 

###Instalación de paquetes.
Seleccionar Paquetes - Instalar Paquetes (la computadora debe estar conectada a internet).  
Seleccionar el espejo CRAN desde donde se bajará el paquete (utilizar uno cercano).  
Seleccionar un paquete de la lista despegable. Opciones similares pueden encontrarse en RStudio. 
Alternativamente utilizar:  
```{r, eval=FALSE}
install.packages("nombre_del_paquete")
```

###Carga de paquetes.
Para que un paquete esté disponible en una sesión de trabajo seleccionar Paquetes – Cargar paquetes y elegirlo en la lista desplegable que se abre. Alternativamente puede cargarse escribiendo en la consola principal de R.
```{r, eval=FALSE}
library(nombre_del_paquete)
```

###Actualización de R. 
En Windows se puede desinstalar y volver a instalar el programa en cada actualización. Alternativamente utilizar el paquete *installr*
```{r, eval=FALSE}
install.packages("installr")
library(installr)
updateR() #seguir las instrucciones que aparezcan
```


##Instalación de R y de sus paquetes en Ubuntu.
###Instalación de R.
Puede instalarse R desde el centro de software de Ubuntu, pero habitualmente es una versión antigua, ya que tarda en actualizarse. Para evitar esto usted debe realizar 3 pasos.  

1. Añadir una fuentes de software desde "Configuración del Sistema" - "Software y actualizaciones" - "Otro Software". Seleccionar "Añadir". En la ventana que solicita la línea de APT completa del repositorio colocar:   
deb http://cran.rstudio.com/bin/linux/ubuntu/ xenial/   
En esta línea puede cambiar el espejo CRAN elegido (http://...), por alguno de los listados en  http://cran.r-project.org/ Por ejemplo http://mirror.fcaglp.unlp.edu.ar/CRAN/bin/linux/ubuntu corresponde a la Universidad Nacional de la Plata. Puede cambiar también la distribución de Ubuntu empleada (xenial, trusty, saucy, quantal, etc. indicada en la última palabra). 

2. Agregar una clave de seguridad al sistema (en caso de que esta opción "no funcione", ver otras posibilidades en http://cran.r-project.org/). Tipear en la terminal:     

```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E084DAB9
```

3. Instalar R desde terminal.     

```
sudo apt-get update
sudo apt-get install r-base
```

###Instalación de paquetes.
La manera más práctica de hacerlo es instalar RStudio y utilizar su gestor de paquetes.  Alternativamente se puede hacer desde terminal.

```
sudo R
install.packages("nombre_del_paquete")
```

###Carga de paquetes.
Debe cargarse desde la consola principal de R o Rstudio.
```{r, eval=FALSE}
library(nombre_del_paquete)
```

###Actualización de R. 
Al agregar una fuente de software a la computadora, R se actualizará junto a todos los otros programas, automáticamente. Para mantener al día la última versión de R basta con colocar en la terminal.   
```
sudo apt-get update
sudo apt-get upgrade
```

-----------------------------------------------------------------------------------------------------------------

> **Nota 1**: en algunas versiones de Windows, el usuario no puede modificar las carpetas contenidas en "Archivos de programas". En ese caso, los paquetes no se guardarán correctamente y se borrarán cada vez que reinicie la computadora. Para evitarlo, localice la carpeta "R" dentro de los Archivos de programa, selecciónela con el botón derecho del mouse, abra la opción Propiedades - Seguridad, y asegúrese de permitir "control total" a todos los usuarios.  
> **Nota 2**: para este curso, se requieren los siguientes paquetes extra: *glmnet*, *ggplot2*, *lattice*, *rgl*, *sciplot*, *car*, *arm*, *MuMIn*, *devtools*, *leaps* y todas las dependencias necesarias.  
> **Nota 3**. Se recomienda además utilizar la última versión de R por compatibilidad con algunas rutinas.  
> **Nota 4**: Para este curso utilizaremos RStudio (descargar desde http://www.rstudio.com/).   

-----------------------------------------------------------------------------------------------------------------
