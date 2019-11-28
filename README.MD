# Implementación del Algoritmo de Dijkstra en el campus UNI
## @autor: Cristhian Wiki Sánchez Sauñe (HiroForAnni)
## @autor: ryanwilliamconnor
## Fecha de inclusión: 28/11/2019
![alt text](https://i.imgur.com/thK4CQe.png "Inicio de la interfaz")
### Visión general

Esta aplicación es una recreación de las capacidades de búsqueda de rutas de Google Maps. 
Su componente GUI interactúa con Google Maps. Es una mofificación del programa proporcionado por Ryan Connor, implementado en la Universidad Nacional de Ingeniería, para el curso de Matemática Discreta.

Esta aplicación le mostrará y le dirá el camino más corto desde una intersección inicial hasta hasta siete intersecciones más recorridas  en orden.  Los tiempos se muestran en minutos y las distancias se muestran en kilometros. Esta aplicación también le mostrará y le dirá una ruta que está muy cerca de la ruta más corta desde una intersección de inicio que visita hasta siete intersecciones más exactamente una vez y termina en la ubicación inicial (el problema del vendedor ambulante (TSP)). Aqui nos centraremos en el algoritmo de Dikjstra, no ahondaremos en los otros métodos de optimización.

### Finalidad de esta Aplicación

El propósito de esta aplicación es 

1) dar al usuario la ruta más corta, en términos de duración del viaje, desde una ubicación de inicio hasta siete ubicaciones más (y opcionalmente de vuelta a la ubicación de inicio) y
2) demostrar las diferencias entre varios algoritmos de búsqueda de gráficos .

El usuario puede seleccionar un mapa precargado, cargar su propio mapa, seleccionar una ubicación de inicio, seleccionar hasta siete ubicaciones de detención y seleccionar uno de los cinco algoritmos de ruta de acceso. A continuación, la GUI mostrará visualmente la ruta más corta calculada con el algoritmo seleccionado con paradas numeradas y mostrará algunas estadísticas sobre la ruta.

### Significado de "Ruta más corta" en esta Aplicación

"Ruta más corta" en esta aplicación significa "la ruta de una intersección a otra que minimiza la duración del viaje para un vehículo de motor".
El tiempo recorrido en una carretera se determina únicamente por la longitud y el límite de velocidad de dicha carretera(vía).  Por lo tanto, el tiempo viajado NO tiene actualmente en cuenta variables como "tráfico actual" o "hora del día" (que a su vez podría influir en una variable de "tráfico proyectado").
El límite de velocidad para un tipo de carretera es como cabría esperar para la ubicación promedio en dentro de la UNI: Por ejemplo, las vías automovilísticas tienen un límite de velocidad de 15 kilómetros por hora y las vías peatonales tienen un límite de velocidad peatonal de 6.5 kilómetros por hora.

### Tecnologías utilizadas

El núcleo(kernel) del programa, incluyendo las estructuras de datos de los grafos y algoritmos de búsqueda de rutas, se implementa en Java.
La GUI fue construida principalmente en Java usando JavaFX y GMapsFX, con un poco de HTML, JavaScript y CSS.

### El trabajo en este proyecto

La confirmación inicial para este repositorio es el código de inicio como se describe en
la sección siguiente. Todos los compromisos posteriores son trabajo de Ryan Connor:

1. Diseñar e implementar la estructura de clases para representar carreteras en una ubicación geográfica como un gráfico.
2. Implementado cinco algoritmos de búsqueda de ruta (descritos en una sección a continuación).
3. Se modificó el GUI para acomodar los dos algoritmos TSP.
4. Se ha modificado la GUI para aceptar siete paradas en lugar de solo una.
5. Escribió pruebas para asegurar la precisión de los dos algoritmos TSP (los otros algoritmos fueron calificados por UCSD).

Yo me encargué de modificar el código fuente, asi como traducirlo al español. Añadí una ventana de carga, asi como una moficación de los íconos.

### Algoritmos de Pathing

El usuario puede seleccionar cinco algoritmos diferentes para calcular una ruta más corta:

1. BFS (significa Breadth First Search)
	- Encuentra el camino más corto de principio a fin de usar una simple primera búsqueda de amplitud.
	- Si se selecciona más de una parada, hará BFS desde el inicio hasta la primera parada, luego desde la primera parada hasta la segunda parada, y así sucesivamente.
2. Dijsktra
	- Encuentra el camino más corto desde el principio hasta dejar de usar el algoritmo de Dijkstra.
	- Si se selecciona más de una parada, realizará una búsqueda Dijkstra de inicio a la primera parada, luego desde la primera parada hasta la segunda parada, y así sucesivamente.
