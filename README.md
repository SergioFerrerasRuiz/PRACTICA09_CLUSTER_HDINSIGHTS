# Solucion ETL con Hadoop en Azure
## ¿Que servicios se usan?:
Los servicios que se usaran para la Solucion ETL para batch precesssing con Hadoop en ambiente Azure, usara los servicios HDInsights + Azure Data Lake Storage Gen 2 + HIVE + SQL Database + Sqoop

![Foto de proyecto](assets/1.png)

## Pasos:
Estos son los paso que hay que seguir para realizar esta solucion.

### Crear un grupo de recursos:
Entramos en crear recurso
![Foto de proyecto](assets/2.png)
Buscamos "Grupos de recursos"
![Foto de proyecto](assets/3.png)
Pulsamos nuevo
![Foto de proyecto](assets/4.png)
Creamos el grupo de recursos poniendo un nombre y localizacion, en este caso Norte de Italia:
![Foto de proyecto](assets/5.png)
Pulsamos Revisar y Crear y pulsamos el boton de crear:
![Foto de proyecto](assets/6.png)
Comprobamos que este creado el grupo de recursos:
![Foto de proyecto](assets/7.png)
![Foto de proyecto](assets/8.png)
### Crear Source: Azure Data Lake Store Gen 2
#### Creacion Storage Account:
Empezamos por la Storage Account:

Buscamos Cuentas de Almacenamiento y clickamos:
![Foto de proyecto](assets/9.png)
Creamos una nueva cuenta de almacenamiento:
![Foto de proyecto](assets/10.png)
Rellenamos con esta info:
![Foto de proyecto](assets/11.png)
![Foto de proyecto](assets/12.png)
Pulsamos siguiente:
![Foto de proyecto](assets/13.png)
Pulsamos siguiente:
![Foto de proyecto](assets/14.png)
Pulsamos siguiente:
![Foto de proyecto](assets/15.png)
Tags lo podemos dejar en blanco, asique pulsamos siguiente:
![Foto de proyecto](assets/16.png)
Pulsamos revisar y crear y click en crear:
![Foto de proyecto](assets/17.png)
![Foto de proyecto](assets/18.png)
### Creacion SQL Server
Ahora crearemos el servicio de SQL Server que es donde se de destianaran los datos

Buscamos SQL Database:
![Foto de proyecto](assets/19.png)
Una vez dentro, hacemos click en crear:
![Foto de proyecto](assets/20.png)
Rellena el grupo de recursos, el nombre de la BBDD y crea un nuevo servidor:
![Foto de proyecto](assets/21.png)
Crea el servidor con los siguientes datos:
- Nombre de servidoor: servidorpractica09
- Ubicacion: ``Norte Italia``
- Metodo de Autenticicacion: ``Uso de la autenticicacion de SQL y Microsoft Entra``
- Usuario: ``admin01``
- Contraseña: ``alumno1234?``
![Foto de proyecto](assets/22.png)
Despues de esto, pulsamos `Establecer Administrador`![Foto de proyecto](assets/23.png)
Buscamos nuestro email correspondiente con nuestra cuenta de estudiante y seleccionamos
![Foto de proyecto](assets/24.png)
Queda algo similar a esto y pulsamos aceptar
![Foto de proyecto](assets/25.png)
Una vez pulsemos aceptar, revisamos que quede el apartado `Crear base de datos`  de esta manera
![Foto de proyecto](assets/26.png)
Pulsamos en el apartado proceso y almacenamiento configurar base de datos
![Foto de proyecto](assets/27.png)
Configuramos la BBDD como en la imagen y pulsamos aplicar
![Foto de proyecto](assets/28.png)
Despues de configurar la BBDD, pulsamos en el apartado Redes
![Foto de proyecto](assets/29.png)
Pulsamos siguiente:
![Foto de proyecto](assets/30.png)
Pulsamos siguientes:
![Foto de proyecto](assets/31.png)
Pulsamos siguiente:
![Foto de proyecto](assets/32.png)
Pulsamos siguiente, lo podemos dejar en blanco:
![Foto de proyecto](assets/33.png)
Pulsamos crear y esperamos a la implementacion:
![Foto de proyecto](assets/34.png)
Una vez implementado, pulsamos ir al recurso:
![Foto de proyecto](assets/35.png)
Despues vamos a editor de consultas:

