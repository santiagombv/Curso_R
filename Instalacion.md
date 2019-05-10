# Instalación

## R
R es un lenguaje utilizado principalmente para realizar análisis estadísticos, gráficos y para programación. Puede ser copiado y distribuido de forma gratuita y permite que los usuarios estudien como funciona, lo modifiquen de acuerdo a sus necesidades y publiquen sus mejoras y extensiones para ser compartidas con otros usuarios. Presenta además numerosas ventajas para su uso en la investigación científica.   

1. Es actualmente el software que ofrece mayor número de funciones estadísticas y aplicaciones para la creación de gráficos. Colaboradores de todo el mundo producen paquetes destinados a resolver problemas particulares o desarrollan determinadas técnicas estadísticas. Cada usuario puede, además, crear sus propias funciones y rutinas combinando las funciones existentes.   
2. Existe una amplia comunidad de usuarios y por lo tanto, una gran oferta de ayudas para aquellos que se inician en el uso del programa, incluyendo foros de discusión en internet y una creciente bibliografía.   
3. R es una *lingua franca* para el intercambio de ideas en el ámbito científico. De la misma manera en que se escribe un artículo para comunicar ideas, pueden escribirse rutinas para comunicar un análisis. Un número creciente de trabajos citan este software o presentan rutinas escritas en este lenguaje publicadas.   

## ¿Qué es una rutina, qué es un código?

Las rutinas utilizadas por los científicos pueden ser separadas en dos grandes categorías ([Mislan *et al.* 2016](https://www.sciencedirect.com/science/article/pii/S0169534715002906)).   

1. Las rutinas de análisis (*analysis code*) se utilizan para ensamblar bases de datos y corregir errores, simular el resultado de modelos, realizar análisis estadísticos y crear figuras. Poner a disposición de otros investigadores las rutinas de análisis es necesario para que los resultados de un estudio puedan ser reproducidos.   
2. El software científico (como puede ser un paquete de R o Python) es diseñado para poder ser utilizado en muchos proyectos diferentes. Puede ser  el producto mismo de una investigación.   

## Sobre las rutinas de este manual

En este manual las rutinas aparecen incrustadas en la página web y pueden reconocerse por su distinta tipografía y por estar en recuadros. Sin embargo, por comodidad es recomendable utilizar block de notas, gedit, R scripts, RStudio o TinnR para realizarlas (evitar los procesadores de texto como Word o Writer). A partir de 2015 todo el material de este curso está disponibles en forma abierta en los repos https://github.com/santiagombv/cursoR (rutinas y archivos) y en la página http://santiagombv.github.io/Curso_R/ (teóricos y prácticos). Desde 2015 también utilizaremos de forma preferente RStudio (https://www.rstudio.com/) como interfaz gráfica de usuario (GUI).
El objetivo de este curso es capacitar al alumno para realizar una rutina de análisis por sí mismo, aplicando conocimientos de modelos lineales, manejos de gráficos y programación. 

## Paquetes
Los paquetes de R son colecciones de funciones para un fin determinado, asociadas a su ayuda y, ocasionalmente, bases de datos y demostraciones de uso (*vignettes*). Para mayo de 2019, se encontraban disponibles 14191 paquetes.

> Para este curso, se requieren los siguientes paquetes que no son descargados de forma automática con la instalación de R, por lo que deberá instalarlos manualmente: *glmnet*, *ggplot2*, *lattice*, *rgl*, *sciplot*, *car*, *arm*, *MuMIn*, *devtools*, *leaps* y todas las dependencias necesarias.  

## Instalación de R y de sus paquetes en Windows

### Instalación de R

Entrar a http://cran.r-project.org/   
En download R for Windows - base, seleccionar la última versión disponible de R (3.6.0 "Planting of a Tree" actualmente). Guardar el archivo R-3.6.0-win.exe en cualquier parte de la computadora, ejecutarlo y seguir las instrucciones de instalación. 

### Instalación de paquetes.

Seleccionar Paquetes - Instalar Paquetes (la computadora debe estar conectada a internet). En R Studio seleccionar install en la pestaña packages.  
Seleccionar el espejo CRAN desde donde se bajará el paquete.  
Seleccionar un paquete de la lista despegable. 

Alternativamente utilizar:  

```R
install.packages("nombre_del_paquete")
```

### Carga de paquetes

Para que un paquete esté disponible en una sesión de trabajo seleccionar Paquetes – Cargar paquetes y elegirlo en la lista desplegable que se abre. Alternativamente puede cargarse escribiendo en la consola principal de R.

```R
library(nombre_del_paquete)
```

### Actualización de R. 
En Windows se puede desinstalar y volver a instalar el programa en cada actualización. Alternativamente utilizar el paquete *installr*

```R
install.packages("installr")
library(installr)
updateR() #seguir las instrucciones que aparezcan
```

## Instalación de R y de sus paquetes en Ubuntu

### Instalación de R

Añadir una fuentes de software desde "Configuración del Sistema" - "Software y actualizaciones" - "Otro Software". Seleccionar "Añadir". En la ventana que solicita la línea de APT completa del repositorio colocar:   
```
deb https://cloud.r-project.org/bin/linux/ubuntu bionic-cran35/
```
En esta línea puede cambiar el espejo CRAN elegido por alguno de los listados en  http://cran.r-project.org/ Por ejemplo http://cran.rstudio.com/bin/linux/ubuntu/  corresponde al espejo propio de RStudio. Puede cambiar también la distribución de Ubuntu empleada (disco, cosmic, bionic, xenial, trusty, etc. indicada antes del -cran35). 

Agregar una clave de seguridad al sistema (en caso de que esta opción "no funcione", ver otras posibilidades en http://cran.r-project.org/). En la terminal ingresar:     
```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
```

Instalar R desde terminal.     

```
sudo apt-get update
sudo apt-get install r-base
```

### Instalación de paquetes

La manera más práctica en Ubuntu es desde terminal.
```
sudo R
install.packages("nombre_del_paquete")
```

### Carga de paquetes

Debe cargarse desde la consola principal de R o Rstudio

```R
library(nombre_del_paquete)
```

### Actualización de R

Al agregar una fuente de software a la computadora, R se actualizará junto a todos los otros programas, automáticamente. Para mantener al día la última versión de R basta con colocar en la terminal. 

```
sudo apt-get update
sudo apt-get upgrade
```




