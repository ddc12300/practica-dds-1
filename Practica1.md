---
title: "Actividad Evaluable 1 - Data Driven Security"
author: "Raquel Abad y Daniel Díaz"
date: "11 de enero, 2024"
output: 
  html_document:
    keep_md: true
---

## Data Science

### Pregunta 1: Clasificación de preguntas

1.  Dado un registro de vehículos que circulan por una autopista, disponemos de su marca y modelo, país de matriculación, y tipo de vehículo (por número de ruedas). Con tal de ajustar precios de los peajes, ¿Cuántos vehículos tenemos por tipo? ¿Cuál es el tipo más frecuente? ¿De qué países tenemos más vehículos?

**Descriptiva**: Se basa en identificar como es el conjunto de datos, en este caso se quieren clasificar los vehículos por tipo, el tipo más frecuente y la procedencia de los vehículos para ajustar el precio de los peajes.

2.  Dado un registro de visualizaciones de un servicio de video-on-demand, donde disponemos de los datos del usuario, de la película seleccionada, fecha de visualización y categoría de la película, queremos saber ¿Hay alguna preferencia en cuanto a género literario según los usuarios y su rango de edad?

**Exploratoria**: Se quiere buscar la relación que existe entre los datos, en este caso de pretende buscar la relación entre el género literario de la pelicula y el rango de edad de los usuarios.

3.  Dado un registro de peticiones a un sitio web, vemos que las peticiones que provienen de una red de telefonía concreta acostumbran a ser incorrectas y provocarnos errores de servicio. ¿Podemos determinar si en el futuro, los próximos mensajes de esa red seguirán dando problemas? ¿Hemos notado el mismo efecto en otras redes de telefonía?

**Predictiva**: Se quieren predecir valores no vistos, en este caso, se intenta predecir si las peticiones futuras de una red de telefonía específica continuarán causando errores.

4.  Dado los registros de usuarios de un servicio de compras por internet, los usuarios pueden agruparse por preferencias de productos comprados. Queremos saber si ¿Es posible que, dado un usuario al azar y según su historial, pueda ser directamente asignado a un o diversos grupos?

**Inferencial**: Se pretende generalizar los datos a una muestra mayor, en este ejemplo, se busca identificar si un usuario puede ser asignado a un grupo basado en su historial de compras, obteniendo así una muestra mayor.

### Pregunta 2: Considera el siguiente escenario:

Sabemos que un usuario de nuestra red empresarial ha estado usando esta para fines no relacionados con el trabajo, como por ejemplo tener un servicio web no autorizado abierto a la red (otros usuarios tienen servicios web activados y autorizados). No queremos tener que rastrear los puertos de cada PC, y sabemos que la actividad puede haber cesado. Pero podemos acceder a los registros de conexiones TCP de cada máquina de cada trabajador (hacia donde abre conexión un PC concreto). Sabemos que nuestros clientes se conectan desde lugares remotos de forma legítima, como parte de nuestro negocio, y que un trabajador puede haber habilitado temporalmente servicios de prueba. Nuestro objetivo es reducir lo posible la lista de posibles culpables, con tal de explicarles que por favor no expongan nuestros sistemas sin permiso de los operadores o la dirección.

### 1. Plantear las preguntas

El primer paso para analizar los datos seria plantear la pregunta. En este caso se podria plantear la siguiente pregunta: Dado el acceso a poder visualizar todas la connexiones TCP de los usuarios de nuestra red empresarial y sabiendo cuales son los servicios web autorizados para poder tener abiertos en la red, se pretende reducir al máximo el número de posibles usuarios que han tenido abiertos servicios no autorizados en la red.

### 2. Obtención de Datos

Para obtener datos para entontrar respuesta a la pregunta planteada

Como punto de partida, se usarían los registros de conexiones TCP de las computadoras de los empleados. Estos registros incluirían detalles como las direcciones IP, los puertos y las marcas de tiempo, que son clave para el análisis.

### 3. Tratamiento de Datos y Análisis Exploratorio

Antes de analizar los datos, sería necesario limpiarlos para quitar cualquier información irrelevante o incorrecta. Esto implicaría eliminar datos duplicados y normalizar los formatos de las direcciones IP y los puertos.

