---
title: "Práctico 7. Construyendo Rutinas."
output: 
  html_document:
    highlight: tango
---

El objetivo de este trabajo es practicar la escritura de una rutina, la cual debe incluir el ingreso de datos, la carga de paquetes (en caso de ser necesario), la modificación de variables (en caso de ser necesario), exploración gráfica de los datos, planteo de diferentes modelos, comprobación de supuestos y el modelo final elegido. Utilizaremos R Studio para construir la rutina, la cual guardaremos con la extensión .R.     
Luego utilizaremos el mismo programa para construir un documento R Markdown, el cual combina la naturaleza de una rutina de R con la habilidad de generar un documento que presente los resultados del análisis (el manual de este curso está construido de esta forma). Para esto, en R Studio seleccione New file - R Markdown. En output format, elija HTML (para pdf es necesario instalar algunos programas extra en la computadora). Se abrirá un documento auto-explicado, solo es necesario reemplazar su contenido por el contenido deseado. Al finalizar, haga click en el ícono de "Knit HTML". La ayuda general de R Markdown puede encontrarse en http://rmarkdown.rstudio.com/.         


###Ejercicio 1. Nereis.txt
Identificar el efecto de la densidad de especies y del tipo de nutriente que estas reciben sobre la concentración de sólidos solubles en el bentos marino. Las variables incluidas son:
concentration:  concentración de sólidos solubles.
biomass: densidad del poliqueto Nereis diversicolor.
nutrient: tipo de nutriente (1= NH4-N, 2=PO3-P, 3= NO3-N).

###Ejercicio 2. Sparrows2.txt
Modelar el número de gorriones de las marismas en función de varias variables que describen la cobertura de distintas especies de plantas. La variable respuesta es banded (número de gorriones), las variables de cobertura son Avgmaxht, Avgdens, ht.thatch, S.patens, Distichlis, S.alternifloraShort, S.alternifloraTall, Juncus, Bare, Other, Phragmites, Shrub, Tallsedge, Water.

###Ejercicio 3. Sparrows3.txt 
Probar si la relación entre la longitud del ala y el peso en ejemplares de gorrión varía de acuerdo al mes de muestreo y al sexo. 
Las variables de importancia son:
wingcrd: longitud del ala
wt: peso
Month: mes
sex: sexo 