![Foto de proyecto](assets/36.png)

Nos saldra un error:
![Foto de proyecto](assets/37.png)
Para arreglar el error, salimos a home y pulsamos en todos los recursos:
![Foto de proyecto](assets/38.png)
Pulsamos en servidorpractica09 ``SQL server`` y dentro pulsamos en seguridad>redes:
![Foto de proyecto](assets/39.png)
![Foto de proyecto](assets/40.png)
Pulsamos en redes seleccionadas y en reglas de firewall, pulsamos en agregar direccion IPv4 del cliente:
![Foto de proyecto](assets/41.png)
Nos saldra esto:
![Foto de proyecto](assets/42.png)
Si nos da error pulsamos agregar una regla de firewall:
![Foto de proyecto](assets/43.png)
Escribimos nuestra IP a mano y le ponemos nombre
- Nombre: miIP
- IP inicial: 72.14.201.63
- IP final: 72.14.201.65
![Foto de proyecto](assets/44.png)
Pulsamos guardar:
![Foto de proyecto](assets/45.png)
Despues de esto volvemos SQL database en editor de consultas y pulsamos continuar como sergio.ferreras@tajamar365.com:
![Foto de proyecto](assets/46.png)
Como no funcionaraes pulsamos 188.26.212.189 IP.... ``(Texto azul de arriba)``
![Foto de proyecto](assets/47.png)
Ahora si volvemos a pulsar continuar como sergio.ferreras@tajamar365.com:
![Foto de proyecto](assets/46.png)
![Foto de proyecto](assets/48.png)
### Añadir Managed Identity 
Añadimos Managed Identity a Gen2 y a DataBase. ¿Cual es el proposito de usar Managed Identity?
Para ello buscamos Identidades Administradas y lo pulsamos:
![Foto de proyecto](assets/49.png)
Ahora hacemos click en crear:
![Foto de proyecto](assets/50.png)
Seleccionamos
- Grupo de Recursos: ``Practica09``
- Region: ``Italy North``
- Nombre: ``mi-practica09``
y pulsamos siguiente
![Foto de proyecto](assets/51.png)
Las etiquetas las podemos dejar en blanco, pulsamos en siguiente:
![Foto de proyecto](assets/52.png)
Y ahora pulsamos crear y esperamos a que se implemente:
![Foto de proyecto](assets/53.png)
![Foto de proyecto](assets/54.png)
Ahora volvemos a home>Recursos y buscamos storagegen2preactica09 y pulsamos en el:
![Foto de proyecto](assets/55.png)
Despues vamos a Control de acceso (IAM):
![Foto de proyecto](assets/56.png)
Hacemos click en agregar y acto seguido a asignacion de roles:
![Foto de proyecto](assets/57.png)
![Foto de proyecto](assets/58.png)
Buscamos en la barra ``Propietario de datos de Storage Blob`` y lo seleccionamos, despues pulsaremos siguiente:
![Foto de proyecto](assets/59.png)
En la pestaña miembros, seleccionaremos Identidad administrada y despues seleccionar miembros:
![Foto de proyecto](assets/60.png)
El la seleccion de identidades administrads, seleccionamos ``identidad administrada asignada por el usuario`` y seleccionamos en este caso ``mi-practica09``:
![Foto de proyecto](assets/61.png)
Despues pulsamos seleccionar
![Foto de proyecto](assets/62.png)
Hacemos click en siguiente:
![Foto de proyecto](assets/63.png)
Condiciones las dejamos como aparecen por defecto:
![Foto de proyecto](assets/64.png)
Por ultimo pulsamos en el apartado Revision y Asignacion abajo el boton que pone Revisar y Asignar:
![Foto de proyecto](assets/65.png)
Nos sacara a la pagina de antes
![Foto de proyecto](assets/66.png)
Volvemos a home y buscamos servidorpractica09:
![Foto de proyecto](assets/67.png)
Ahora pulsamos en ``acceso control (IAM)``:
![Foto de proyecto](assets/68.png)
Hacemos click en agregar y acto seguido a asignacion de roles:
![Foto de proyecto](assets/57.png)
![Foto de proyecto](assets/58.png)
Buscamos en la barra de busqueda ``Colaborador de SQL Server`` y lo seleccionamos, pulsamos siguiente:
![Foto de proyecto](assets/69.png)
Seleccionamos identidad administrada y despues en seleccionar miembros:
![Foto de proyecto](assets/70.png) 
En la seleccion de miembros, ponemos en identidad administrada ``Identidad administrada asignada por el usuario`` y seleccionamos mi-practica09 y por ultimo clickamos seleccionar:
![Foto de proyecto](assets/71.png) 
Hacemos click en siguiente:
![Foto de proyecto](assets/72.png)
Hacemos click en Revisar y asignar:
![Foto de proyecto](assets/73.png)
![Foto de proyecto](assets/74.png)
Ahora volvemos a home y buscamos ``Clústeres de HDInsights``:
![Foto de proyecto](assets/75.png)
Ahora pulsamos crear:
![Foto de proyecto](assets/76.png)
Ponemos los datos como la imagen, con el 
- Grupo de recursos ``Practica09``
- Nombre del Clúster: ``clusterpractica09``
- Region: ``Italy North``
![Foto de proyecto](assets/77.png)
Para arreglar el error que sale pulsamos click en ``Haga click aqui para realizar un registro...``
![Foto de proyecto](assets/78.png)
En el buscador ponemos ``HDInsight`` y lo seleccionamos y pulsamos registrarse:
![Foto de proyecto](assets/79.png)
Esperamos y despues de que estemos registrados volvemos a la pestaña ``Crear Clúster`` y veremos que ahora si funciona, despues pulsamos ``seleccionar tipo de cluster``:
![Foto de proyecto](assets/80.png)
Seleccionamos ``Interactive Query``:
![Foto de proyecto](assets/81.png)
La primera parte se deberia de ver asi:
![Foto de proyecto](assets/82.png)
Ahora asignamos el login y password para el Clúster, en mi caso
- Nombre de usuario: ``admin01``
- Contraseña: ``Alumno1234?``
Luego, pulsamos siguiente:
![Foto de proyecto](assets/83.png)
En almacenamiento, seleccionamos lo siguiente y ponemos de nombre de sistema de archivos ``filesystemhive``:
![Foto de proyecto](assets/84.png)
En el apartado de Identidad, seleccionamos ``mi-practica09`` y lo demas en blanco, despues pulsamos siguiente:
![Foto de proyecto](assets/85.png)
El primer apartado se queda por defecto sin tocar nada:
![Foto de proyecto](assets/86.png)
En el segundo apartado lo dejamos como en la siguiente imagen, y despues pulsamos siguiente:
![Foto de proyecto](assets/87.png)
Nos saldra que la cuota es muy alta, para arreglarlo, cambiamos el nodo ``trabajador`` su valor 4 por 1 (En esta parte, puede dar un fallo en el que te diga que no has seleccionado ningun clúster, si pasa eso cambia de zona a una que sea mejor) despues de cambiarlo , te dejara continuar, pulsa siguiente:
![Foto de proyecto](assets/88.png)
![Foto de proyecto](assets/89.png)
Lo dejamos todo como viene por defecto y pulsamos siguiente:
![Foto de proyecto](assets/90.png)
Pulsamos crerar, dependiendo de la conexion que tengamos tardara mas o menos, (Es posible que nos de un error debido a un error del nombre del clúster, si es asi, lo que hay que hacer es cambiarlo, para no liarnos añade letras repetidas como: ``clusteer``):
![Foto de proyecto](assets/91.png)
Despues de un rato, se creará:
![Foto de proyecto](assets/92.png)
### Ingestion de Datos
Una vez hallamos terminado los pasos anteriores, ingestaremos nuestros datos al Data Lake Storage, descargaremos nuestro archivo csv que se llama ``FlightDelayData.csv`` (Se encuentra en este repositorio).
En Azure, nos vamos a ``storagegen2practica09`` y hacemos click en ``Data Lake Storage ``.
![Foto de proyecto](assets/93.png)
Ahora hacemos click en ``Explorador de almacenamiento``:
![Foto de proyecto](assets/94.png)
Haz click en contenedores de Blob:
![Foto de proyecto](assets/95.png)
Y ahora en ``filesystemhive``:
![Foto de proyecto](assets/96.png)
Una vez dentro pulsamos ``Agregar directorio``:
![Foto de proyecto](assets/97.png)
Ahora creamos carpeta con el nombre de ``demo``:
![Foto de proyecto](assets/98.png)
Buscamos la carpeta ``demo`` y nos situamos en ella:
![Foto de proyecto](assets/99.png)
![Foto de proyecto](assets/100.png)
Ahora creamos la carpeta ``FlightDelayData``:
![Foto de proyecto](assets/101.png)
![Foto de proyecto](assets/102.png)
Ahora entramos en la carpeta ``FlightDelayData`` y creamos una carpeta con el nombre `` InputData``:
![Foto de proyecto](assets/103.png)
![Foto de proyecto](assets/104.png)
Ahora creamos una carpeta con el nombre `` output``:
![Foto de proyecto](assets/123.png)
![Foto de proyecto](assets/124.png)
Ahora entramos en ``InputData`` y cargamos el archivo `` FlightDelayData.csv``:
![Foto de proyecto](assets/105.png)
Subimos el archivo:
![Foto de proyecto](assets/106.png)
Si lo has seleccionado, ya estara subido, pulsa cargar:

![Foto de proyecto](assets/107.png)
![Foto de proyecto](assets/108.png)
### Data Extraction con Hive
Volvemos al menú home y hacemos click en nuestro cluster:
![Foto de proyecto](assets/109.png)
Una vez dentro pulsamos ``Inicio de Ambari``:
![Foto de proyecto](assets/110.png)
Nos pedira la creedenciales que escribimos cuando levantamos el clusteer, en este caso son:
- Nombre de usuario: ``admin01``
- Contraseña: ``Alumno1234?``
![Foto de proyecto](assets/111.png)
Una vez dentro pulsamos los 9 cuadraditos azules:
![Foto de proyecto](assets/112.png)
Escogemos Hive View 2.0:
![Foto de proyecto](assets/113.png)
De forma automatica validara el cluster:
![Foto de proyecto](assets/114.png)
Despues de validar el cluster, se nos mostrara el editor de queries de Hive:
![Foto de proyecto](assets/115.png)
Ahora copiaremos y pegaremos el contenido del fichero ``query-1.txt`` esta en este mismo repositorio y pulsamos ``Execute``:
![Foto de proyecto](assets/116.png)
Despues vamos al apartado tables para comprobar que se ha ejecutado todo correctamente:
![Foto de proyecto](assets/117.png)
![Foto de proyecto](assets/118.png)
Volvemos a Query y pulsanro ``+`` abrimos un nuevo worksheet:
![Foto de proyecto](assets/119.png)
Dentro del nuevo worksheet, hacemos una consulta como puede ser ``select * from flightdelay limit 5;`` y pulsamos el boton ``execute``:
![Foto de proyecto](assets/120.png)
El resultado de la consulta se ve abajo en results:
![Foto de proyecto](assets/121.png)
(Puede dar un error en esta parte, porque crea otra ruta demo/..., si es asi carga en los dos demos el csv)
En esta sección hemos realizado una ingestión desde Data Lake Storage Gen 2 hasta un lugar dentro del cluster utilizando T-SQL mediante Hive.
### Data Transformation con Hive
Abrimos un nuevo worksheet y ejecutamos la siguiente query ``(query-3.txt)`` archivo que se encuentra en este repositorio:
![Foto de proyecto](assets/122.png)
¿Que hace ``(query-3.txt)`` ?
El código SQL selecciona el nombre de la ciudad de origen y el promedio de los retrasos por clima de la tabla flightdelay, eliminando las comillas simples del nombre de la ciudad y filtrando los registros con retrasos por clima no nulos. Luego, agrupa los resultados por ciudad de origen y los guarda en un archivo CSV en el directorio especificado (/demo/FlightDelayData/output), con los campos separados por comas.

Ahora volvemos a Azure a storagegen2practica09 y ahcemos Click en containers y luego click a filesystemhive:
![Foto de proyecto](assets/126.png)
![Foto de proyecto](assets/127.png)
Navegaremos hasta el output, para ello selecciona demo:
![Foto de proyecto](assets/128.png)
Ahora entra en ``FlightDelayData``:
![Foto de proyecto](assets/129.png)
Y por ultimo en output:
![Foto de proyecto](assets/130.png)
Veremos la carpeta output (la creara si no existe), dentro veremos los resultados:
![Foto de proyecto](assets/125.png)
### Data Export mediante Sqoop
Esta parte de aqui no funciona, es por eso por lo que no esta explicada