El siguiente paso sería echar un vistazo general a los datos para entender qué está pasando. Se calcularían algunas estadísticas básicas y se observaría cómo se distribuyen las conexiones a lo largo del tiempo para detectar cualquier cosa fuera de lo común.

### 4. Modelar e Identificación de Patrones

Para encontrar pistas sobre los servicios web no autorizados, se aplicarían métodos como el agrupamiento de datos (clustering) y la detección de anomalías. La idea sería buscar cualquier patrón de conexión que no encaje con lo que se espera de un uso normal.

### 5. Visualización de Datos y Comunicación de Resultados

Para hacer más fácil la interpretación de los datos, se crearían gráficos como mapas de calor o gráficos de red. Estas visualizaciones ayudarían a ver mejor los patrones y a explicar los hallazgos a otras personas.

Al final, se prepararía un informe o una presentación con los resultados del análisis. Se usarían las visualizaciones para hacer que la información sea más accesible y se explicaría lo que se encontró de manera clara y sencilla.

## 2. Introducción a R

Como comentamos anteriormente la segunda parte de este informe, se centra en el análisis práctico de un conjunto de datos utilizando R.

El conjunto de datos elegido para este análisis contiene registros de peticiones HTTP, y la tarea consiste en importar y manipular este dataset para responder a preguntas específicas sobre el tráfico de un servidor web. La selección y limpieza de los datos es esencial para garantizar la precisión de nuestro análisis, por lo que previamente a empezar a trabajar prestamos especial atención a la estructura de los datos y a cómo estos pueden ser transformados y analizados eficazmente.

**Fragmento Dataset:**

```         
141.243.1.172 [29:23:53:25] "GET /Software.html HTTP/1.0" 200 1497
query2.lycos.cs.cmu.edu [29:23:53:36] "GET /Consumer.html HTTP/1.0" 200 1325
tanuki.twics.com [29:23:53:53] "GET /News.html HTTP/1.0" 200 1014
wpbfl2-45.gate.net [29:23:54:15] "GET / HTTP/1.0" 200 4889
wpbfl2-45.gate.net [29:23:54:16] "GET /icons/circle_logo_small.gif HTTP/1.0" 200 2624
wpbfl2-45.gate.net [29:23:54:18] "GET /logos/small_gopher.gif HTTP/1.0" 200 935
140.112.68.165 [29:23:54:19] "GET /logos/us-flag.gif HTTP/1.0" 200 2788
```

### Pregunta 1: Una vez cargado el Dataset a analizar, comprobando que se cargan las IPs, el Timestamp, la Petición (Tipo, URL y Protocolo), Código de respuesta, y Bytes de reply. Responde a las preguntas númeradas a continuación:

Utilizando la librería `readr`, cargamos el dataset en nuestro entorno de trabajo en R. En concreto, utilizamos la función `read_table`, que está optimizada para leer archivos grandes al interpretar automáticamente que cada espacio en blanco es un separador entre columnas. Esto nos facilita mucho el trabajo en los siguientes ejercicios.

La función es invocada de la siguiente manera:


```r
library(readr)
data <- read_table("epa-http.csv", col_names = FALSE)
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   X1 = col_character(),
##   X2 = col_character(),
##   X3 = col_character(),
##   X4 = col_character(),
##   X5 = col_character(),
##   X6 = col_double(),
##   X7 = col_character()
## )
```

```
## Warning: 21 parsing failures.
##  row col  expected    actual           file
## 7527  -- 7 columns 6 columns 'epa-http.csv'
## 7528  -- 7 columns 6 columns 'epa-http.csv'
## 7529  -- 7 columns 6 columns 'epa-http.csv'
## 7549  -- 7 columns 6 columns 'epa-http.csv'
## 7550  -- 7 columns 6 columns 'epa-http.csv'
## .... ... ......... ......... ..............
## See problems(...) for more details.
```

Aquí, `read_table` se aplica al archivo "epa-http.csv", y el argumento `col_names = FALSE` indica que el archivo no contiene una fila de cabecera con nombres de columnas. Como resultado, R lee el archivo y asume que cada columna está separada por espacios, creando un dataframe donde cada columna corresponde a un campo del archivo original.

Una vez cargado el dataframe, procedemos a asignar nombres significativos a sus columnas para facilitar el análisis posterior:


```r
colnames(data) <- c("IP", "Timestamp", "Tipo", "URL", "Protocolo", "Respuesta", "Bytes")
```

