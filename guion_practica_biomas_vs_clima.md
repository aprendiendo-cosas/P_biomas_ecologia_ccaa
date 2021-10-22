# Instrucciones para realizar la práctica denominada "relación entre los biomas y el clima"


> + **_Versión_**: 2020-2021
> + **_Asignatura (grado)_**: Ecología (Ciencias ambientales). Curso 2020-2021
> + **_Autor_**: Curro Bonet-García (fjbonet@uco.es)
> + **_Duración_**: 3 horas.



## Objetivos

Esta práctica tiene los siguientes objetivos operacionales y competenciales :

 + Disciplinares: Tienen que ver con la ecología.
    +    Analizar la relación entre la distribución de los biomas en la Tierra y el clima.
    +    Discutir sobre las posibles excepciones a esta relación general. 
    +    Construir una gráfica que constate la relación anterior.
 + Competenciales: Se refiere al conjunto de habilidades o conocimientos que se espera que adquieran los estudiantes durante el desarollo de esta actividad.
   +    Practicar SIG. Concretamente en relación a las funciones de extracción de información de capas raster a puntos.
   +    Introducción a la programación con R. Se construirá una gráfica sencilla con este lenguaje de programación.
   + Remarcar la importancia del concepto de flujo de trabajo como herramienta básica para planificar experimentos y procesos de análisis de datos ecológicos. 
   
     