3. A*
	- Encuentra la ruta más corta de principio a fin de dejar de usar el algoritmo A*.
	- Si se selecciona más de una parada, realizará una búsqueda A* de inicio a la primera parada, luego desde la primera parada hasta la segunda parada y en.
4. GreedyTSP
	- Intenta encontrar el ciclo hamiltoniano más corto desde el principio a través de cada parada seleccionada utilizando un algoritmo puramente codicioso basado en una búsqueda A* desde la ubicación actual hasta cada parada aún no visitada.
5. Greedy2OptTSP
	- Intenta encontrar el ciclo hamiltoniano más corto desde el principio a través de cada parada seleccionada utilizando un algoritmo codicioso de 2 optes.
	- En primer lugar, Greedy2OptTSP encuentra un ciclo utilizando un algoritmo puramente codicioso basado en la búsqueda A* desde la ubicación actual a cada parada aún no visitada.
	- Entonces, Greedy2OptTSP intenta acortar el ciclo eliminando pares de caminos (S1, S2), (S3, S4) entre paradas y reconstruyendo el ciclo con nuevos caminos (S1, S3), (S2, S4).  Si esto da como resultado una ruta más corta, el proceso se reinicia con esta mejor ruta.  Esto es lo que hace que este algoritmo 2OptTSP	 sea "codicioso" -- para ahorrar tiempo computacional, el algoritmo reinicia el proceso de intercambio inmediatamente después de encontrar un ciclo más corto en lugar de "recordar" el ciclo más corto y continuar a través de todas las posibilidades de intercambio para el ciclo actual.
	- La reconstrucción del ciclo tiene en cuenta las calles unidireccionales. Es decir, el intercambio del par de trayectos (S1, S2), (S3, S4) hacia fuera para los nuevos trayectos (S1, S3), (S2, S4) significa que algunos otros trayectos existentes se deben invertir.  Si las rutas que se van a invertir tienen calles unidireccionales, las rutas invertidas pueden tener un tiempo de viaje diferente al de los trazados no invertidos.  Por lo tanto, si invertir esas rutas da como resultado más tiempo perdido que el intercambio de rutas ahorra tiempo, se rechaza la nueva ruta de acceso.
	- En la única prueba concreta de este algoritmo, el algoritmo dio una solución dentro del 1,5% de lo óptimo en 0.005 segundos para un recorrido mundial de ocho ciudades.

