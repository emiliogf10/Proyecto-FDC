# CARRERA DE ESPECIALISTA EN HACKING ÉTICO

# PRIMER CURSO : ANÁLISIS FORENSE BÁSICO EN SISTEMAS WINDOWS (Tiempo de realización: 6-7 dias aproximadamente)

## Dia 14/12/2023

Para estos dos primeros cursos, opto por empezar creando una máquina virtual Windows 10 en Virtual Box. Con la máquina ya creada y configurada con las Guest adittions, configuración de red y demás, empiezo por el primer punto del primer curso:

## _COOKIES_

Las cookies son ficheros de texto que almacenan información. Suelen utilizarse normalmente para recordar accesos a páginas web(la web identifica tu ordenador,por lo tanto, si vuelves a entrar en ellas sabrán quien eres y que has echo anteriormente) y conocer hábitos de navegación.En el caso de un forense de Windows, lo que interesa es encontrar esos archivos en el sistema (mas adelante veremos algunos comandos para encontearlos en el sistema) y dentro de ellos, analizar y comprobar si se ha filtrado información o documentación.

Para todo ellos, con la máquina virtual corriendo, lanzamos una consola **con permisos de administrador**.Nos colocamos en la raiz de C: y escribimos el siguiente comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/23f98567a166444481860c2b1ea5eab6b4e95008/Hacking_%C3%89tico/cookies1.png)

Este comando sirve para buscar en todo el sistema, todos los archivos que empiecen por cookie,que tengan cualquier cosa a continuación, un punto y cualquier extensión de archivo. /s lo utilizaremos para que busque tambien en subdirectorios y la /p para que nos pagine la búsqueda. Si le damos a intro, nos resultados como este:


![](https://github.com/emiliogf10/Proyecto-FDC/blob/23f98567a166444481860c2b1ea5eab6b4e95008/Hacking_%C3%89tico/cookies2.png)

Nos vamos a fijar, especialmente en las de mozilla firefox(sabemos que mozilla es un navegador, por lo tanto van a ser cookies de navegación), en donde podemos ver que hay un archivo con extensión sqlite:


![](https://github.com/emiliogf10/Proyecto-FDC/blob/23f98567a166444481860c2b1ea5eab6b4e95008/Hacking_%C3%89tico/cookies3.png)


Lo primero que haremos será ir a la ubicación del archivo, que en este caso es 'C:\Users\user\AppData\Roaming\Mozilla\Firefox\Profiles\qydm2eh3.default-release'. Cuando tengamos el archivo localizado, abriremos Mozilla (o cualquier otro navegador) y agregamos una extensión para la manipulación y visualización de archivos sqlite (en mi caso instalé la extensión SQLite Manager):


![](https://github.com/emiliogf10/Proyecto-FDC/blob/23f98567a166444481860c2b1ea5eab6b4e95008/Hacking_%C3%89tico/cookies4.png)

Abrimos la extensión:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/23f98567a166444481860c2b1ea5eab6b4e95008/Hacking_%C3%89tico/cookies5.png)

Y arrastramos el archivo en cuestión:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/23f98567a166444481860c2b1ea5eab6b4e95008/Hacking_%C3%89tico/cookies6.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/23f98567a166444481860c2b1ea5eab6b4e95008/Hacking_%C3%89tico/cookies7.png)

Vemos que arriba nos aparece una consulta sql, en donde podremos cambiar el 'limit' de 10 a 20 por ejemplo, para que nos aparezcan más resultados.Entre los datos más importantes aparecen: el host de origen(podemos ver en mi caso de Google y Marca), cuando expiran,el último acceso y cuando se crearon entre otros.Muchos de los lugares(hosts) que aparecen y sabemos que no hemos navegado en ellos, posiblemente sean cookies de terceros(en la mayoria de casos, estas cookies tienen finalidad publicitaria). Este archivo procederiamos a guardarlo para posibles incidencias futuras.

### LUGARES EN DONDE HEMOS NAVEGADO

Para encontrar estos lugares, nos vamos a la consola **en modo administrador** y escribimos en siguiente comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ef531b1305171f2f4fa2b7371604670e17a5abd7/Hacking_%C3%89tico/cookies9.png)

En donde busca todos los archivos que tengan el nombre history con cualquier extensión y en todos los subdirectorios del sistema.Algunos de los resultados de este comando son estos:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/23f98567a166444481860c2b1ea5eab6b4e95008/Hacking_%C3%89tico/cookies8.png)

En donde podemos ver que al igual que antes, aparecen muchos resultados, pero los que nos interesan son los archivos de los navegadores. En esta foto podemos ver que hay un archivo 'History' en la carpeta del navegador Chrome.Lo que tenemos que hacer es ir a esa ruta en nuestra máquina y abrir el archivo con la extensión de Mozilla, ya que es un archivo sql.

En el curso, el profesor ejecuta otro comando distinto; **>dir index.dat /s /p /a**, en donde busca todos los archivos index.dat en todo el sistema, incluidos los subdirectorios. Resulta que index.dat guardaba los datos de navegación de Internet Explorer, y éste al desaparecer, se cambió la forma de guardado de los navegadores.
Para Edge y Chrome existe la carpeta History dentro de sus respectivas carpetas y dentro de UserData, sin embargo para firefox tenemos un archivo sql llamado places.sql, que abriremos con la extensión de Mozilla para ver su contenido.

## Dia 15/12/2023

### APLICACIONES QUE SE HAN EJECUTADO

Antes de ver el comando que nos mostrará las aplicaciones que se han ejecutado en nuestro equipo, vamos a abrir por ejemplo un wordpad en nuestra máquina, para ver si posteriormente se guarda su apertura. Por lo tanto, abrimos un wordpad y vemos la hora en la que se abrió:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ef531b1305171f2f4fa2b7371604670e17a5abd7/Hacking_%C3%89tico/cookies10.png)

Y ahora para poder ver las aplicaciones que se han ejecuado en nuestro equipo, simplemente vamos a ejecutar este comando en la consola, siempre **en modo administrador**:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ef531b1305171f2f4fa2b7371604670e17a5abd7/Hacking_%C3%89tico/cookies11.png)

En donde este comando busca en todo el equipo, incluyendo subdirectorios, los archivos que tienen extensión .pf (prefetch), que no son más que los archivos que guardan como se cargan y ejecutan las aplicaciones.Algunos de los resultados son los siguientes:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ef531b1305171f2f4fa2b7371604670e17a5abd7/Hacking_%C3%89tico/cookies12.png)

En donde vemos que en la penúltima posición aparece el wordpad que abrimos anteriormente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ef531b1305171f2f4fa2b7371604670e17a5abd7/Hacking_%C3%89tico/cookies13.png)

Toda esta información, se recomienda guardarla en un archivo de texto, para posteriormente buscar algún programa malicioso que se haya ejecutado o incluso programas extraños que no conozcamos y se ejecuten en horas extrañas del dia.También si tenemos conocimiento de que se ha producido una brecha de seguridad o una filtración a ciertas horas del día, podriamos comprobar qué programas se han ejecutado a esas horas.

### LISTADO CACHE MUI