Este documento contiene la misma información que [esta](https://github.com/aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/presentaciones/protocolo_biomas_vs_clima_v2020-2021.pptx) presentación, que se usará en clase como guía para la práctica. 


<div style="page-break-after: always; break-after: page;"></div>



## Contextualización ecológica

 ([Whittaker, R.H. 1962](https://link.springer.com/article/10.1007%2FBF02860872)) clasificó los distintos tipos de biomas existentes en la Tierra en función de la precipitación y temperatura. Su trabajo constató la relación existente entre la distribución de estos dos factores climáticos y los biomas a escala global. 

El mapa que ves a continuación muestra la distribución espacial de los biomas. La gráfica inferior representa  de manera muy intuitiva esta relación. Se puede ver claramente cómo los bosques tropicales, por ejemplo, viven en lugares donde tanto la temperatura como la precipitación son altos. 



<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/mapa_biomas.png" alt="biomes" style="zoom:75%;" />

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/68/Climate_influence_on_terrestrial_biome.svg/1920px-Climate_influence_on_terrestrial_biome.svg.png" alt="biomes" style="zoom:25%;" />



En esta práctica aprenderemos a construir una gráfica como la de arriba. Esto nos permitirá aprender algo de SIG, de programación y también nos ayudará a evocar conocimiento ya adquirido en las sesiones teóricas de ecología I. 

En definitiva, haremos algo parecido a lo que hace [esta](https://media.hhmi.org/biointeractive/biomeviewer_web/index.html?_ga=2.261737284.818222928.1535646280-907606146.1534976570) web, pero de forma algo más "casera" (y también útil para tu aprendizaje). Cuando entres en la aplicación haz doble clik en cualquier punto del mundo. Se mostrará una ficha con los datos climáticos básicos y con algunos datos ecológicos del bioma en cuestión. En la parte inferior de dicha ficha verás un botón que indica "*compare*". Esto te permitirá comparar dos puntos diferentes desde el punto de vista que nos ocupa. 

<div style="page-break-after: always; break-after: page;"></div>

## Material 
Para lograr el objetivo que nos hemos planteado y construir la gráfica de Whittaker, usaremos los siguientes conjuntos de datos:
+ Mapa de distribución de los principales biomas del mundo. Hay multitud de iniciativas que han cartografiado la distribución de los biomas a escala global. En este caso usaremos un mapa generado por una ONG llamada [RESOLVE](https://www.resolve.ngo/). [Aquí](https://ecoregions2017.appspot.com/) puedes ver esta información en un visor web. Y en [este](https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/geoinfo/biomas.zip) enlace puedes acceder a un archivo zip (**biomas.zip**) que contiene el mapa visible por un SIG. Tendrás que descargar el zip anterior para hacer la práctica. Este mapa está en formato *shapefile* ("fichero de formas"). Este tipo de ficheros son comúnmente utilizados en los SIG para representar información vectorial. [Aquí](https://en.wikipedia.org/wiki/Shapefile) puedes leer algo más sobre esto. Cada fichero de formas tiene al menos cuatro archivos con extensiones diversas (*.shx, .dbf, .shp, .sbn*)
+ Mapa de distribución de la temperatura media anual a escala global (**temp.tif**). Este mapa se ha descargado de la web del proyecto [WORDCLIM](https://worldclim.org/data/index.html), cuyo objetivo es generar mapas de variables climáticas de todo el planeta. Para ello interpolan datos climáticos de miles de estaciones meteorológicas distribuidas por todo el planeta. El mapa con la temperatura media anual se puede descargar en [este](https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/geoinfo/temp.tif) enlace. Tendrás que descargarlo para hacer la práctica. Este mapa está en formato *.tif*. Se trata de un formato raster en el que la información se almacena mediante una malla de píxeles regular. Cada píxel tiene información sobre la temperatura media anual. Esta temperatura se expresa en décimas de grado Celsius.
+ Mapa de precipitación total anual (**rain.tif**). Tiene el mismo origen que el anterior y se puede descargar en [este](https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/geoinfo/rain.tif) enlace. También es un archivo raster.

Las fuentes de datos descritas hasta aquí se denominan con el término genérico de "capa" (*layer* en inglés). Son homólogas al concepto de mapa en papel al que estamos acostumbrados. Cada capa se almacena en uno o varios archivos digitales. Una capa no es más que una representación simplificada de la realidad. Hay dos formas básicas de representar esa realidad: capas raster y vectoriales. El siguiente esquema muestra las diferencias entre ambas concepciones.





<img src="http://www.newdesignfile.com/postpic/2012/05/vector-and-raster-data-model_131901.jpg" alt="biomes" style="zoom:100%;" />



+ Proyecto de QGIS (**clima_vs_biomas.qgs**). [QGIS](https://qgis.org/es/site) es un sistema de información geográfica de [código abierto](https://en.wikipedia.org/wiki/Open_source). En QGIS la información geográfica se muestra creando un "proyecto". Se trata de un conjunto de visualizaciones a capas de información geográfica. Dentro de un proyecto se pueden mostrar multitud de capas raster y vectoriales. Estos proyectos se almacenan en un archivo con extensión *.qgs*. En realidad no contienen la información geográfica, sino únicamente referencias a ésta. Los proyectos solo contienen instrucciones que indican dónde están las capas que han de ser representadas en el SIG. Por ejemplo: "ve a la ruta *d:/misdocs/archivos/rain.tif*" y representa esa capa en la pantalla con una paleta de colores del azul al rojo". Es decir, si envías a alguien por correo electrónico el proyecto de QGIS no le estarás enviando las capas, sino únicamente referencias a las mismas. Para facilitarte el manejo de la información geográfica, puedes descargar [este](https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/geoinfo/clima_vs_biomas.zip) proyecto de QGIS, que ya contiene una vista de las capas descritas anteriormente. Cuando lo descargues, deberás descomprimirlo en tu carpeta de trabajo.
+ Tabla con la lista de biomas (**lista_biomas.txt**). Es una simple tabla con el código de cada bioma, su nombre y un color (expresado con una palabra). Este color es el que se usará para mostrar cada punto de la gráfica. De esta manera, todos los puntos que estén en el mismo bioma tendrán el mismo color. [Aquí](https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/geoinfo/lista_biomas.zip) tienes el enlace para descargar esta información. Encontrarás el archivo de texto cuando descomprimas el archivo *.zip.*



En definitiva, aquí tienes enlaces a todo el material que has de bajar:

+ [Mapa de biomas.](https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/geoinfo/biomas.zip)
+ [Mapa de precipitación.](https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/geoinfo/rain.tif)
+ [Mapa de temperaturas.](https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/geoinfo/temp.tif)
+ [Proyecto de QGIS.](https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/geoinfo/clima_vs_biomas.zip)
+ [Lista de biomas.](https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/geoinfo/lista_biomas.zip)



<div style="page-break-after: always; break-after: page;"></div>

## Metodología y flujo de trabajo

Para construir la gráfica deseada, seleccionaremos una serie de puntos en todo el planeta sabiendo qué bioma se desarrolla en ellos. A continuación asignaremos a esos puntos su temperatura media y su precipitación anual. De esta manera tendremos, para cada punto, los dos valores que se necesitan para construir la gráfica.  Los primeros pasos de esta sencilla metodología se darán usando QGIS. Para construir la gráfica usaremos un lenguaje de programación llamado [R](https://en.wikipedia.org/wiki/R_(programming_language)). 

La secuencia de pasos descrita brevemente en el párrafo anterior es lo que se denomina "[flujo de trabajo](https://en.wikipedia.org/wiki/Workflow)" (*workflow* en inglés). Se trata de una secuencia ordenada de acciones o pasos que se llevan a cabo de una manera sistemática para conseguir un objetivo concreto. El concepto de flujo de trabajo es muy habitual en ciencias como la ecología, en la que es necesario analizar multitud de datos para satisfacer un objetivo determinado. Es muy útil describir el flujo de trabajo, bien en modo texto (párrafo inicial de esta sección), o bien en modo gráfico, como puedes ver a continuación:

<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/workflow_bioma_vs_clima.png" alt="biomes" style="zoom:55%;" />

Los números morados que aparecen en la imagen superior son los distintos pasos del flujo de trabajo que aplicaremos. Los rombos se corresponden con procesos y análisis que aplicaremos sobre los datos (representados por rectángulos o por trapecios). Todos los procedimientos que están sobre un fondo azul, se realizarán usando QGIS. Los que están sobre un fondo naranja se harán usando R. En la siguiente sección describiremos con detalle cada uno de los pasos de este flujo de trabajo.

<div style="page-break-after: always; break-after: page;"></div>

## Paso a paso ##

### 0) Primer vistazo a la información suministrada ###

En primer lugar veremos con el explorador de Windows la información que necesitamos para la práctica. Debes de descargar y descomprimir todos los archivos descritos anteriormente. Ponlos todos en la misma carpeta de tu ordenador (importante: no los pongas en el escritorio, sino en tu carpeta de documentos). Una vez hecho esto, deberás ver algo como esto:



<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/archivos.png" alt="biomes" style="zoom:55%;" />

Veamos a qué corresponde cada archivo:

+ Todos los archivos cuyo nombre empieza por "biomas", son parte de una capa vectorial (de tipo *shapefile*). Cada uno de ellos tiene información importante. Los dos más importantes son los que se describen en la siguiente imagen. El archivo con extensión *.shp* contiene la geometría, es decir, los polígonos que representan la distribución espacial de los biomas. El archivo con extensión *.dfb* contiene la información alfanumérica asociada a cada polígono anterior. Es decir, los atributos temáticos de cada polígono. En este caso es el nombre del bioma al que pertenece cada polígono, o su superficie.

<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/descripcion_biomas.png" alt="biomes" style="zoom:55%;" />

+ Los archivos con extensión *.tif* se corresponden con las capas climáticas que usaremos en la práctica: precipitación y temperatura. Se trata de capas raster en las que cada píxel tiene información sobre la variable representada. Ver siguiente figura.



<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/descripcion_capas_clima.png" alt="biomes" style="zoom:55%;" />

+ El archivo con extensión *.qgs* es un proyecto de QGIS del que hablaremos en el siguiente punto.



### 1) Primeros pasos con QGIS ###

+ Trabajaremos con la versión 2.16 de QGIS. Búscalo en tu ordenador y ábrelo. 
+ Luego dale al botón *abrir* y navega hasta la carpeta en la que hayas guardado la información descrita anteriormente. Selecciona el archivo llamado *clima_vs_biomas.qgs*. Pulsa aceptar. Verás algo parecido a lo siguiente:



<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/qgis.png" alt="biomes" style="zoom:55%;" />



Has abierto un proyecto en el que hay referencias a las capas que necesitaremos para la práctica. Si borramos las capas descritas anteriormente, el proyecto no las mostrará, ya que no será capaz de encontrarlas.

+ Practica un poco con las herramientas de zoom y activar/desactivar capas.

<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/zoom.png" alt="biomes" style="zoom:55%;" />

+ Ahora observaremos con un poco más de detalle el mapa de los biomas de la Tierra. Sigue las instrucciones que aparecen en las siguientes imágenes.

<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/qgis_biomas_1.png" alt="biomes" style="zoom:55%;" />





<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/qgis_biomas_2.png" alt="biomes" style="zoom:55%;" />





<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/qgis_biomas_3.png" alt="biomes" style="zoom:55%;" />





<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/qgis_biomas_4.png" alt="biomes" style="zoom:55%;" />



La siguiente tabla muestra la lista de biomas existentes en el mapa que estamos utilizando.

| Código del bioma | Nombre del bioma                               |
| ---------------- | ---------------------------------------------- |
| 1                | Tropical & Subtropical moist broadleaf forests |
| 2                | Tropical & Subropical dry broadleaf forests    |
| 3                | Tropical & Subropical coniferous forests       |
|4 | Temperate broadleaf & mixed forests |
|5 | Temperate confier forests|
|6 | Boreal forests. Taiga|
|7 | Tropical & subropical grasslands, savannas & shrublands|
|8 | Temperate grasslands, savannas & shrublands|
|9 | Flooded grasslands & savannas|
|10 | Montane grasslands & Shrublands|
|11 | Tundra |
|12  | Mediterranean forests, Woodlands & Scrub|
|13 | Deserts & Xeric shrublands |
|14 | Mangroves |


### 2) Crear capa de puntos ###
Para construir la gráfica que relaciona los biomas con el clima, necesitamos seleccionar una serie de puntos. Estos puntos deben de estar "etiquetados" con el código del bioma en el que se encuentran. Con QGIS crearemos una capa de puntos (vectorial). Cada bioma tendrá 5 puntos que deberán de estar distribuidos de manera homogénea por toda el área de distribución del bioma. Por ejemplo, si estás dibujando puntos en el bioma *Mediterranean forests...*, deberás de dibujar puntos tanto en la cuenca Mediterránea como en California o Sudáfrica. 

En QGIS las capas de puntos se crean usando formato **shapefile** (el mismo formato que tiene el mapa de biomas). Un shapefile puede contener, puntos, líneas o polígonos. En nuestro caso crearemos un shapefile de puntos (el de biomas tiene polígonos). Primero lo crearemos vacío, lo guardaremos en el disco duro y luego iremos añadiendo los puntos. Sigue las instrucciones que se muestran a continuación:

+ Ve al menú *Capa -> Crear capa -> Nueva capa de archivo shape*

+ En la ventana emergente completa los siguientes parámetros:

  + Nombre y ubicación del archivo. Pincha en los tres puntos que hay a la derecha y navega hacia la carpeta donde quieras guardar el archivo (que debe de ser la misma en la que tienes las demás capas). Se abrirá la ventana de navegación de Windows. Una vez en la carpeta elegida, teclea: "puntos" (sin comillas...). Dale a guardar.

  + Volveremos a la ventana anterior. Donde pone "*tipo de geometría*", has de asegurarte de seleccionar "punto". Y en el sistema de referencia debemos poner "*EPSG:3857*".

  + En ella vamos a crear un nuevo campo dentro de la capa. Recuerda que cada capa tiene asociada una tabla de atributos que permite asignar características a cada uno de los elementos geométricos. Ejemplo: los nombres de los ríos en una capa de ríos. En la ventana buscamos la opción de crear "nuevo campo". Allí ponemos el nombre del nuevo campo: "*bioma_id*". Indicamos que será un campo de tipo "*Número entero*". Luego pulsamos en la opción de "*añadir a la lista de campos*".

  + Una vez hecho lo anterior, pulsamos aceptar. QGIS habrá creado un shapefile vacío llamado *puntos*. Si observamos este fichero de formas en el explorador de Windows, veremos que contiene varios archivos con distintas extensiones.

  + Al aceptar, la nueva capa se carga automáticamente en la vista de QGIS, como puedes ver en la siguiente figura.
  
    

<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/puntos_1.png" alt="biomes" style="zoom:55%;" />



+ Edición de la capa de puntos. Para añadir puntos a nuestra capa vacía, hemos de iniciar una sesión de edición. Sigue las instrucciones mostradas en la imagen inferior.



<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/activar_edicion.png" alt="biomes" style="zoom:55%;" />

+ Ya podemos añadir puntos. Empieza haciendo zoom en la zona que quieras y sigue las instrucciones de la imagen. 

  

<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/dibujar_puntos.png" alt="biomes" style="zoom:55%;" />

+ Una vez que hayas dibujado todos los puntos, puedes parar la sesión de edición, tal y como se indica en la siguiente figura:



<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/parar_edicion.png" alt="biomes" style="zoom:55%;" />



### 3) Asignar valores de clima a cada punto ###

Este paso es fundamental porque nos permitirá tener, para cada punto, su valor de la precipitación y temperatura media. Es lo que necesitamos para construir la gráfica que es objeto de esta práctica. Desde el punto de vista de los SIG, este proceso consiste en "extraer" información de una o varias capas raster en aquellos píxeles donde están los puntos que hemos creado anteriormente. Para hacer esto en QGIS seguiremos los siguientes pasos:

+ En primer lugar, es necesario instalar un *plugin* en QGIS. Se trata de complementos o bloques de código que aportan nuevas funcionalidades al programa. Para hacer esto hemos de navegar hasta el menú *complementos -> Administrar e instalar complementos*. Esto abrirá un menú emergente en el que tecleamos "point sampling tool" (sin comillas). Lee la descripción y entenderás mejor el proceso que vamos a realizar. En la misma ventana selecciona la opción de instalar complemento. 
+ Al instalar el *plugin* aparece un nuevo botón para ejecutar la función deseada. En la siguiente figura puedes ver cómo operar con este *plugin*. La idea es inicar que la capa de puntos que hemos creado es la que contiene los puntos de muestreo (a los que asignaremos los valores de los raster). También hemos de seleccionar los campos que se añadan a la tabla de atributos de la capa de puntos (*temp* y *rain*). Es necesario seleccionar las capas rasters de la que se extraerán los datos. Hacemos click sobre los dos pulsando el botón de mayúsculas. Por último, es necesario indicar la ubicación de la tabla obtenida. Se creará una nueva capa de puntos que denominaremos "*puntos_clima*".

<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/spatial_join.png" alt="biomes" style="zoom:55%;" />



+ La capa obtenida se carga automáticamente en QGIS. Desde un punto de vista de la geometría es igual que la que creamos en el punto **2**. Es decir, contiene los mismos puntos. Pero su tabla de atributos ahora tiene dos campos más. Uno de ellos muestra la temperatura media del punto (en décimas de grado). Y el otro muestra la precipitación total anual de dicho punto. Sigue las instrucciones de la imagen para acceder a la tabla de atributos.

<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/puntos_clima.png" alt="biomes" style="zoom:55%;" />



### 4) Unir un campo con el color que tendrá cada bioma

Queremos que cada punto de la gráfica tenga el color del bioma en el que se encuentra. Para hacer esto, necesitamos añadir un campo llamado "*color*"a la tabla de atributos de la capa que hemos creado. Esto se hace mediante una sencilla operación llamada "unión entre tablas a través de un campo común". Haz lo que se indica a continuación:

+ Primero añadimos a QGIS la tabla "*lista_biomas.txt*". En ella se asigna un color a cada bioma. Las siguientes imágenes muestran cómo se hace esto en QGIS.

<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/lista_biomas_1.png" alt="biomes" style="zoom:55%;" />

<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/lista_biomas_2.png" alt="biomes" style="zoom:55%;" />



+ Una vez incluída la tabla en el proyecto de QGIS, procedemos a realizar la unión entre la capa "*puntos_clima*" y la tabla recién agregada. Hacemos doble click sobre la capa "*puntos_clima*. Se abrirá la ventana de propiedades.

<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/join_1.png" alt="biomes" style="zoom:55%;" />



<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/join_2.png" alt="biomes" style="zoom:55%;" />



+ Comprobamos que se ha hecho bien la unión abriendo la tabla de atributos de la capa "*puntos_clima*". Debe de verse un campo nuevo llamado "*color*".



### 5) Exportar la tabla de atributos de la nueva capa ###

Como dijimos al principio, construiremos la gráfica usando R. Sabemos que también se puede hacer con Excel y que seguramente será más fácil hacerlo así. Pero se trata de que aprendáis nuevas herramientas. R es un lenguaje de programación muy potente que te resultará muy útil hagas lo que hagas profesionalmente. 

Para que R pueda "ver" los datos, hemos de exportarlo a un archivo de texto. Exportaremos la tabla de atributos de la nueva capa de puntos descrita anteriormente. Para ello, sigue las instrucciones de la siguiente imagen.

<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/exportar_tabla.png" alt="biomes" style="zoom:55%;" />



Es importante que respetes los nombres de los archivos que hay en el guión. De lo contrario podrías tener problemas al crear la gráfica en R. Si quieres, puedes abrir el archivo de texto creado. Para ello usa Notepad++. Deberás ver algo parecido a esto:



<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/tabla_csv.png" alt="biomes" style="zoom:55%;" />



### 6) Generar la gráfica con R

Como hemos comentado anteriormente, generaremos la gráfica en R. Se trata de una gráfica de dispersión en la que en el eje X pondremos la temperatura y en el Y la precipitación. 

Antes de construir la gráfica, veamos un poco qué es R y por qué lo usamos aquí. R es un lenguaje de programación muy utilizado en ciencia. Tiene herramientas de análisis y visualización de datos muy potente. Además, es software libre, por lo que es posible crear programas, extensiones y otros elementos con diversas funcionalidades. [Aquí](https://www.youtube.com/watch?v=9kYUGMg_14s) tienes un video que muestra de manera resumida en qué consiste este lenguaje de programación. 

Es posible usar R en un interfaz de comandos. Es decir, usando la consola. Pero normalmente se usan entornos de desarrollo que facilitan algunas tareas. En nuestro caso usaremos Rstudio. Esta herramienta facilita algunas tareas de programación y visualización de resultados. Rstudio es como una especie "máscara" que se sitúa sobre R. Es decir, cuando interactuamos con Rstudio, éste lo hace con R y le transmite nuestras instrucciones. La siguiente imagen muestra la estructura de Rstudio.



<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/rstudio.png" alt="biomes" style="zoom:55%;" />



Dado que R es un lenguaje de programación, interactuamos con él mediante líneas de código. En estas líneas vamos dándole al programa instrucciones para que manipule los datos y opere con ellos. En nuestro caso, queremos que R haga lo siguiente:*

+ Importa la tabla llamada "*puntos_clima.csv"* que hemos creado al final de la sección anterior.
+ Genera una gráfica de dispersión a partir de  "*puntos_clima.csv*"" en la que el campo "*temp*"" esté en el eje X y el campo "*rain*" esté en el eje Y. Además, pon una etiqueta en cada punto que contenga los valores del campo "*bioma_id*". Por último, colorea cada punto en función de los valores de colores existentes en el campo "*color*".

Para hacer que R entienda lo que se muestra arriba, debemos usar un lenguaje inteligible para él. Lo anterior, traducido a R, se escribe así:

<div style="page-break-after: always; break-after: page;"></div>

```R
#DEFINIMOS EL DIRECTORIO DE TRABAJO
dir_trabajo<-'//cifs/bv2bogaf/Documents/biomas_vs_clima'

#ESTABLECEMOS EL DIRECTORIO DE TRABAJO
setwd(dir_trabajo)
getwd()

# IMPORTAMOS LA TABLA DE DATOS
puntos_clima_color<-read.table("puntos_clima_color.csv",header=T, sep=',')


# CONSTRUIMOS LA GRÁFICA DE DISPERSIÓN
plot(puntos_clima_color$temp, puntos_clima_color$rain,
     col=puntos_clima_color$color, 2020-2021 = "Temperatura vs Precipitación",
     xlab="Temperatura media anual", ylab="Precipitación anual", pch=19,
     text(puntos_clima_color$temp, puntos_clima_color$rain,
          labels=puntos_clima_color$bioma_id, cex = 0.7, pos=3))

```

Veamos paso a paso y con imágenes qué quiere decir este código.

+ Establecer el directorio de trabajo en R. Este primer paso consiste en indicarle al software dónde están los datos con los que trabajará.

<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/dir_trabajo_1.png" alt="biomes" style="zoom:55%;" />



<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/dir_trabajo_2.png" alt="biomes" style="zoom:55%;" />



+ Importar la tabla "*puntos_clima.csv*" en R. Para ello crearemos un objeto de R llamado "*puntos_clima*"que contendrá los registros de la tabla creada anteriomente en QGIS.

  

<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/importa_csv.png" alt="biomes" style="zoom:55%;" />



+ Construir la gráfica. Aquí viene la parte más importante. En la siguiente imagen tienes la sintaxis en R y su "traducción" a lenguaje humano.



<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/crea_grafica.png" alt="biomes" style="zoom:55%;" />



+ Ahora que entendemos el código, podemos ejecutarlo. Para ello sigue las instrucciones mostradas en las imágenes de abajo:



<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/ejecuta_1.png" alt="biomes" style="zoom:45%;" />



<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/ejecuta_2.png" alt="biomes" style="zoom:50%;" />



<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/ejecuta_3.png" alt="biomes" style="zoom:50%;" />



<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/ejecuta_4.png" alt="biomes" style="zoom:50%;" />





<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/ejecuta_5.png" alt="biomes" style="zoom:50%;" />





<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/ejecuta_6.png" alt="biomes" style="zoom:50%;" />



<img src="https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/ejecuta_7.png" alt="biomes" style="zoom:50%;" />



<div style="page-break-after: always; break-after: page;"></div>

## Discusión de los resultados y ejercicio

Observa tu gráfica con detenimiento y responde a las siguientes preguntas:

+ ¿Crees que el resultado se ajusta a lo esperado en la gráfica teórica? Justifica tu respuesta.
+ ¿hay algún bioma cuyos puntos estén demasiado dispersos? ¿Qué significa eso y por qué crees que ocurre?. En [este](https://github.com/Aprendiendo-cosas/P_biomas_ecologia_ccaa/raw/2020-2021/imagenes/pista.png) enlace he dejado una pista que te ayudará a razonar sobre esta pregunta.

Contesta a las preguntas anteriores razonando todo lo que puedas. El objetivo es que reflexiones y uses argumentos convincentes. No hay una solución única a las preguntas, así que no te obsesiones buscando lo que yo quiero que me digas. Se trata de que aprendas mientas haces el ejercicio. 

Deberás subir las respuestas al moodle en formato **word**, **libre office** o equivalente. No en formato **pdf**, por favor.


## Vídeos de las sesiones

El siguiente vídeo muestra la sesión del GM-1. La del GM-2 se grabó mal.

<iframe width="560" height="315" src="https://www.youtube.com/embed/_6RNbeWjSYU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Y el siguiente muestra el desarrollo completo de la práctica en una grabación hecha por mí a modo de tutorial:

<iframe width="560" height="315" src="https://www.youtube.com/embed/vr9CBeYI1bs" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>