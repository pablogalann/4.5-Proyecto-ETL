# W4 Project - Building mySQL Data-base  II

¿ REALMENTE LA LUNA AFECTA NUESTRO COMPORTAMIENTO CRIMINAL?¿HAY MAS CRIMENES LAS NOCHES DE LUNA LLENA?

![pp](https://github.com/pablogalann/4.5-Proyecto-ETL/blob/main/img/lobo.jpg)


El objetivo del proyecto de esta semana consiste en crear una base de datos a partir de la información obtenida de al menos tres fuentes diferentes, siendo esta misma fiable, consistente y con unas interraciones ajustadas a la idea de negocio. En este caso deducimos a partir de los datos proporcionados que se trata de un videoclub

## Planificación del ejercicio

1. Planificacion, busqueda, obtención de información, de al menos tres fuentes diferentes.

2. Analisis de los datos obtenidos

3. Exploración y limpieza de datos, para lo obtención de tablas fiables y consistentes, mediante el uso de python

4. Exportacion de las tablas desde python a mySQL workbench

5. Creacion de una estructura fiable de interrelacion entre las diferentes tablas, representativa del negocio en cuestión

6. Consulta de datos en función de unos parametros determinados.

## Busqueda y obtención de datos.

1 - Descarga de archivo .csv desde www.kaggle.com,  un archivo sobre los crimenes en la ciudad estadounidense de Baltimore, es fiable, tiene gran cantidad de datos, y la utilizo como base del proyecto.

Una vez tengo esta información, trato de obtener datos suficientes como para demostrar si existe un patrón que relacione los crimenes con los ciclos lunares y con los grandes eventos deportivos, para ello busco interconectar la siguiente información.

2 - Información sobre ciclos lunares, mediante el scrapeo de datos de la web https://www.timeanddate.com/moon/phases/usa/baltimore?year=2017, usando Selenium.
![sl](https://github.com/pablogalann/4.5-Proyecto-ETL/blob/main/img/scrapeo_lunas.PNG). La finalidad de esta tra

3- Información sobre los partidos jugados por el equipo de futbol americano de la ciudad mediante el scrapeo de datos de la web https://www.pro-football-reference.com/teams/rav/2017/gamelog/, usando Selenium.
![sl](https://github.com/pablogalann/4.5-Proyecto-ETL/blob/main/img/scrape_2.PNG)



## Analisis de los datos proporcionados.

- Establecemos que cada una de la busquedas de información correspondera a una tabla, que posteriormente se exportara a mySQL para la creación de la base de datos.

Los datos presentados en este momento, previo a su limpieza, son los siguientes:

1- Tabla de crimenes en baltimore, en el que se encuentran las categorias de CrimeDate,CrimeTime,CrimeCode,Location,Description,Inside/Outside,Weapon,Post,District,Neighborhood,Longitude,Latitude,Location 1,Premise

2- Tabla ciclo lunar donde aparece:
![sl](http://localhost:8888/view/4.5-Proyecto-ETL/img/scrapeo_lunas_1.PNG)



## Exploración y limpieza


Para el desarrollo de esta actividad, utilizaremos siguientes procesos, empleandolos de la manera más adecuada en función de la necesidad de cada campo/tabla 

1. VISUALIZACION DE INFORMACION GENERAL: .head .info, .shape .columns
2. MODIFICACION TITULOS DE LAS COLUMNAS: Queremos evitar espacios y diferencias entre mayusculas y minusculas. 
3. LOCALIZACION VALORES NULOS, en el caso que nos compete no existen demasiados nulos
4. MODIFICACIÓN DE VALORES NULOS E INCONSISTENTES
5. LOCALIZACIÓN Y LIMPIEZA DE VALORES CONSTANTE, en el caso de nos compete existen varios columnas a los largo de las tablas con valor cte.

En este punto se planifica la posible conexión entre ambas tablas a partir de la columna id_ciclo_lunar.
Relacionando los ciclos de la luna, conociendo su inicio y su final, podemos enmarcar la fecha de los crimenes en función a que luna correspondiente al del momento del crimen. Para ello se añaden las columnas que vemos a continuación.

![p](https://github.com/pablogalann/4.5-Proyecto-ETL/blob/main/img/columnas_id_lunas.PNG).

Relacionandola así, de manera de directa con la tabla principal de crimenes 

![lo](http://localhost:8888/view/4.5-Proyecto-ETL/img/id_crimes.PNG).


## Exportación de los datos a mySQL y creacion de la base de datos 'crimenes baltimore'

La exportación de los datos se hace directamente desde python, utilizando el modulo sqlalchemy, donde cada data frame representará una de las tabla de la base de datos, previa conexion con el servidor.

    nombre_dataframe_pandas.to_sql(name='nombre_tabla_basedatos', con=cursor, if_exists='replace', index=False)

Una vez introducidos los dataframe con sus datos limpios, se procede a vincular las tablas entre ellas, mediante el id_ciclo_lunar,para facilitar una unión fiable entre ellas, como se ve a continuación 

![pre](http://localhost:8888/view/4.5-Proyecto-ETL/img/eer.PNG).



## Queries

Consultas realizadas, una vez establecida la base de datos.

¿Es cierto que hay mas crimenes en las noches de luna llena?

Analicemos un ciclo lunar:

- Crimenes en noche de luna llena
![pre](http://localhost:8888/view/4.5-Proyecto-ETL/img/luna_llena_verano.PNG).
- Crimenes en noche de luna nueva
![pre](http://localhost:8888/view/4.5-Proyecto-ETL/img/luna_nueva_verano.PNG).
- Crimenes en noche de cuarto creciente
![pre](http://localhost:8888/view/4.5-Proyecto-ETL/img/cuarto_creciente_verano.PNG).
- Crimenes en noche de cuarto menguante
![pre](http://localhost:8888/view/4.5-Proyecto-ETL/img/cuarto_menguante_verano.PNG).

Analicemos el total de homicidios en el periodo de seis años:

- Homicidios en noche de luna llena
![pre](http://localhost:8888/view/4.5-Proyecto-ETL/img/conteo%20homicidios%20full_moon.PNG).
- Homicidios en noche de luna nueva
![pre](http://localhost:8888/view/4.5-Proyecto-ETL/img/conteo%20homicidios%20new_moon.PNG).
- Homicidios en noche de cuarto creciente
![pre](http://localhost:8888/view/4.5-Proyecto-ETL/img/conteo%20homicidios%20cuarto%20creciente.PNG).
- Homicidios en noche de cuarto menguante
![pre](http://localhost:8888/view/4.5-Proyecto-ETL/img/conteo%20homicidios%20cuarto%20menguante.PNG).