Para este punto, necesitaremos una herramienta llamada **"MUICacheView"**. La descargamos en nuestra máquina ([Pincha aqui para ir a la página de descarga](https://www.nirsoft.net/utils/muicache_view.html)) y la abrimos **en modo administrador**. Este es su aspecto al abrirla:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ef531b1305171f2f4fa2b7371604670e17a5abd7/Hacking_%C3%89tico/cookies14.png)

## Dia 18/12/2023

En este programa se van a ver todos los programas que se han lanzado, y han escrito una clave en windows (con esto quiero decir que se deja una entrada en el Registro de Windows).Todas las herraminetas que vamos a ver, en principio, van a tener el mismo menú; si seleccionamos varias aplicaciones y le damos a File, podremos eliminarlos de la caché (acción peligrosa, ya que si eliminamos una entrada incorrecta, podriamos interferir en el correcto funcionamiento de alguna aplicación) o incluso guardarlos en un archivo de texto o .CSV (archivo de texto que se usa para guardar datos tabulares, que no son más que los datos de una tabla separados por comas).

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ef531b1305171f2f4fa2b7371604670e17a5abd7/Hacking_%C3%89tico/cookies15.png)

Despues de ver el programa, en el Registro de Windows podriamos ver cuando se ha lanzado cierta aplicación, y tener una idea de lo que ha pasado.

### BASES DE DATOS DE MINIATURAS

Lo que vamos a ver en este apartado, son básicamente, las miniaturas de imágenes, videos u otros archivos multimedia, que están guardadas en el sistema en archivos .db(bases de datos). Aunque tu elimines, por ejemplo, alguna imagen o algún video de tu ordenador, internamente, siguen estando en el sistema en forma de estas miniaturas, que no son más que un tamaño reducido de estos archivos que el sistema utiliza para proporcionar vistas previas más rápidas al navegar entre las carpetas.Para ver las miniaturas que tenemos en nuestro equipo, escribimos el siguiente comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ef531b1305171f2f4fa2b7371604670e17a5abd7/Hacking_%C3%89tico/cookies16.png)

Este comando busca en todas las carpetas del sistema, incluidos los subdirecotrios, todos los archivos que empiecen por thumb y tengan la extensión ".db".En versiones anteriores de Windows (de Windows 7,no ncluido, para atrás), los archivos se llamaban "Thumbs.db", sin embargo, ahora los archivos se llaman "thumbcache_xxx.db", en donde las x son un identificador único.Incluimos también la paginación, para que los resultados nos aparezcan por páginas y bien ordenados.Los resultados de este comando son los siguientes:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ef531b1305171f2f4fa2b7371604670e17a5abd7/Hacking_%C3%89tico/cookies17.png)

Ahora nos iriamos a la carpeta en donde nos hemos encontrado estos archivos y los intentariamos abrir **con permisos de administrador** con un programa llamado **"Windows File Analyzer" (WFA Thumbs)**.[El enlace para la página está aqui](https://www.mitec.cz/wfa.html).La interfaz del programa seria la siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ef531b1305171f2f4fa2b7371604670e17a5abd7/Hacking_%C3%89tico/cookies18.png)

Con el siguiente menú:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ef531b1305171f2f4fa2b7371604670e17a5abd7/Hacking_%C3%89tico/cookies19.png)

En donde en la primera opción abririamos los archivos antes mencionados. Sin embargo, al utilizar una máquina virtual y al no tener directorios borrados, los archivos .db salen como corruptos y no se pueden abrir.

Por el resto del programa, vemos que las opciones son las siguientes:

2. Ver los programas/aplicaciones que se han lanzado (archivos prefetch).
3. Leer los shortcuts en la carpeta especificada y mostrar los datos almacenados en ellos.
4. Abre los archivos Index.dat de internet explorer.
5. Analizar la papelera de reciclaje.

## _CAPTURA DE LA MEMORIA VOLATIL_

En este apartado vamos a ver el guardado de la memoria Volatil. La memoria volatil de un sistema es la memoria de acceso aleatorio(RAM), que se utiliza para almacenar temporalmente datos y programas mientras el sistema está en funcionamiento.

Para todo ello, utilizaremos una herramienta llamada **"Dumpit"**, que hará un volcado de la memoria en formato .raw y del mismo tamaño que la memoria actual.Descargamos la herramienta (en mi caso desde [Aqui](https://www.toolwar.com/2014/01/dumpit-memory-dump-tools.html)) y la lanzaremos **en modo administrador**.La apariencia es la siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ef531b1305171f2f4fa2b7371604670e17a5abd7/Hacking_%C3%89tico/volatil1.png)

En donde escribiremos la "y" y se nos hará el volcado en la carpeta en donde pone Destination:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ef531b1305171f2f4fa2b7371604670e17a5abd7/Hacking_%C3%89tico/volatil2.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ef531b1305171f2f4fa2b7371604670e17a5abd7/Hacking_%C3%89tico/volatil3.png)

Vemos que, efectivamente, el archivo .raw es del tamaño de la RAM de la máquina virtual (3GB):

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ef531b1305171f2f4fa2b7371604670e17a5abd7/Hacking_%C3%89tico/volatil4.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ef531b1305171f2f4fa2b7371604670e17a5abd7/Hacking_%C3%89tico/volatil5.png)

Con el archivo .raw creado, solo faltaria analizarlo con herramientas forenses como Volatility o WinDbg.

## _CAPTURA DE LA MEMORIA DE PAGINACIÓN_

## Dia 19/12/2023

La memoria de paginación es una memoria "auxiliar" que utiliza en sistema operativo cuando necesita más memoria de la que está disponible físicamente en la RAM. El sistema operativo simula una "extensión" de la memoria RAM, mediante el uso de espacio en el disco duro.

Por defecto, el archivo de paginación en sistemas operativos Windows, se llama **pagefile.sys** y se encuentra generalmente en la raiz de C:.

Como curiosidad, decir que el sistema operativo realiza un proceso que mueve datos entre la RAM y el archivo de paginación llamado "Swapping de datos". El proceso consiste en mover los datos menos utilizados por la RAM o inactivos al archivo de paginación para liberar espacio y darle prioridad a datos activos.

Para todo ello, vamos a comprobar que existe el archivo "pagefile.sys" en RAIZ DE C:. Vamos a nuestra consola y escribimos el siguiente comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/36952c590493288740896e67e6654cb8c792d220/Hacking_%C3%89tico/paginacion1.png)

Evidentemente, este comando lo lanzaremos cuando estemos en raiz de C:. Lo que hace, es simplemene visualizar los archivos y directorios que hay en dicha carpeta, incluyendo los archivos ocultos.El resultado es el siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/36952c590493288740896e67e6654cb8c792d220/Hacking_%C3%89tico/paginacion2.png)

En donde vemos podemos ver en medio, que efectivamente encontramos el archivo citado anteriormente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/36952c590493288740896e67e6654cb8c792d220/Hacking_%C3%89tico/paginacion3.png)

Dicho archivo, deberiamos guardarlo tambien, al igual que la memoria volatil (RAM). Al intentar copiarlo surge un problema, y es que el archivo no se puede copiar porque está bloqueado por Windows. De ahi surge la necesidad de utilizar un programa con el cual podamos copiar dicho archivo, y se llama "ShadowCopy". Lo instalamos en nuestra máquina, y lo abrimos **en modo administrador**. La apariencia es la siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/36952c590493288740896e67e6654cb8c792d220/Hacking_%C3%89tico/paginacion4.png)

En donde:
1. En la opción "copy from" ponemos el directorio C:\.
2. En la opción "file mask" ponemos su extensión; *.sys dentro de los asteriscos que vienen por defecto.
3. En la opción "copy to" ponemos el directorio en donde queremos que se copie el archivo o los archivos.
4. Desmarcamos la primera opción de copiar subdirectorios.

Quedaria asi:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/36952c590493288740896e67e6654cb8c792d220/Hacking_%C3%89tico/paginacion5.png)

Y le damos a copy.

![](https://github.com/emiliogf10/Proyecto-FDC/blob/36952c590493288740896e67e6654cb8c792d220/Hacking_%C3%89tico/paginacion6.png)

Vemos que está intentando buscar los archivos, y efectivamente empieza a copiar el archivo que queremos:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/36952c590493288740896e67e6654cb8c792d220/Hacking_%C3%89tico/paginacion7.png)

Y termina de copiarlo satisfactoriamente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/36952c590493288740896e67e6654cb8c792d220/Hacking_%C3%89tico/paginacion8.png)