![alt text](https://i.imgur.com/el0qlfe.png "Implementación final del algoritmo de Dikjstra")

### Intrucciones (Uso de esta aplicación)

**Ejecutar la aplicación**
En este momento, la forma más fácil de ejecutar la aplicación es:

1. Descargue todos los archivos de este repositorio en un nuevo directorio de su ordenador.
2. Importe el directorio como un proyecto en el [Eclipse IDE](http://www.eclipse.org/) (enlace proporcionado en caso de que necesite descargar Eclipse).
3. Ejecute el archivo denominado "MapApp.java" como una aplicación Java.

Si quieres usar otro IDE o compilar el programa manualmente, consulta en Google, es tu amigo ;).
Se trato de hacer una aplicación independiente exportando el proyecto como un JAR de Eclipse,pero el mapa de Google no se carga en el JAR.  Me he puesto en contacto con el equipo de desarrollo de UCSD y recorrido Internet sobre este problema en vano.  
Por favor, consulte la pregunta en [StackOverflow](http://stackoverflow.com/questions/35341779/google-maps-api-works-in-eclipse-not-when-exported-as-a-jar/) si está interesado en ayudar a resolver este problema. Me encantaría que contacte al autor original por correo electrónico (ryanwc13@gmail.com) o conmigo (csanchezs@uni.pe) con una solución.

**Uso de la GUI**

Después de que se abra la aplicación, verá un mapa de Google interactivo centrado en UCSD.

*Para utilizar las funciones de búsqueda de ruta:*

1. Cargue un mapa desde el menú desplegable en la parte superior derecha o obtenga un nuevo mapa introduciendo el nombre del archivo en la parte inferior izquierda.
2. Seleccione una ubicación de inicio y haga clic en el botón "Inicio" en la pestaña "Routing".
3. Seleccione una parada y haga clic en el botón "Detener" correspondiente en la pestaña "Routing".
  Haz esto hasta siete paradas.
4. Seleccione una de las cinco estrategias de búsqueda de rutas.
5. Haga clic en el botón "Mostrar ruta".

Una vez que tenga una ruta, puede hacer clic en "Ocultar ruta" para borrar la ruta y seleccionar un nuevo algoritmo.

*Para visualizar el algoritmo de búsqueda de ruta:*

Después de mostrar una ruta, haga clic en el botón "Iniciar visualización" para ver las intersecciones que visita el algoritmo en su camino para determinar la ruta más corta a cada parada.
Esta característica no trabaja actualmente con los algoritmos TSP.

**Notas finales sobre la GUI**

Mejoré la GUI proporcionada como razonable, teniendo en cuenta que mi papel en el proyecto era desarrollar la estructura de clases para representar la ubicación geográfica e implementar los algoritmos de ruta, pero la GUI sigue siendo un poco torpe.
Si los iconos de detención numerados no se muestran correctamente, intente acercar o alejar. 
Esto parece solucionar el problema y puede volver al nivel de zoom original. Además, el mapa no siempre se actualiza correctamente después de la selección de marcadores y el cálculo de ruta.  Actualmente, no hay ningún botón de "reset" (usted puede implementarlo ;) ), por lo que si el mapa se vuelve inutilizable, puede cargar otro mapa o reinicie la aplicación.

### Carga tus propios datos

Hay utilidades en este repositorio que convertirán un mapa de Google Maps en un formulario utilizable por este programa.
Los archivos de datos utilizables son archivos de texto sin formato que terminan en .map en los que cada línea define un segmento de carretera unidireccional (una intersección inicial, una intersección de detención, un nombre de carretera y un tipo de carretera) de la siguiente forma:

[startLat] [startLon] [endLat] [endLon] "[nombre de la carretera]" [tipo de carretera]

Tenga en cuenta que esto significa que una carretera de dos vías entre dos intersecciones necesita dos líneas.
Para cargar sus propios datos (mapa), tiene dos opciones:

1. Especifique la ruta de los datos en la esquina inferior izquierda de la GUI. Usted debe hacer
 esto cada vez que usted recomienza el GUI.
2. Coloque un nuevo archivo de datos en el directorio data/maps/ y agregue el nombre de archivo
 al archivo "mapfiles.list" en ese directorio.  Esto agregará el archivo al menú desplegable del
 mapa en el GUI.

### Código de inicio

*La siguiente sección muestra las notas del equipo UCSD sobre el código de inicio que proporcionaron.
  El código de inicio es la confirmación inicial de este repositorio.*

[**Código de inicio y aplicación GUI para el curso 3 en la programación Java: Diseño orientado a objetos de especialización de estructuras de datos: estructuras de datos avanzadas en Java**](https://www.coursera.org/learn/advanced-data-structures)

Creado por UCSD MOOC Team:
Mia Minnes, Christine Alvarado, Leo Porter, Alec Brickner y Adam Setters
Fecha: 12/16/2015

**DESCRIPCIÓN**

Los archivos proporcionados son código de esqueleto, así como vistas previas de calificación y archivos de prueba que se utilizarán para completar las asignaciones de programación del curso. Además, se le proporciona un programa JavaFX runnable que le ayudará a probar y demostrar sus implementaciones.

**ARCHIVOS POR SEMANA**

A continuación se presentan los archivos introducidos en cada semana y utilizados en cada semana
del curso. Ver archivo para la descripción...

*Semana 1 : Introducción al curso y a los gráficos*

    basicgraph. Graph.java
    basicgraph. GraphAdjList.java
    basicgraph. GraphAdjMatrix.java

*Semana 2: Diseño de clase y búsqueda de gráficos simple*

    cartografía. MapGraph.java
    week2example. Maze.java
    week2example. MazeLoader.java
    week2example. MazeNode.java

*Archivos de utilidad*

    Geografía. GeographicPoint.java
    Geografía. RoadSegment.java
    util. GraphLoader.java

**SETUP** 

Importación de Proyecto en eclipse:

1. Cree un nuevo proyecto Java en su espacio de trabajo
2. Importe los archivos de inicio:
	- Archivo -> Importar -> Seleccione "Sistema de archivos" 
		-> Siguiente -> Examinar y establecer el directorio raíz en el contenido 
            de la carpeta de zip se extrajeron a -> Finalizar

Siéntase libre de usar otro IDE o compilar y ejecutar manualmente sus programas.  
Si necesitas ayuda, google es tu amigo.

### Licencia

Este software está licenciado bajo la [licencia MIT](https://opensource.org/licenses/MIT).  
Consulte el archivo denominado "LICENSE" en este repositorio para obtener más información.