Esto nos permite referirnos a cada columna por un nombre legible que describe los datos que contiene, mejorando la claridad y mantenibilidad del código en los pasos de análisis subsiguientes.

1.  Cuales son las dimensiones del dataset cargado (número de filas y columnas)


```r
# Número de filas
num_filas <- nrow(data)
paste("Número de filas:", num_filas)
```

```
## [1] "Número de filas: 47748"
```

```r
# Número de columnas
num_columnas <- ncol(data)
paste("Número de columnas:", num_columnas)
```

```
## [1] "Número de columnas: 7"
```

2.  Valor medio de la columna Bytes


```r
# Calculamos el valor medio de la columna Bytes sin tener en cuenta los datos n/a
data$Bytes <- as.numeric(data$Bytes)
```

```
## Warning: NAs introduced by coercion
```

```r
mean_bytes <- mean(data$Bytes, na.rm = TRUE)

paste("El valor medio de la columna Bytes es:", mean_bytes)
```

```
## [1] "El valor medio de la columna Bytes es: 7352.3349128887"
```

### Pregunta 2:De las diferentes IPs de origen accediendo al servidor, ¿cuantas pertenecen a una IP claramente educativa (que contenga ".edu")?


```r
#Para poder utilizar la función str_detect() necesitamos importar la libreria stringr
library(stringr)
#Buscamos cadenas que acaben en .edu
ips_educativas <- sum(str_detect(data$IP, "\\.edu$"))

paste("El número de dominios educativos identificados son ", ips_educativas)
```

```
## [1] "El número de dominios educativos identificados son  6317"
```

### Pregunta 3:De todas las peticiones recibidas por el servidor cual es la hora en la que hay mayor volumen de peticiones HTTP de tipo "GET"?


```r
#Creamos una columna que solo contiene la hora de la peticion
data$Hour <- str_extract(data$Timestamp, "(?<=:)[0-9]{2}(?=:)")


get_requests <- data[data$Tipo == "\"GET", ]
hour_counts <- table(get_requests$Hour)

hour_with_most_gets <- which.max(hour_counts)
hour_with_most_gets_name <- names(hour_with_most_gets)
num_of_gets <- hour_counts[hour_with_most_gets]

paste("La hora con mayor volumen de peticiones GET es: ", hour_with_most_gets_name, "horas, con ", num_of_gets, "peticiones a esa hora")
```

```
## [1] "La hora con mayor volumen de peticiones GET es:  14 horas, con  4546 peticiones a esa hora"
```

### Pregunta 4: De las peticiones hechas por instituciones educativas (.edu), ¿Cuantos bytes en total se han transmitido, en peticiones de descarga de ficheros de texto ".txt"?


```r
data$Bytes <- as.numeric(data$Bytes)

#Buscamos las peticiones que la IP acaba en .edu y la URL acaba en .txt
edu_requests <- data[str_detect(data$IP, "\\.edu$") & str_detect(data$URL, "\\.txt$"), ]
total_bytes_txt_edu <- sum(edu_requests$Bytes, na.rm = TRUE)

paste("Total de bytes transmitidos en peticiones .txt por .edu:", total_bytes_txt_edu)
```

```
## [1] "Total de bytes transmitidos en peticiones .txt por .edu: 106806"
```

### Pregunta 5: Si separamos la petición en 3 partes (Tipo, URL, Protocolo), usando str_split y el separador " " (espacio), ¿cuantas peticiones buscan directamente la URL = "/"?


```r
#Buscamos las URL que coinciden con /
num_peticiones_slash <- sum(data$URL == "/", na.rm = TRUE)

paste("Total de peticiones a la URL '/':", num_peticiones_slash)
```

```
## [1] "Total de peticiones a la URL '/': 2382"
```

### Pregunta 6: Aprovechando que hemos separado la petición en 3 partes (Tipo, URL, Protocolo) ¿Cuantas peticiones NO tienen como protocolo "HTTP/0.2"?


```r
#Buscamos las peticiones que no usan el protocolo HTTP0.2
num_peticiones_no_http <- sum(data$Protocolo != "HTTP/0.2\"")

paste("Total de peticiones que no son con el protocolo HTTP/0.2:", num_peticiones_no_http)
```

```
## [1] "Total de peticiones que no son con el protocolo HTTP/0.2: 47747"
```