Para ver que se copió correctamente en el directorio que pusimos, vamos a la consola y nos colocamos en dicho directorio. Después, escribimos el siguiente comando que muestra los archivos y directorios, incluyendo ficheros ocultos. Y este es el resultado:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/36952c590493288740896e67e6654cb8c792d220/Hacking_%C3%89tico/paginacion9.png)

Posteriormente, estos archivos los guardariamos en una carpeta de pruebas junto con los de los apartados anteriores.

## _USUARIOS QUE HAN INICIADO SESIÓN EN EL SISTEMA_


Para saber los usuarios que han iniciado sesión en el sistema, vamos a descargar una herramienta llamada **"PsLoggedon"**. Al intentar descargar esta herramienta, en mi caso desde la página oficial de Microsoft, descargo un kit de herramientas forenses bastante útiles en las que se incluye la que necesitamos ([Pincha aqui para descargar el kit de herramientas](https://learn.microsoft.com/es-es/sysinternals/downloads/psloggedon#installation)). Extraemos los archivos y colocamos el ejecutable en una carpeta accesible; en mi caso, muevo los ejecutables (todas las herramientas del kit forense) a una carpeta de herramientas situada en el escritorio. Con esto hecho, nos vamos a nuestra consola de Wondows, **en modo administrador** y nos colocamos dentro de la carpeta en donde metimos el ejecutable. Acto seguido, lo ejecutamos:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/36952c590493288740896e67e6654cb8c792d220/Hacking_%C3%89tico/logged1.png)

En la imagen podemos ver que, en mi caso, el único usuario que inició sesión en el sistema ha sido "user", que es el nombre del usuario que estoy utilizando para estas pruebas.Esto también lo guardariamos en la carpeta de evidencias con los archivos anteriores (habria que añadirle al nombre de la herramienta un > y un nombre de archivo; por ejemplo, el comando en este caso seria **PsLoggedon.exe > usuariosLogueados.txt** por ejemplo).

## _CAPTURA DE SERVICIOS EN EJECUCIÓN_

Un servicio es un proceso o aplicación que se ejecuta en segundo plano. Generalmente están diseñados para realizar tareas específicas sin requerir interacción con el usuario y algunos ejemplos son los servicios web, servicios de red, servicios de impresión...etc. Por todo ello es interesante capturar en tiempo real los servicios en ejecución, para comprobar si hay funcionamiento de algún RAT ([Mas información sobre los RAT aqui](https://es.wikipedia.org/wiki/Troyano_de_acceso_remoto));

Para todo ello, debemos utilizar herramientas que no sean del SO porque podrian estar comprometidas por un Rootkit (Un rootkit en esencia es un software malicioso diseñado para ocultar la presencia de actividades maliciosas o el acceso no autorizado en un sistema informático; por ejemplo para ocultar un RAT). En este caso, utilizaremos una herramienta del kit descargado en el apartado anterior, llamada **"psService"**. Para ello, nos vamos a la consola **en modo administrador**, nos colocamos en la carpeta en donde tenemos el ejecutable (en mi caso, como dije antes, tengo todas las herramientas en una carpeta en el escritorio llamada herramientas) y lo ejecutamos. Estos son algunos de los resultados:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/36952c590493288740896e67e6654cb8c792d220/Hacking_%C3%89tico/servicios1.png)

Como vemos en la imagen, salen todos los servicios en ejecución en el sistema y podemos ver que pueden estar parados o en ejecución(propiedad state). Todo ello lo añadiriamos a un archivo de texto como se explicó en el apartado anterior (en este caso el comando seria **PsService.exe > servicios.txt**), y lo incluiriamos en la carpeta de incidencias, junto con los documentos de los apartados anteriores.

## _CAPTURA DE PROCESOS EN EJECUCIÓN_

## Dia 20/12/2023

Un proceso es un conjunto de recursos que usa el sistema operativo para disponer de un programa en ejecución. Los procesos son independientes, y el mal funcionamiento de uno no debería afectar a otros.

Para ver los procesos que hay en este momento corriendo en nuestro sistema, utilizaremos una herramienta del kit descargado anteriormente; la herramienta en cuestión es **"PsList"**.

Nos vamos a la consola **en modo administrador**, nos colocamos en la carpeta en donde tenemos el ejecutable (en mi caso, como dije antes, tengo todas las herramientas en una carpeta en el escritorio llamada herramientas) y lo ejecutamos. Estos son algunos los resultados:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a1d510206943722a8355c02825acd32cbbdfe09c/Hacking_%C3%89tico/procesos1.png)

Todos estos resultados, como viene siendo de costumbre, los metemos en un archivo de texto como se explicó en el apartado anterior (en este caso seria **pslist.exe > procesos.txt**) y se meteria en la carpeta de evidencias junto con los documentos de apartados anteriores.

Como curiosidad, a la hora de visualizar los procesos, vemos que el primer proceso es **"Idle"**:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a1d510206943722a8355c02825acd32cbbdfe09c/Hacking_%C3%89tico/procesos2.png)

Es importante revisar el estado de este proceso, ya que representa a la inactividad del sistema y por lo tanto si su PID no es 0 podría ser sintoma de que la CPU no está completamente inactiva y por lo tanto podria significar que hay un problema en el sistema como procesos en segundo plano o software malicioso.

## _CAPTURA DE ESTADO DE RED (PROMISCUOUS DETECT)_

Entre todas las evidencias que estamos guardando, es importante recavar información de red; dominios permitidos, estado DHCP, caché de DNS,tablas de netbios...etc.

1. Mostrar tabla ARP del sistema ([Pulsa aqui para saber más sobre las tablas ARP](https://www.ionos.es/digitalguide/servidores/know-how/arp-resolucion-de-direcciones-en-la-red/)). Para mostrarla, tendremos que utilizar el comando **"arp -a"** en una consola **en modo administrador**. El resultado es el siguiente:

   ![](https://github.com/emiliogf10/Proyecto-FDC/blob/a1d510206943722a8355c02825acd32cbbdfe09c/Hacking_%C3%89tico/red1.png)

   Esta tabla se guardaria en un documento de texto con el comando **"arp -a > estadored.txt"** y se añadiria a la carpeta de evidencias.
   
2. Mostrar tabla NETBIOS (https://learn.microsoft.com/es-es/windows-server/administration/windows-commands/nbtstat). Para mostrarla y guardarla en nuestro archivo de estado de red, tendremos que utilizar el comando **"nbtstat -c >> estadored.txt"**.En este caso no muestro el resultado del comando, porque mi máquina virtual utiliza otro protocolo de red (DNS).
   
3. Mostrar información sobre los adaptadores de red. Para ello utilizaremos el comando **"ipconfig /all >> estadored.txt"** para guardar el resultado en nuestro documento de estado de red. El resultado del comando sería el siguiente en mi caso:

  ![](https://github.com/emiliogf10/Proyecto-FDC/blob/a1d510206943722a8355c02825acd32cbbdfe09c/Hacking_%C3%89tico/red2.png)

  Deberiamos revisar nuestra tarjeta de red y nuestra dirección MAC para ver si coinciden, y comprobar que no se está haciendo [macspoofing](https://es.wikipedia.org/wiki/MAC_spoofing).
  
4. Mostrar tabla DNS ([Más información aqui](https://www.redeszone.net/tutoriales/redes-cable/como-ver-borrar-cache-dns-windows/)). Para ello utilizaremos el comando **"ipconfig /displaydns"** que nos dará un resultado como este:

  ![](https://github.com/emiliogf10/Proyecto-FDC/blob/a1d510206943722a8355c02825acd32cbbdfe09c/Hacking_%C3%89tico/red3.png)

  En donde se ve el nombre de dominio con su respectiva dirección IP. Añadimos esta tabla a nuestro archivo de estado de red mediante **"ipconfig /displaydns >> estadored.txt"**.

5. Mostrar estadísticas de red ([Más información aqui](https://www.ionos.es/digitalguide/servidores/herramientas/una-introduccion-a-netstat/)). Para ello utilizaremos el comando **"netstat -an"**. El resultado es el siguiente:

   ![](https://github.com/emiliogf10/Proyecto-FDC/blob/a1d510206943722a8355c02825acd32cbbdfe09c/Hacking_%C3%89tico/red4.png)

   Podemos ver que con ete comando vemos los puertos que están abiertos y a la escucha, con su respectivo ID.Este resultado también lo guardaremos en nuestro archivo de estado de red mediante el comando **"netstat -an >> estadored.txt"**.

## Dia 24/12/2023

## _CAPTURA MBR (REGISTRO DE ARRANQUE PRINCIPAL)_

Para esto, necesitamos saber un poco sobre un diSpositivo de almacenamiento de datos ([Más información aqui](https://www.ionos.es/digitalguide/servidores/herramientas/una-introduccion-a-netstat/)). Lo básico es saber que la parte mínima de un dispositivo de almacenamiento de datos es un sector (normalmente 512 bytes) y éstos se agrupan en clústers (grupo de sectores); pues bien, en el sector 0 se encuentra alojado el MBR. Contiene informacíon relativa a cómo iniciar el sistema, qué tipo de particiones hay en el dispositivo y el tamaño de las mismas, etc. En cierto tipo de incidentes, principalmente relacionados con malware, puede resultar de interés extraerlo para que en un posterior análisis se determine si está infectado.

En este caso, vamos a analizar el análisis directamente y para ello, vamos a descargar una herramienta llamada **MBRCheck** ([Descarga MBRCheck](https://www.majorgeeks.com/files/details/mbrcheck.html)). Con ella ya descargada, vamos al directorio en donde tenemos el ejecutable, y lo ejecutamos. La apariencia es la siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/68a68ce6b9ec58c2ca46f407fc49ab95c0e41a35/Hacking_%C3%89tico/mbr1.png)

Vemos que nos dice que el MBR no es estándar y esto se debe a que estamos utilizando una máquina virtual(si en una máquina real hubiera algún problema con el MBR, seguiriamos los pasos que nos da el programa). El programa crea un fichero de todo este análisis y lo guarda (en mi caso) en el escritorio. Veámoslo:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/68a68ce6b9ec58c2ca46f407fc49ab95c0e41a35/Hacking_%C3%89tico/mbr2.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/68a68ce6b9ec58c2ca46f407fc49ab95c0e41a35/Hacking_%C3%89tico/mbr3.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/68a68ce6b9ec58c2ca46f407fc49ab95c0e41a35/Hacking_%C3%89tico/mbr4.png)

Este archivo, básicamente es un resumen de todo lo que ha analizado y al final su resultado. Este fichero lo guardariamos en nuestra carpeta de evidencias con las demás pruebas.

## _FIRMAR NUESTROS ARCHIVOS_

Por último, necesitariamos firmar nuestros archivos para que sirvieran como prueba de que el archivo es veridico y no ha sido cambiado. Tendriamos que buscar algun programa para ello, firmar los archivos y guardar la clave HASH que te daria el programa junto con su respectivo archivo. Evidentemente, los firmariamos todos y los guardariamos cada uno en una carpeta con su HASH y a la vez todos dentro de una carpeta general de evidencias.

## Dia 27/12/2023

# SEGUNDO CURSO : ANÁLISIS FORENSE AVANZADO EN SISTEMAS WINDOWS (Tiempo de realización: 6-7 dias aproximadamente)

## _CAPTURAR ESTRUCTURA MAC DE FICHEROS Y ARCHIVOS_

En este apartado lo que vamos a hacer es capturar el "timeline" de un archivo. Con una serie de comandos en consola, veremos que ha pasado y cuando en diferentes archivos.

 En este caso, voy a listar los archivos del directorio en donde tengo las herramientas; pero antes crearé un archivo llamado "prueba" y le añadiré un texto para ver si se muestra como el último archivo modificado:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/05dd4e7dd876b05cc8376c784598d859ba65b6ce/Hacking_%C3%89tico/mac1.png)

Y con el siguiente comando vamos a obtener los diretorios cuyo tipo sean de escritura, ordenados por fecha y buscando también en los subdirectorios. Incluye también la paginación:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/05dd4e7dd876b05cc8376c784598d859ba65b6ce/Hacking_%C3%89tico/mac2.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/05dd4e7dd876b05cc8376c784598d859ba65b6ce/Hacking_%C3%89tico/mac3.png)

En donde podemos ver, que al final de todo y antes de que muestre los archivos de un subdirectorio llamado "muicacheview", aparece el archivo de prueba que hemos creado anteriormente.

![](https://github.com/emiliogf10/Proyecto-FDC/blob/05dd4e7dd876b05cc8376c784598d859ba65b6ce/Hacking_%C3%89tico/mac4.png)

Al comando visto anteriormente, podemos cambiarle por ejemplo, a que en vez de mostrar los archivos por última modificación, nos muestre los archivos pero por último acceso. El comando en cuestión quedaría asi:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/05dd4e7dd876b05cc8376c784598d859ba65b6ce/Hacking_%C3%89tico/mac5.png)

En donde sólo sustituiriamos la 'w' por la 'a' en el tipo. Evidentemente seguiría apareciendo el archivo creado de último porque a parte de modificarlo, lo he abierto para comprobar y enseñaros la modificación. En este apartado, sólo listamos los archivos del directorio "herramientas" porque si llegamos a ejecutar este comando en la raiz de C:, nos listaria los archivos de todo el sistema y seria mucha información.

También cabe decir que ordenar toda la información por fecha, nos servirá para encontrar el o los archivos que en la fecha del incidente han sido modificados o abiertos. 
De ahi, ya habría que analizar el archivo y ver que ha pasado en el sistema.

Toda esta información, debemos agregarla a un archivo y guardarla en una carpeta de evidencias. En este caso y de forma sencilla guardariamos esta información en un archivo llamado **"archivos.txt"** con este comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/05dd4e7dd876b05cc8376c784598d859ba65b6ce/Hacking_%C3%89tico/mac6.png)

En donde solo agregariamos el más igual junto con el nombre que le queremos poner al archivo. El archivo vemos que se guarda en la carpeta "herramientas":

![](https://github.com/emiliogf10/Proyecto-FDC/blob/05dd4e7dd876b05cc8376c784598d859ba65b6ce/Hacking_%C3%89tico/mac7.png)

## _CAPTURAR CONTRASEÑAS DESDE NAVEGADORES_

En este apartado, necesitaremos una herramienta llamada **"WebBrowserPassView"** ((Descarga WebBrowserPassView)[https://www.nirsoft.net/utils/web_browser_password.html]). Con ella ya descargada, introducimos su ejecutabale en la carpeta herramientas (opcional) y la ejecutamos **en modo administrador**. La apariencia es la siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/05dd4e7dd876b05cc8376c784598d859ba65b6ce/Hacking_%C3%89tico/contra0.png)

Como podemos ver, no aparece nada, y eso se debe a que en la máquina virtual que estoy utilizando no hay guardada ninguna contraseña de ningún sitio web en ningún navegador. Dicho esto, pongo 3 imágenes sacadas del video de este curso y explicando un poco el programa:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/05dd4e7dd876b05cc8376c784598d859ba65b6ce/Hacking_%C3%89tico/contra1.png)

En la imagen anterior, podemos ver que el programa encuentra dos contraseñas; una de amazon y otra de facebook. En un equipo normal, aparecerían muchas más como cuentas bancarias, correos electrónicos, juegos, etc. Si damos doble click sobre la primera, se no abrirá una ventana en donde nos darán más información:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/05dd4e7dd876b05cc8376c784598d859ba65b6ce/Hacking_%C3%89tico/contra2.png)

Nos dice:
1. El sitio web es amazon.com.
2. La contraseña está guardada en Mozilla (en este caso está guardada aqui, pero nos podemos encontrar contraseñas de muchos navegadores).
3. El nombre de ususario.
4. En el apartado de Password Field nos dice que la contraseña está encriptada (por eso en el apartado de password hay una cadena tan larga).
5. En el apartado password strenght nos dice que la contraseña es muy fuerte.
6. Fecha de creación y modificación.
7. Directorio en donde se guarda toda esta información.

Si le damos doble click en la segunda contraseña, nos aparecerá la misma información (evidentemente con sus datos correspondientes) pero veriamos que en este caso facebook no encripta la contraseña:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/05dd4e7dd876b05cc8376c784598d859ba65b6ce/Hacking_%C3%89tico/contra3.png)

Visto esto también vamos a ver en donde podemos "filtrar" los navegadores que nos interesan. Para ello, vamos a "Options" y entramos en "Advanced Options". Nos saldrá la siguiente ventana emergente en donde podremos seleccionar los navegadores que nos convengan:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/05dd4e7dd876b05cc8376c784598d859ba65b6ce/Hacking_%C3%89tico/contra4.png)

Por último, guardariamos las contraseñas que nos interesaran en un archivo; para ello, seleccionamos con un click aquellas que nos interesen y le damos a la opción "File" y "Save Selected Items". Le indicaremos en donde queremos guardar el fichero junto con su nombre y listo (lo suyo sería guardarlo directamente en la carpeta de evidencias).

![](https://github.com/emiliogf10/Proyecto-FDC/blob/05dd4e7dd876b05cc8376c784598d859ba65b6ce/Hacking_%C3%89tico/contra5.png)

## Dia 28/12/2023

## _CAPTURAR DISPOSITIVOS USB QUE SE HAYAN CONECTADO_

En este apartado utilizaremos una herramienta llamada **"USBHistory"** ((Descarga USBHistory aqui)[https://sourceforge.net/projects/usbhistory/]). Esta herramienta, al igual que algunas anteriores que hemos utilizado, no se lanza como entorno gráfico sino que se lanza desde linea de comandos. Para ello lanzamos una consola **en modo administrador** y nos vamos a la carpeta en donde tenemos el ejecutable (yo meti la carpeta del programa en la carpeta herramientas). Desde ahi lanzamos el archivo .exe y ocurre esto:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ee0ea80a05e71ab892e08f39ce7bad12eed04a51/Hacking_%C3%89tico/usb1.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ee0ea80a05e71ab892e08f39ce7bad12eed04a51/Hacking_%C3%89tico/usb2.png)

Vemos que no nos aparece nada, y eso es debido a que estamos trabajando en una máquina virtual y no hemos introducido ningún USB. Si hubieramos introducido algún dispositivo USB en la máquina en esa lista nos aparecería el nombre del dispositivo, la MAC y la fecha de introducción entre otras cosas.

Es importante listar estos dispositivos, debido a que la mayoria de incidentes de fuga de información se producen a través de ellos. 

## _CAPTURA LA CONFIGURACIÓN FIREWALL_

En este apartado vamos a ver el registro de las reglas del firewall. Esto consiste en inspeccionar, analizar y verificar las reglas de firewall.

Para ver estas reglas debemos de entrar al editor de registro. Esto se hace buscando en tu equipo **"regedit"** y lo abrimos **en modo administrador**. Nos saldrá esto:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ee0ea80a05e71ab892e08f39ce7bad12eed04a51/Hacking_%C3%89tico/firewall1.png)

Desde aqui tendremos que navegar hasta una carpeta llamada "FirewallRules", que se encontrará en la siguiente dirección: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SharedAccess\Parameters\FirewallPolicy\FirewallRules**. Nos encontraremos con todo esto:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ee0ea80a05e71ab892e08f39ce7bad12eed04a51/Hacking_%C3%89tico/firewall2.png)

Lo que haremos será exportar las reglas y guardarlas en nuestra carpeta de evidencias. Lo haremos haciendo click con el botón derecho encima de la carpeta FirewallRules y exportar. Se abrirá una ventana en donde nos pedirá el nombre y la ubicación de guardado (lo guardaremos en nuestra carpeta de eviencias). Hecho esto, iremos a abrir el archivo que hemos guardado pero **OJO, NO SE PUEDE ABRIR HACIENDO DOBLE CLICK, SI NO LAS REGLAS SE CARGARIAN OTRA VEZ EN EL REGISTRO, POR LO TANTO ABRIMOS EL ARCHIVO CON OTRA APLICACIÓN, COMO POR EJEMPLO EL BLOC DE NOTAS**. El resultado es el siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ee0ea80a05e71ab892e08f39ce7bad12eed04a51/Hacking_%C3%89tico/firewall3.png)

Aqui se tendrian que analizar las últimas entradas de estas reglas, pero eso ya es un uso muy avanzado que no explican en este curso.

## _CAPTURA DE REGISTRY SPAWINGS_

En este apartado vamos a revisar los programas por defecto que tienen asignadas ciertos tipos de extensiones; algunos atacantas modifican estos registros para que por ejemplo un archivo .txt en vez de abrirlo con un bloc de notas se abra una consola. Para ver esto, nos vamos otra vez al regedit. Buscamos en nuestro equipo regedit y lo ejecutamos **en modo administrador**. Desde aqui, desplegamos la opción de "HKEY_CLASSES_ROOT" y tendremos que buscar las extensiones que nos interesen. En mi caso, en primer lugar voy a buscar los archivos .txt. Veamos que nos aparece:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ee0ea80a05e71ab892e08f39ce7bad12eed04a51/Hacking_%C3%89tico/registro1.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ee0ea80a05e71ab892e08f39ce7bad12eed04a51/Hacking_%C3%89tico/registro2.png)

Podemos ver que los archivos .txt, se abren por defecto con notepad. Ahora vamos a ver los documentos docx:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ee0ea80a05e71ab892e08f39ce7bad12eed04a51/Hacking_%C3%89tico/registro3.png)

En donde vemos que se abririan con Microsoft Word. De este modo revisariamos todas las extensiones que nos interesasen y las copiariamos y pegariamos en un documento de texto que posteriormente guardaremos en nuestra carpeta de evidencias.

## _CAPTURA DE LISTADO DE APLICACIONES INSTALADAS_

En este apartado vamos a listar todas las aplicaciones que tenemos instaladas en el sistema. Para ello vamos a ir otra vez al regedit, lo buscamos y lo abrimos **en modo administrador**. Nos vamos a la siguiente dirección: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall**.

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ee0ea80a05e71ab892e08f39ce7bad12eed04a51/Hacking_%C3%89tico/app1.png)

Y podremos ver información de las aplicaciones:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ee0ea80a05e71ab892e08f39ce7bad12eed04a51/Hacking_%C3%89tico/app2.png)

Aqui lo que nos interesaria es ver si todas las aplicaciones son legítimas; podemos ver que de toda la información que nos da, también aparece la URL del desarrollador. Si no apareciera el nombre de desarrollador pero si la URL, estariamos hablando de una aplicación potencialmente maliciosa.

Con todo esto, exportamos todo esto a un fichero mediante botón derecho encima de la carpeta "uninstall" y le damos a exportar con el nombre que queramos y en la ubicación que queramos; preferiblemente en la carpeta de evidencias.

## Dia 07/01/2024

## _USO DE VOLATILITY_

Con esta herramienta vamos a analizar en volcado de memoria que hicimos con la herramienta **Dumpit** en el primer curso. Para ello tendremos que generar u obtener previamente un archivo .raw y situarlo por ejemplo en nuestra carpeta de herramientas. A continuación, descargaremos la herramienta **Volatility** ((Descarga Volatility aqui)[https://www.volatilityfoundation.org/releases]) y colocaremos el ejecutable en la carpeta herramientas. A continuación ejecutaremos el siguiente comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/02f3c86325dd4b760b7db37a5a0c0cf82e737497/Hacking_%C3%89tico/vol1.png)

En donde "volatilityxxx.exe" es el ejecutable antes mencionado, "imageinfo" es un plugin de Volatility que se utiliza para obtener información sobre la imagen de la memoria, "-f" se utiliza para especificar que a continuación se escribirá un archivo y por último, como acabo de decir, el propio archivo que será el ".raw".El comando en si lo que hace es analizar el volcado de memoria que le damos y muestra información general, como la versión, arquitectura,etc. El resultado del siguiente comando es el siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/02f3c86325dd4b760b7db37a5a0c0cf82e737497/Hacking_%C3%89tico/vol2.png)

En el apartado "Suggested Profile(s)" nos dice que no hay un perfil aconsejado, entoces tendremos que ver los perfiles que hay y ver cual es el que más nos interesa. Para ello vamos a escribir el siguiente comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/02f3c86325dd4b760b7db37a5a0c0cf82e737497/Hacking_%C3%89tico/vol3.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/02f3c86325dd4b760b7db37a5a0c0cf82e737497/Hacking_%C3%89tico/vol4.png)

En donde podemos ver que nos da una lista (incompleta porque no cabian todos en pantalla) de los perfiles que hay disponibles, a parte de mas información que ahora mismo no nos interesa.

# TERCER CURSO : ANÁLISIS FORENSE BÁSICO EN SISTEMAS LINUX (Tiempo de realización: 6-7 dias aproximadamente)

Para estos dos siguientes cursos, voy a crear una máquina virtual Linux, en mi caso una Ubuntu de 2GB de RAM. Dicho esto y con la máquina ya configurada, empezamos con el primer punto:

Para empezar este curso deberemos saber unos comandos básicos en consola (iremos viendo su funcionamiento a medida que avance el curso):

1. 'sudo' -> Este comando lo utilizaremos para ejecutar otros comandos con privilegios de superusuario o administrador.
2. 'cd' -> Este comando lo utilizaremos para navegar entre las distintas carpetas.
3. 'ls' -> Este comando se utiliza para listar el contenido de un directorio.
4. 'cat' -> Este comando sirve para concatenar archivos, pero nosotros lo vamos a utilizar mayoritariamente para mostrar el contenido de un archivo.
5. 'grep' -> Este comando lo utilizaremos para buscar patrones de texto en archivos o para filtrar lineas de texto que coincidan con un patrón específico.
6. 'nano' -> Lo vamos a utilizar para editar archivos de texto o incluso sólo para ver su contenido, al igual que cat.
7. 'su' -> Este comando lo pondremos solo, y lo que va a hacer es que vamos a cambiar al usuario root con todos los privilegios.
8. 'find' -> Este comando se utiliza para buscar archivos y directorios , basándose en diversos criterios como el nombre del archivo, la fecha de modificación, el tamaño y otros atributos. 

Evidentemente, también deberemos crear una carpeta de evidencias en donde guardaremos todas las pruebas que vayamos recavando (en mi caso la crearé en el escritorio). Esto se puede hacer fácilmente con el comando "mkdir":

![](https://github.com/emiliogf10/Proyecto-FDC/blob/9d1e7c45e2a4a11043571eab14df4420d2149259/Hacking_%C3%89tico/evidencias1.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/9d1e7c45e2a4a11043571eab14df4420d2149259/Hacking_%C3%89tico/evidencias2.png)

## _ARCHIVOS DE REGISTRO_

Los archivos de registro también conocidos por "logs" o "registros" son archivos que contienen información de diversas actividades del sistema, aplicaciones o servicios. Son muy útiles a la hora de detectar fallos e intentar solucionarlos. Los podremos encontrar en la carpeta **/var/log**. Dicho esto, nos vamos a nuestra máquina virtual y escribimos el siguiente comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/02f3c86325dd4b760b7db37a5a0c0cf82e737497/Hacking_%C3%89tico/reg1.png)

En donde simplemente nos colocamos en la carpeta arriba indicada. Ahora hacemos el "ls" para ver los archivos que contiene dicha carpeta:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/02f3c86325dd4b760b7db37a5a0c0cf82e737497/Hacking_%C3%89tico/reg2.png)

Vemos que nos aparecen diferentes archivos de registro (evidentemente solo aparecen los disponibles y esto variará dependiendo del equipo) como:

1. Auth.log -> log de autenticación.
2. Boot.log -> Registro de inicio del sistema.
3. Kern.log -> Registro de kernel.

Al ser una máquina virtual nunca antes usada solo aparecen estos dos archivos "destacables" pero nos podriamos encontrar con otros como :

1. Message -> Registro de mensajes del sistema.
2. Maillog -> Registro de servidor de mails.
3. mysqld.log -> Registro de la base de datos Mysql.

Si quisieramos abrir alguno de estos archivos para ver su contenido, lo primero que hariamos seria pasarlos a usuario root:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/9d1e7c45e2a4a11043571eab14df4420d2149259/Hacking_%C3%89tico/reg3.png)

Y despues ejecutariamos el siguiente comando para ver el contenido del archivo:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/9d1e7c45e2a4a11043571eab14df4420d2149259/Hacking_%C3%89tico/reg4.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/9d1e7c45e2a4a11043571eab14df4420d2149259/Hacking_%C3%89tico/reg5.png)

El resultado del comando ls, lo meteremos en un archivo y lo enviaremos a la carpeta de evidencias:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/9d1e7c45e2a4a11043571eab14df4420d2149259/Hacking_%C3%89tico/reg6.png)

Y firmaremos el archivo, metiendo su hash en un archivo .txt de firmas ubicado también en la carpeta evidencias:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/9d1e7c45e2a4a11043571eab14df4420d2149259/Hacking_%C3%89tico/reg7.png)


## _ARCHIVO SUDOERS_

En este fichero se especifican los permisos que tiene cada usuario en nuestro sistema. Accederemos a el y comprobaremos que no hay ningún usuario que no tenga que estar ahí. Para ello, con el usuario root ejecutaremos el siguiente comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/9d1e7c45e2a4a11043571eab14df4420d2149259/Hacking_%C3%89tico/sudoers1.png)

Y el resultado será el siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/9d1e7c45e2a4a11043571eab14df4420d2149259/Hacking_%C3%89tico/sudoers2.png)

En donde vemos los permisos que tiene cada usuario y por supuesto que no hay nigún usuario extraño. Vemos que el usuario "emilio" no aparece, y la razón es que el usuario "emilio" pertenece al grupo "emilio" y este grupo no tiene permisos para utilizar el comando sudo, con lo cual no va a aparecer en el fichero sudoers. Evidentemente esto se podría cambiar con un comando, agregando al usuario "emilio" al grupo "sudo" (un usuario puede pertenecer a varios grupos).

Este archivo lo copiaremos y lo meteremos en la carpeta de evidencias:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/9d1e7c45e2a4a11043571eab14df4420d2149259/Hacking_%C3%89tico/sudoers3.png)

Y lo firmaremos, metiendo su hash en el fichero de firmas anterior:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/9d1e7c45e2a4a11043571eab14df4420d2149259/Hacking_%C3%89tico/sudoers4.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/9d1e7c45e2a4a11043571eab14df4420d2149259/Hacking_%C3%89tico/sudoers5.png)

Podemos ver que esta firma y la anterior se metieron correctamente en el fichero firmas.txt.

## Dia 10/01/2024

## _LISTADO DE PAQUETES INSTALADOS_

Un paquete no es más que una unidad de software que contiene archivos y metadatos necesarios para instalar y gestionar un programa o una aplicación. Para ver los paquetes que tenemos instalados en el sistema, utilizaremos el siguiente comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/bc23a473ad55081295f8ab392c2c189279b22cc4/Hacking_%C3%89tico/paquetes1.png)

En donde le añadimos "-l" para visualizarlo en formato lista. El resultado sería el siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/bc23a473ad55081295f8ab392c2c189279b22cc4/Hacking_%C3%89tico/paquetes2.png)

Un comando similar sería este:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/bc23a473ad55081295f8ab392c2c189279b22cc4/Hacking_%C3%89tico/paquetes3.png)

En donde el resultado sería el siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/bc23a473ad55081295f8ab392c2c189279b22cc4/Hacking_%C3%89tico/paquetes4png.png)

Podemos ver que nos aparece el nombre y la versión, pero si sólo queremos el nombre, ejecutariamos el siguiente comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/bc23a473ad55081295f8ab392c2c189279b22cc4/Hacking_%C3%89tico/paquetes5.png)

En donde podemos ver que efectivamente, aparece sólo el nombre del paquete:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/bc23a473ad55081295f8ab392c2c189279b22cc4/Hacking_%C3%89tico/paquetes6.png)

Todos estos comandos, es recomendable que los ejecutemos siempre **en modo root**.Los guardaremos directamente en un archivo de texto como hemos hecho en casos anteriores. Como ejemplo, voy a guardar el último comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/bc23a473ad55081295f8ab392c2c189279b22cc4/Hacking_%C3%89tico/paquetes7.png)

Estos tres comandos, al mostrarnos algo similar, sólo guardaremos en un archivo uno de ellos; evidentemente alguno de los dos que hemos visto que nos de más información.

Lo siguiente es eliminar los paquetes que no se usan nunca en el sistema. Para ello, Linux tiene un comando que los elimina a todos automáticamente y es el siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/bc23a473ad55081295f8ab392c2c189279b22cc4/Hacking_%C3%89tico/paquetes8.png)

En donde nos pregunta si estamos seguros de que queremos eliminar 391 MB de espacio en disco (paquetes que no se utilizan). Le decimos que si y este es el resultado:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/bc23a473ad55081295f8ab392c2c189279b22cc4/Hacking_%C3%89tico/paquetes9.png)

## Dia 15/01/2024

## _GUARDADO DEL FICHERO SHADOW Y PASSWD_

En este apartado vamos a hacer una copia del fichero 'shadow' y del fichero 'passwd'. Para ello vamos a ver qué es cada fichero:

1. shadow -> Este fichero es el que almacena las contraseñas de los usuarios de manera segura; es decir, cifradas. Este archivo es esencial para el sistema y ayuda a proteger las contraseñas de los usuarios contra accesos no autorizados.
2. passwd -> Este fichero almacena información sobre los usuarios del sistema. Proporciona una lista de usuarios y sus detalles pero sin sus contraseñas reales, sino que cifradas.

Dicho esto, vamos a hacer una copia de los dos archivos y guardarlas en nuestra carpeta de evidencias. Para ello nos vamos a la consola **en modo root** y ejecutamos los siguientes comandos:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/1fd2f3e75f2024d164bffd63e49c23860a62e69a/Hacking_%C3%89tico/copia1.png)

En donde simplemente hacemos uso del comando cp(copy) y le damos el fichero que copiar y la carpeta en donde queremos que nos coloque la copia con su respectivo nombre. Asi para los dos ficheros. Ahora vamos a nuestra carpeta de evidencias y vemos que, efectivamente se han copiado correctamente los ficheros:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/1fd2f3e75f2024d164bffd63e49c23860a62e69a/Hacking_%C3%89tico/copia2.png)

Ahora, vamos a firmar nuestros archivos y meter su respectivo HASH en nuestro archivo de firmas. Ejecutariamos los siguientes comandos:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/1fd2f3e75f2024d164bffd63e49c23860a62e69a/Hacking_%C3%89tico/copia3.png)

En donde vemos que firmamos los archivos y añadimos el HASH a nuestro archivo de firmas. Nos vamos al archivo firmas y vemos que efectivamente se añadieron correctamente los HASH:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/1fd2f3e75f2024d164bffd63e49c23860a62e69a/Hacking_%C3%89tico/copia4.png)

## _FILTRADO DEL FICHERO SHADOW_

En este apartado vamos a filtrar el archivo shadow del sistema y lo vamos a comparar con nuestro archivo shadow guardado en el apartado anterior. Para ello ejecutamos el siguiente comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/1fd2f3e75f2024d164bffd63e49c23860a62e69a/Hacking_%C3%89tico/filtrado1.png)

En donde:

1. 'cut' -> Es el comando principal de filtrado; extrae secciones específicas de lineas de texto según unos criterios.
2. '-d:' -> Especifica el delimitador que el comando cut va a utilizar para dividir las lineas. En este caso el delimitador van a ser los dos puntos (el fichero shadow utiliza los dos puntos para separar sus campos).
3. '-f2' -> Indica que solo se debe extraer el segundo campo después del primer delimitador de cada línea (en el fichero shadow, el segundo campo de cada línea suele ser la contraseña cifrada de cada usuario).
4. '/etc/shadow' -> El archivo del cual se extraerán los datos.
5. '|more' -> El indicador '|' indica que la salida del comando anterior se redirige al siguiente comando, que en este caso es more, y significa que el contenido se va a mostrar de forma paginada.

El resultado seria el siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/1fd2f3e75f2024d164bffd63e49c23860a62e69a/Hacking_%C3%89tico/filtrado2.png)

En donde comparariamos este contenido con el fichero que teniamos guardado previamente y revisariamos si se produció algún cambio.

## _GUARDADO DE FICHEROS Y DIRECTORIOS_

En este apartado vamos a guardar una lista con los ficheros con permisos 444 (sólo lectura) y otra lista con los directorios con permisos 111 (sòlo ejecución). Esto es importante, ya que por ejemplo la lista de los directorios con permisos de sólo ejecución, nos va a ayudar a saber en donde puede el usuario ejecutar programas o scripts pero no visualizar el contenido de ese directorio. En el caso de los ficheros, es importante saber los ficheros de sólo lectura porque éstos podrian contener información crítica o logs y si observamos algún cambio en alguno de ellos podrían indicar una manipulación indebida.

En definitiva, estas dos listas son dos pruebas más para el forense que le ayudarán a ver o comprender el por qué del problema al que nos enfrentamos. Dicho esto, vamos a guardar primero los archivos con permisos 444 ejecutando el siguiente comando **en la raíz del sistema**:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/1fd2f3e75f2024d164bffd63e49c23860a62e69a/Hacking_%C3%89tico/fi1.png)

En donde:

1. 'find' -> Comando principal para buscar archivos y directorios.
2. '-type f' -> Especifica que se están buscando archivos mediante 'f'.
3. '-perm -444' -> Especifica que se están buscando archivos con permisos 444 (sólo lectura para todos los usuarios).
4. '> /home/emilio/Desktop/evidencias/ficheros_444' -> Indica que la salida del comando find sea redirigida a un archivo llamado 'ficheros_444'.

Y vemos que el archivo se guardó correctamente en nuestra carpeta de evidencias:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/1fd2f3e75f2024d164bffd63e49c23860a62e69a/Hacking_%C3%89tico/fi2.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/1fd2f3e75f2024d164bffd63e49c23860a62e69a/Hacking_%C3%89tico/fi3.png)

Ahora vamos a guardar los directorios con permisos 111. Ejecutamos el siguiente comando **en la raíz del sistema**:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/1fd2f3e75f2024d164bffd63e49c23860a62e69a/Hacking_%C3%89tico/fi4.png)

En donde:

1. 'find' -> Comando principal para buscar archivos y directorios.
2. '-type d' -> Especifica que se están buscando directorios mediante 'd'.
3. '-perm -111' -> Especifica que se están buscando archivos con permisos 111 (sólo ejecución para todos los usuarios).
4. '> /home/emilio/Desktop/evidencias/directorios_111' -> Indica que la salida del comando find sea redirigida a un archivo llamado 'directorios_111'.

Y vemos que el archivo se guardó correctamente en nuestra carpeta de evidencias:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/1fd2f3e75f2024d164bffd63e49c23860a62e69a/Hacking_%C3%89tico/fi5.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/1fd2f3e75f2024d164bffd63e49c23860a62e69a/Hacking_%C3%89tico/fi6.png)

Con los dos archivos guardados, como de costumbre, hay que firmarlos. Para ello ejecutariamos los siguientes comandos desde la carpeta de eviencias:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/1fd2f3e75f2024d164bffd63e49c23860a62e69a/Hacking_%C3%89tico/fi7.png)

Y vemos que, efectivamente los archivos se firmaron correctamente y se guardaron sus respectivos HASH en el fichero firmas:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/1fd2f3e75f2024d164bffd63e49c23860a62e69a/Hacking_%C3%89tico/fi8.png)

## Dia 16/01/2024

## _CAPTURA DE LO QUE HA ESCRITO EL USUARIO EN LA CONSOLA_

En este apartado vamos a guardar en un fichero, lo que ha escrito el usuario por consola. Esta información se encuentra en un archivo oculto del sistema en la carpeta de usuario. Lo que guarda este archivo, es básicamente cada comando que ejecuta el usuario en la terminal. Dicho esto, vamos a copiar el archivo a nuestra carpeta de evidencias:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/e3c42c8c60f7687caeed11d595897cf632a66685/Hacking_%C3%89tico/h1.png)

En donde simplemente hacemos uso del comando cat, redirigido a un archivo con el nombre de 'bash_history'.

![](https://github.com/emiliogf10/Proyecto-FDC/blob/e3c42c8c60f7687caeed11d595897cf632a66685/Hacking_%C3%89tico/h2.png)

Y podemos ver que se ha guardado correctamente con los comandos hechos hasta ahora. Con este archivo guardado, revisaríamos si hay algún comando inusual o que nos parezca extraño.

Por último faltaría firmar el archivo y guardar su HASH en nuestro archivo de firmas:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/e3c42c8c60f7687caeed11d595897cf632a66685/Hacking_%C3%89tico/h3.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/e3c42c8c60f7687caeed11d595897cf632a66685/Hacking_%C3%89tico/h4.png)

## _CAPTURA DEL ENTORNO DE RED_

En este apartado, simplemente vamos copiar a un archivo, todo el entorno de red del sistema. Para ver todo ello, ejecutaremos el siguiente comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/e3c42c8c60f7687caeed11d595897cf632a66685/Hacking_%C3%89tico/ip1.png)

Con el resultado siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/e3c42c8c60f7687caeed11d595897cf632a66685/Hacking_%C3%89tico/ip2.png)

En donde podemos ver todas las interfaces de red disponibles, con su dirección IP, dirección MAC,etc. Lo siguiente es introducir todo esto en un fichero, mediante el siguiente comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/e3c42c8c60f7687caeed11d595897cf632a66685/Hacking_%C3%89tico/ip3.png)

En donde redirigimos la salida del comando 'ip addr' a un fichero de texto llamado 'red.txt' situado en evidencias.

![](https://github.com/emiliogf10/Proyecto-FDC/blob/e3c42c8c60f7687caeed11d595897cf632a66685/Hacking_%C3%89tico/ip4.png)

Vemos que el archivo se guardó correctamente y su contenido es el visto anteriormente.

## _INFORMACIÓN DE LAS CONEXIONES DE RED_

En este apartado vamos a capturar la información de las conexiones de red. Para ello vamos a ejecutar el siguiente comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/e3c42c8c60f7687caeed11d595897cf632a66685/Hacking_%C3%89tico/net1.png)

En donde las opciones '-an' muestran todas las conexiones, escuchas y direcciones y números de puerto en formato numérico (sin resolver los nombres mediante DNS). El resultado es el siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/e3c42c8c60f7687caeed11d595897cf632a66685/Hacking_%C3%89tico/net2.png)

Toda esta información, la introduciremos en el mismo archivo del apartado anterior, ya que es información de red. Para ello, ejecutaremos el siguiente comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/e3c42c8c60f7687caeed11d595897cf632a66685/Hacking_%C3%89tico/net3.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/e3c42c8c60f7687caeed11d595897cf632a66685/Hacking_%C3%89tico/net4.png)

Y podemos ver que la información se añadió correctamente.

## _DIAGNOSIS CON EL COMANDO DIG_

En este apartado vamos a realizar una diagnosis del estado de ciertos entornos de la red. Para ello, vamos a ver cómo funciona el comando 'dig'. Básicamente, dicho comando realiza consultas DNS en donde nos da información detallada como direcciones IP asociadas, servidores de nombres autoritativos, registros de recursos, etc. Dicho esto, nosotros vamos a ejecutar el comando 'dig' solo para que haga una consulta al servidor de nombres local y nos de información.Para ello ejecutamos el comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/e3c42c8c60f7687caeed11d595897cf632a66685/Hacking_%C3%89tico/dig1.png)

Y la respuesta es la siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/e3c42c8c60f7687caeed11d595897cf632a66685/Hacking_%C3%89tico/dig2.png)

En donde en la sección 'answer section' nos muestra la lista de los servidores DNS de nivel superior ([Dominios de nivel superior](https://www.redeszone.net/tutoriales/internet/dominios-tld/)) para el dominio raiz ('.').Entre otras opciones, también nos aparece en la sección 'SERVER' la dirección IP y el puerto del servidor DNS utilizado para la consulta . 

Dicho todo esto, añadiriamos toda esta información al archivo de los dos apartados anteriores. Ejecutamos el siguiente comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/e3c42c8c60f7687caeed11d595897cf632a66685/Hacking_%C3%89tico/dig3.png)

Y observariamos que se añadió correctamente al archivo:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/e3c42c8c60f7687caeed11d595897cf632a66685/Hacking_%C3%89tico/dig4.png)

[MÁS INFORMACIÓN SOBRE EL COMANDO DIG](https://www.hostinger.es/tutoriales/comando-dig-linux)

## _CAPTURA DE LA TABLA DE ENRUTAMIENTO_

En este apartado queremos capturar la tabla de enrutamiento del sistema. La razón de ello es que esta tabla nos proporciona información sobre la conectividad de red y cómo el sistema decide enviar paquetes de datos a través de la red ([Más información sobre el comando route](https://somebooks.es/comando-route-ubuntu/)). Para ello vamos a ejecutar el siguiente comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/e3c42c8c60f7687caeed11d595897cf632a66685/Hacking_%C3%89tico/route1.png)

En donde la salida es la siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/e3c42c8c60f7687caeed11d595897cf632a66685/Hacking_%C3%89tico/route2.png)

[MÁS INFORMACIÓN SOBRE LAS TABLAS DE ENRUTAMIENTO](https://www.mallotore.com/blog/entendiendo-la-tabla-de-rutas-linux/)

Guardamos la salida de este comando en el archivo de red, ejecutando el siguiente comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/e3c42c8c60f7687caeed11d595897cf632a66685/Hacking_%C3%89tico/route3.png)

Y vemos que se añadió correctamente la información a nuestro archivo de red:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/e3c42c8c60f7687caeed11d595897cf632a66685/Hacking_%C3%89tico/route4.png)

Solo faltaría firmar nuestro archivo de red y guardar su corrspondiente HASH en nuestro fichero de firmas:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/e3c42c8c60f7687caeed11d595897cf632a66685/Hacking_%C3%89tico/dig5.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/e3c42c8c60f7687caeed11d595897cf632a66685/Hacking_%C3%89tico/dig6.png)

## _CAPTURA DE FICHEROS DE LOG_

