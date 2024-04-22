# CARRERA DE ESPECIALISTA EN HACKING ÉTICO

# PRIMER TALLER : ANÁLISIS FORENSE BÁSICO EN SISTEMAS WINDOWS (Tiempo de realización: 6-7 dias aproximadamente)

## Dia 14/12/2023

Para estos dos primeros talleres, opto por empezar creando una máquina virtual Windows 10 en Virtual Box. Con la máquina ya creada y configurada con las Guest adittions, configuración de red y demás, empiezo por el primer punto del primer curso:

## _COOKIES_

Las cookies son ficheros de texto que almacenan información. Suelen utilizarse normalmente para recordar accesos a páginas web(la web identifica tu ordenador,por lo tanto, si vuelves a entrar en ellas sabrán quien eres y que has echo anteriormente) y conocer hábitos de navegación.En el caso de un forense de Windows, lo que interesa es encontrar esos archivos en el sistema (mas adelante veremos algunos comandos para encontearlos en el sistema) y dentro de ellos, analizar y comprobar si se ha filtrado información o documentación.

Para todo ellos, con la máquina virtual corriendo, lanzamos una consola **con permisos de administrador**. Nos colocamos en la raiz de C: y escribimos el siguiente comando:

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

# SEGUNDO TALLER : ANÁLISIS FORENSE AVANZADO EN SISTEMAS WINDOWS (Tiempo de realización: 6-7 dias aproximadamente)

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

## Dia 11/03/2024

Volatility es un framework de código abierto, completamente programado en Python y que tiene como función el análisis forense de la memoria volátil (memoria RAM). Como se verá en el video de la instalación, va a ser necesaria la instalación de Python, asi como algunos ([paquetes complementarios (https://github.com/volatilityfoundation/volatility/wiki/installation)) que nos dicen en el github oficial y un compilador.

Con esta herramienta vamos a analizar en volcado de memoria que hicimos con la herramienta **Dumpit** en el primer curso. Para ello tendremos que generar u obtener previamente un archivo .raw y situarlo por ejemplo en nuestra carpeta de herramientas. A continuación, descargaremos la herramienta **Volatility** ((Video Tutorial de la descarga del Framework Volatility)[https://www.youtube.com/watch?v=iU9mqB4h3Tg&t=304]).Cuando tengamos completamente instalado Volatility, con todos sus paquetes y por supuesto Python, iremos a la carpeta en donde tengamos la carpeta de volatility y la imagen .raw y ejecutamos el siguiente comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/08ece591b5ef474ad26694494f5e3330db61367d/Hacking_%C3%89tico/vol1.png)

En donde "vol.py" es un script de python que es la interfaz principal para utilizar Volatility en la linea de comandos, "imageinfo" es un plugin de Volatility que se utiliza para obtener información sobre la imagen de la memoria, "-f" se utiliza para especificar que a continuación se escribirá un archivo y por último, como acabo de decir, el propio archivo que será el ".raw".El comando en si lo que hace es analizar el volcado de memoria que le damos y muestra información general, como la versión, arquitectura,etc. El resultado del siguiente comando es el siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/08ece591b5ef474ad26694494f5e3330db61367d/Hacking_%C3%89tico/vol2.png)

En el apartado "Suggested Profile(s)" nos dice que hay un perfil aconsejado; que es el **Win10x64_19041**. Este perfil lo guardaremos porque nos hará falta en futuros comandos. Si nos dijera que no hay ningún perfil aconsejado, entoces tendríamos que ver los perfiles que hay y ver cual es el que más nos interesa. Para ello vamos a escribir el siguiente comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/08ece591b5ef474ad26694494f5e3330db61367d/Hacking_%C3%89tico/vol3.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/08ece591b5ef474ad26694494f5e3330db61367d/Hacking_%C3%89tico/vol4.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/08ece591b5ef474ad26694494f5e3330db61367d/Hacking_%C3%89tico/vol5.png)

En donde podemos ver que nos da una lista (incompleta porque no cabian todos en pantalla) de los perfiles que hay disponibles, a parte de mas información que ahora mismo no nos interesa.

Ahora vamos a ver unos cuantos comandos en volatility para extraer información de nuestra imagen .raw. Lo primero que vamos a extraer, son los procesos en ejecución en el momento que se hizo la captura de memoria. Ejecutariamos el siguiente comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/08ece591b5ef474ad26694494f5e3330db61367d/Hacking_%C3%89tico/vol6.png)

En donde:
1. 'vol.py' -> Como se mencionó anteriormente, es un script de python que vamos a utilizar en todos nuestros comandos.
2. '-f (nombre archivo .raw)' -> Hace referencia al volcado de la memoria (archivo .raw).
3. '--profile==Win10x64_19041' -> Le indica el perfil del sistema operativo utilizado en el volcado de memoria.
4. 'pslist' -> Comando específico que le damos a volatility para ejecutar y que en este caso significa que nos liste todos los procesos en ejecución en el momento del volcado de memoria (nos dará nombre, PID,hilos...).

La respuesta a este comando es la siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/08ece591b5ef474ad26694494f5e3330db61367d/Hacking_%C3%89tico/vol7.png)

En los comandos posteriores, será todo igual a excepción evidentemente del comando específico que queremos ejecutar. Dicho esto, lo siguiente que vamos a ver es la lista de servicios en ejecución en el momento del volcado. Ejecutaremos el siguiente comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/08ece591b5ef474ad26694494f5e3330db61367d/Hacking_%C3%89tico/vol8.png)

En donde vemos que el comando específico es 'psscan'. La salida es la siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/08ece591b5ef474ad26694494f5e3330db61367d/Hacking_%C3%89tico/vol9.png)

Podemos observar que nos dan todos los servicios en ejecución en el momento del volcado, con su nombre, PID y la fecha y hora en la que se crearon, entre demás información.

Lo siguiente que vamos a ver, son las ((DLL)[https://programarfacil.com/blog/dll-que-son-y-para-que-sirven/]) que carga cada proceso en el momento del volcado de memoria. Ejecutaremos el siguiente comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/222f1e29781f46f3edda73b7d3cea53aa272d713/Hacking_%C3%89tico/vol10.png)

En donde vemos que el comando específico a ejecutar es 'dlllist'. La respuesta es la siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/222f1e29781f46f3edda73b7d3cea53aa272d713/Hacking_%C3%89tico/vol11.png)

![]()

En donde podemos ver dos procesos diferentes con algunas de sus 'dll'. Ahora vamos a ver el mismo comando pero filtrando las dll de un proceso en específico. Lo único que hay que hacer es añadirle al comando anterior un PID de un proceso después del comando dlllist:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/222f1e29781f46f3edda73b7d3cea53aa272d713/Hacking_%C3%89tico/vol12.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/222f1e29781f46f3edda73b7d3cea53aa272d713/Hacking_%C3%89tico/vol13.png)

Por último, vamos a ver los servicios que estaban en ejecución en el sistema en el momento del volcado de memoria. Ejecutamos el siguiente comando:

![]((https://github.com/emiliogf10/Proyecto-FDC/blob/222f1e29781f46f3edda73b7d3cea53aa272d713/Hacking_%C3%89tico/vol15.png))

En donde vemos que le añadimos el comando específico 'svcscan'. La salida es la siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/222f1e29781f46f3edda73b7d3cea53aa272d713/Hacking_%C3%89tico/vol15.png)



# TERCER TALLER : ANÁLISIS FORENSE BÁSICO EN SISTEMAS LINUX (Tiempo de realización: 6-7 dias aproximadamente)

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

1. auth.log -> log de autenticación.
2. boot.log -> Registro de inicio del sistema (muy importante tener una copia desde el primer arranque del sistema).
3. kern.log -> Registro de kernel.
4. dpkg.log -> Este archivo registra las acciones del comando 'dpkg', que básicamente lo que hace es instalar,actualizar o eliminar paquetes de nuestro sistema.

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

A parte de capturar la lista de ficheros de log que hay, deberiamos guardar una copia de los más importantes y guardarlos en nuestra carpeta de evidencias. En mi caso voy a guardar los archivos "auth.log","boot.log","dpkg.log" y "kern.log". Para ello, ejecutamos los siguientes comandos:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f7042be41fa5c71e273b89100f51e9f8172312d3/Hacking_%C3%89tico/reg8.png)

Y vemos que los archivos se guardaron correctamente. Ahora los firmariamos y meteriamos el HASH en nuestro archivo de firmas:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f7042be41fa5c71e273b89100f51e9f8172312d3/Hacking_%C3%89tico/reg9.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f7042be41fa5c71e273b89100f51e9f8172312d3/Hacking_%C3%89tico/reg10.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f7042be41fa5c71e273b89100f51e9f8172312d3/Hacking_%C3%89tico/reg11.png)


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

## Dia 19/01/2024

# CUARTO TALLER : ANÁLISIS FORENSE AVANZADO EN SISTEMAS LINUX (Tiempo de realización: 6-7 dias aproximadamente)

## _VOLCADO DE MEMORIA_

En este apartado vamos a realizar un volcado de la memoria del sistema. Para ello, voy a seguir con la misma máquina virtual del curso anterior. Lo primero que vamos a hacer es instalar git (lo utilizaremos para descargar desde un repositorio de github la herramienta Lime). Para ello hacemos lo siguiente, preferiblemente **como root**:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f7042be41fa5c71e273b89100f51e9f8172312d3/Hacking_%C3%89tico/volcado1.png)

Después, clonaremos un repositorio de github en la raíz de root:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f7042be41fa5c71e273b89100f51e9f8172312d3/Hacking_%C3%89tico/volcado2.png)

En donde se nos creará una carpeta llamada 'Lime':

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f7042be41fa5c71e273b89100f51e9f8172312d3/Hacking_%C3%89tico/volcado3.png)

Para utilizar esta herramienta, vamos a necesitar la versión de kernel que estamos usando, así como unos paquetes. La versión de kernel la variamos mediante el siguiente comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f7042be41fa5c71e273b89100f51e9f8172312d3/Hacking_%C3%89tico/volcado4.png)

Es importante quedarnos con la versión de kernel, porque la necesitaremos en el paso siguiente. A continuación, instalaremos los paquetes que son necesarios para la ejecución de la herramienta:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f7042be41fa5c71e273b89100f51e9f8172312d3/Hacking_%C3%89tico/volcado5.png)

En donde vemos que al final, ponemos la versión de nuestro kernel; ya que si instalamos otros paquetes, pueden no ser compatibles con nuestro kernel, y por lo tanto para nuestro sistema. Hecho esto, nos dirigimos a la carpeta '/src/' para crear un módulo, que no es más que una unidad de código que se carga dinámicamente en el kernel para permitir la extracción forense de información de la memoria RAM.

## Dia 29/01/2024

Lo siguiente es crear un "kernel object", que no es más que un módulo que utilizaremos posteriormente para el volcado de la memoria. Para ello, nos vamos a la carpeta "Lime/src" y ejecutamos el comando "make". Se vería así:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/cc63286dd3549c3e4b444013be1f60418198b5ad/Hacking_%C3%89tico/volcado6.png)

Y vemos que se creó un archivo con extensión ".ko":

![](https://github.com/emiliogf10/Proyecto-FDC/blob/cc63286dd3549c3e4b444013be1f60418198b5ad/Hacking_%C3%89tico/volcado7.png)

Con todos estos pasos hechos, estamos preparados para el volcado de memoria. Para ello ejecutaremos el siguiente comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/cc63286dd3549c3e4b444013be1f60418198b5ad/Hacking_%C3%89tico/volcado8.png)

En donde:

1. "sudo insmod" -> Ejecuta con privilegios de superusuario el comando insmod, que lo que hace es cargar el módulo de kernel que se le pasa con los parámetros correspondientes.
2. "lime-6.2.0-39-generic.ko" -> Es el módulo que se le pasa al comando insmod.
3. "path=/home/emilio/Desktop/evidencias/volcado_memoria format=raw" -> Aqui, le indicamos la ruta en donde queremos que nos guarde el volcado, con su nombre y su formato.

Y vemos que se creó de forma correcta el archivo:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/cc63286dd3549c3e4b444013be1f60418198b5ad/Hacking_%C3%89tico/volcado9.png)

Podemos ver también, que el tamaño del volcado es de 2GB, al igual que la cantidad de memoria RAM que tenemos en nuestra máquina virtual:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/cc63286dd3549c3e4b444013be1f60418198b5ad/Hacking_%C3%89tico/volcado10.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/cc63286dd3549c3e4b444013be1f60418198b5ad/Hacking_%C3%89tico/volcado11.png)

## _VERIFICACIÓN DEL KERNEL Y DE LOS PROCESOS DE INICIALIZACIÓN_

En este apartado, simplemente vamos a comprobar dos archivos: el HASH del kernel (este archivo debería crearse en cuanto se inicia un nuevo equipo) y el HASH del kernel en el momento de la intrusión (archivo que se va a generar como primer paso cuando tengamos algún tipo de problema y tengamos que verificar si el kernel del sistema fue manipulado). En este caso, voy a hacerlos los dos al mismo tiempo, pero evidentemente con distinto nombre:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/cc63286dd3549c3e4b444013be1f60418198b5ad/Hacking_%C3%89tico/ker1.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/cc63286dd3549c3e4b444013be1f60418198b5ad/Hacking_%C3%89tico/ker2.png)

En donde simplemente firmamos el kernel del sistema (OJO con poner la versión de kernel adecuada, porque pueden aparecer varios archivos con versiones distintas) y redireccionamos su HASH a un archivo de texto ubicado en nuestra carpeta de evidencias. Ahora vamos a compararlos:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/cc63286dd3549c3e4b444013be1f60418198b5ad/Hacking_%C3%89tico/ker3.png)

Vemos que no nos dice nada, y es evidente porque es lo mismo pero con diferente nombre. Ahora bien, esto en una situación real puede cambiar; vamos a simular que el kernel fue modificado y ver que la comparación nos va a dar alguna diferencia. Para ello modificamos el segundo archivo, por ejemplo borrándole alguna letra de su HASH y vemos que ya nos salta una diferencia:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/cc63286dd3549c3e4b444013be1f60418198b5ad/Hacking_%C3%89tico/ker4.png)

Ahora vamos a hacer exactamente lo mismo, pero para el archivo "dpkg.log" que tenemos en nuestra carpeta de evidencias. Para ello, haremos una copia de dicho archivo que simulará la copia creada después del problema. Después cambiaremos algo de su contenido y los compararemos:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/cc63286dd3549c3e4b444013be1f60418198b5ad/Hacking_%C3%89tico/ker5.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/cc63286dd3549c3e4b444013be1f60418198b5ad/Hacking_%C3%89tico/ker6.png)

Añadir que se podrían comparar los achivos en "crudo" como lo estamos haciendo ahora mismo, o comparar sus HASH, siempre y cuando hubieramos firmado el primer archivo (el que se hace al iniciar un nuevo equipo).

Para comparar, también podemos utilizar el comando "diff", que es un comando muy rápido. Veamos un ejemplo con los archivos dpkg:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/cc63286dd3549c3e4b444013be1f60418198b5ad/Hacking_%C3%89tico/ker7.png)

A mi, en especial me gusta más este último comando, por una sencilla razón: te muestra el número de linea en la que está el cambio y el cambio realizado con el antes y el despúes.

## Dia 05/02/2024

## _FICHERO LASTLOG_

En este apartado vamos a ver qué usuarios se han logueado en el sistema (mediante acceso remoto). Para ello, veremos un fichero llamado "lastlog".

Abrimos una consola de comandos, y simplemente escribimos "lastlog" y le damos a enter. Nos saldrá lo siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/5effafa87137992695a94dd516964f37f7d7baf2/Hacking_%C3%89tico/lastlog1.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/5effafa87137992695a94dd516964f37f7d7baf2/Hacking_%C3%89tico/lastlog2.png)

En donde vemos que nos dice que ningún usuario se ha logueado; y es normal, porque yo en esta máquina estoy logueado de forma gráfica.

## _CAMBIOS EN EL NIVEL DE EJECUCIÓN_

En este apartado vamos a ver en qué "runlevel" (nivel de ejecución) está nuestra máquina. Los "runlevels" no son más que estados específicos del sistema que determinan qué servicios y procesos están activos en un momento dado, en donde 0 es el apagado del sistema y 6 es el reinicio del sistema. Para más información de estos estados, ([pulsa aquí](https://es.wikipedia.org/wiki/Nivel_de_ejecución)).

Para ello vamos a ejecutar el comando "runlevel" en nuestra consola:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/08fff0c9831a96de9fa04d5a8885aa71158c2f8f/Hacking_%C3%89tico/runlevel1.png)

vemos que nuestra máquina está en el runlevel 5; lo que significa que está como multiusuario gráfico. En la siguiente foto, al igual que en el enlace anterior, se pueden ver los distintos niveles:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/08fff0c9831a96de9fa04d5a8885aa71158c2f8f/Hacking_%C3%89tico/runlevel2.png)

Es importante revisar en qué "runlevel" estamos porque nos da mucha información sobre la configuración actual de nuestro sistema.

## _ELEVACIÓN DE PRIVILEGIOS_

En este apartado, vamos a ver el fichero guardado en apartados anteriores "auth.log". Como ya lo tenemos guardado en nuestra carpeta de evidencias y en principio el fichero se comprobaría en ese mismo momento, podriamos utilizar ese, pero hay que tener en cuenta una cosa muy importante; **la copia del fichero que tenemos guardada, no va a tener la misma información que el fichero original por el simple hecho de que después de la copia se ejecutaron más comandos con sudo y éstos se guardaron en el archivo original y por lo tanto la copia está desactualizada**. Dicho esto, ejecutaremos el siguiente comando sobre el fichero principal, no sobre la copia:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/08fff0c9831a96de9fa04d5a8885aa71158c2f8f/Hacking_%C3%89tico/privilegios1.png)

En donde:

1. "sudo cat /var/log/auth.log" -> Simplemente leemos el archivo que le indicamos en la ruta con permisos de       superusuario.
2. "| grep sudo" -> Utilizamos el comando grep para filtrar, en este caso queremos encontrar la cadena "sud".

La salida es la siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/08fff0c9831a96de9fa04d5a8885aa71158c2f8f/Hacking_%C3%89tico/privilegios2.png)

En donde podemos ver que nos aparece la fecha, hora, usuario y el comando que ejecutamos. Una curiosidad que podemos ver es que cada vez que ejecutamos un comando con sudo, se abre una sesión de root y después se cierra.

En este fichero, básicamente lo que tenemos que comprobar es que en la fecha de la intrusión o ataque no se ejecutaron comandos con permisos de superusuario ejecutando scripts, no se cambiaron los permisos de algún fichero, no se intentó copiar algún archivo del sistema o incluso que no se ejecutó un comando que nos parezca extraño.

Es importante decir que en servidores más grandes como servidores de empresas, sería interesante tener una tarea programada en la que se copia el archivo "auth.log" periódicamente (por ejemplo cada hora) a un archivo oculto y cifrado que sólo sepan los administradores del sistema, ya que en un análisis forense, se va a necesitar este fichero lo más íntegro posible (quien tenga conocimientos suficientes sobre sistemas, podrá manipular dicho archivo). En este caso estamos hablando del archivo "auth.log", pero **lo ideal sería guardar una copia de todos los archivos de "/var/log/" en una carpeta oculta y programar dicha tarea para todos**.

## _PAQUETES MODIFICADOS_

En este apartado vamos a comprobar también un archivo que ya tenemos guardado en nuestra carpeta eviencias, que es el "dpkg.log". Vamos a ejecutar el siguiente comando desde la carpeta de usuario:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/08fff0c9831a96de9fa04d5a8885aa71158c2f8f/Hacking_%C3%89tico/modif0.png)

En donde:

1. "cat /var/log/dpkg.log" -> Leemos el archivo de la ruta especificada.
2. "| grep install" -> Utilizamos el comando grep para filtrar, en este caso queremos encontrar la cadena "install".

La salida es la siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/08fff0c9831a96de9fa04d5a8885aa71158c2f8f/Hacking_%C3%89tico/modif1.png)

Vemos que nos aparece la fecha, hora y el paquete que este caso se instaló. Hacemos exactamente lo mismo con el upgrade:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/08fff0c9831a96de9fa04d5a8885aa71158c2f8f/Hacking_%C3%89tico/modif2.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/08fff0c9831a96de9fa04d5a8885aa71158c2f8f/Hacking_%C3%89tico/modif3.png)

En este apartado comprobaremos qué paquetes se han eliminado, instalado o modificado. En mi caso, con el "delete" habría que hacer exactamente lo mismo que el "install" y el "upgrade", pero en mi máquina no hay paquetes eliminados en este momento.

## _BÚSQUEDA DE ARCHIVOS GRANDES_

En este apartado vamos a buscar en nuestro sistema archivos que tengan un tamaño grande, en mi caso voy a poner 1GB como mínimo:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/08fff0c9831a96de9fa04d5a8885aa71158c2f8f/Hacking_%C3%89tico/aGrandes1.png)

En donde:

1. "find" -> Es el comando utilizado para buscar archivos y directorios.
2. "." -> El punto representa el directorio actual y por lo tanto desde donde se empezará la búsqueda.
3. "-size +1000M" -> Especifica el criterio de búsqueda basado en el tamaño. En este caso, se buscan archivos con un tamaño superior a 1000 megabytes.

La salida es la siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/08fff0c9831a96de9fa04d5a8885aa71158c2f8f/Hacking_%C3%89tico/aGrandes2.png)

En donde vemos que nos encuentra el archivo de volcado de memoria que hicimos que apartados anteriores (el volcado era de 2GB, al igual que la RAM de la máquina virtual).

La búsqueda se podría ampliar a todo el sistema, cambiando el "." por el "/" que es el directorio raíz. Evidentemente también podemos cambiar los tamaños de archivo según nuestro criterio.

## Dia 09/03/2024

BÚSQUEDA DE INFORMACIÓN DE VOLATILITY EN WINDOWS Y LINUX (EL VOLCADO DE MEMORIA EN WINDOWS NO ME SUGIERE UN PERFIL PARA PODER EJECUTAR SUS COMANDOS, Y TENGO QUE BUSCAR POR QÚE).

## Dia 12/03/2024

## _USO DE VOLATILITY_

Como bien se dijo en el curso de Windows, Volatility es un framework de código abierto  completamente programado en Python que tiene como función el análisis forense de la memoria volátil (memoria RAM del sistema).

En este apartado, vamos a utilizar esta herramienta para analizar el volcado de memoria que hicimos en el primer apartado de este curso. Para ello, tendremos que instalar una serie de paquetes en nuestra máquina virtual: ((Video tutorial de la instalación de Volatility en Linux)[https://www.youtube.com/watch?v=5-2-ORNC8CA]).

Para empezar a extraer información, debemos averiguar cuál es el perfil de la captura de memoria volátil que se va a analizar. Para ello, situamos el volcado dentro de la  carpeta de volatility y ejecutamos el siguiente comando:

![]()

En donde podemos ver que que hacemos el comando imageinfo del volcado de memoria que ya teniamos. El resultado es el siguiente:

![]()

## Dia 14/03/2024

# PRIMER CURSO : CURSO DE OSINT (Tiempo de realización: 10 dias aproximadamente)

## _OSINT_

Antes de nada, vamos a hacer un resumen de lo que va a ser este apartado y vamos a ver unos cuantos conceptos clave.

**¿QUE ES OSINT?**

Las siglas OSINT (Open Source Intelligence) se refiere a la recopilación, análisis y utilización de información que se encuentra disponible públicamente.

Conceptos clave:

1. 'Información' ->Se refiere a datos o hechos que han sido recopilados, almacenados o transmitidos de alguna manera.
2. 'Inteligencia' -> Es el resultado del procesamiento, análisis y evaluación de la información para generar conocimiento útil y accionable. La finalidad es la toma de decisiones. Este producto llamado inteligencia, se da después de un **proceso cíclico** que mencionaremos más adelante.
3. 'Ciberinteligencia' -> Aplicar la inteligencia en el ciberespacio. Abarca el ámbito tradicional y tecnológico.
4. 'Fuente abierta' -> se refiere a cualquier recurso o conjunto de datos que esté disponible públicamente y no requiera acceso restringido o privilegiado para ser consultado, ya sea en papel, fotográfico, sonoro, etc. Ejemplos de fuentes abiertas son una enciclopedia, una rueda de prensa, la radio, un blog o una retransmisión de TV.

Ahora vamos a ver otras tres disciplinas dentro de la inteligencia (este curso se centra en la disciplina OSINT), aunque existen más:

1. 'HUMINT' -> Por sus siglas en inglés (Human Intelligence), es la información de fuentes humanas.
2. 'SOCMINT' -> Por sus siglas en inglés (Social Media Intelligence), es la inteligencia en las redes sociales.
3. 'CYBINT' -> Por sus siglas en inglés (Cyber Intelligence), es la inteligencia en el ciberespacio.

## Dia 15/03/2024

**CICLO DE INTELIGENCIA**

Ahora vamos a ver algo que mencioné antes; y es el ciclo de inteligencia.

![](https://github.com/emiliogf10/Proyecto-FDC/blob/eeba410f867d36c46c35e24290ad358600072cde/Hacking_%C3%89tico/osint1.png)

Podemos ver que se divide en 6 fases:

1. 'Requisitos' ->Toma de datos con el cliente, recibir toda la información que nos interese, saber cuál es el objetivo de nuestra investigación, saber las necesidades del cliente,etc.
2. 'Fuentes de información' -> Saber con qué fuentes vamos a trabajar o consultar.
3. 'Adquisición' -> Obtendremos la información que nos interese.
4. 'Procesamiento' -> Le daremos un formato a la información que tenemos.
5. 'Análisis' -> Analizamos la información que tenemos.
6. 'Inteligencia' -> Producción de inteligencia. Elaboraremos un informe con nuestras conclusiones, perspectivas, etc. Al final de todo, lo que se saca es un producto==inteligencia y su finalidad, es la toma de decisiones.

Este ciclo tiene varias problemáticas; la primera es el exceso de información (hay gran cantidad de fuentes y herramientas, lo que nos lleva a un sobreexceso de información), la segunda es la fiabilidad de las fuentes (es necesario clasificar las fuentes y saber si son fiables) y como última problemática tenemos la información errónea o malintencionada (tenemos que comprobar que la información y saber que está contrastada).

**CREDIBILIDAD VS FIABILIDAD**

En la elección de las fuentes de información, es recomendado utilizar el Sistema Internacional de Fuentes; que es básicamente un sistema para clasificar las fuentes de información. En el SIF, la fiabilidad de la fuente se clasifica de la A-F y la credibilidad del contenido del 1-6. Aqui dejo una muestra de como sería:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/eeba410f867d36c46c35e24290ad358600072cde/Hacking_%C3%89tico/osint2.png)

**FACTOR HUMANO**

En este punto vamos a resaltar la figura del analista. Hay que tener muy claro que ninguna herramienta va a suplir a un analista humano; ya sea porque las herramientas no nos puedan dar unas conclusiones, no puedan descartar falsos positivos, no van a saber qué es relevante y no, no van a poder darnos una perspectiva, etc.

## _HACKING CON BUSCADORES_

**GOOGLE HACKING**

Para empezar este punto es necesario saber que hay diferentes metabuscadores (Un metabuscador consiste en un sistema de búsqueda que nos ofrece una amplia muestra sobre la información que buscamos a través de diferentes motores de búsqueda existentes, tipo Google,Bing,Yahoo...); centrados en una temática o de una zona geográfica concreta. Todos ellos tienen como límite la información pública y usan diferentes operadores de búsqueda.

El buscador más utilizado de todos es **Google** y vamos a ver su búsqueda avanzada, vamos a crear nuestro propio motor de búsqueda y vamos a hablar de la Google Hacking Database.

Vamos a empezar por ver los principales Dorks de Google (símbolos que especifican una consición). A continuación dejo una tabla con los más importantes:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/eeba410f867d36c46c35e24290ad358600072cde/Hacking_%C3%89tico/h1.png)

Vemos que tenemos bastantes dorks para una búsqueda de Google; que serán muy importantes para obtener información específica sobre un tema. A continuación os dejo una breve descripción de cada uno:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/30737cd6d7de06f79c049b232b0d4dc66d0d235d/Hacking_%C3%89tico/h4.png)

Si nos vamos a Google y seleccionamos la opción de la esquina inferior derecha 'Configuración', podremos acceder a la búsqueda avanzada:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/eeba410f867d36c46c35e24290ad358600072cde/Hacking_%C3%89tico/h2.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/eeba410f867d36c46c35e24290ad358600072cde/Hacking_%C3%89tico/h3.png)

En esta página, se van a aplicar internamente los dorks que vimos anteriormente; pero mucho más fácil de entender para un usuario normal.

## Dia 16/03/2024

Ahora vamos a ver como crear un motor de búsqueda personalizado. Simplemente ([pulsa aquí para crear uno nuevo](https://programmablesearchengine.google.com/controlpanel/create)). Nos aparecerá la siguiente ventana:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/30737cd6d7de06f79c049b232b0d4dc66d0d235d/Hacking_%C3%89tico/h5.png)

El motor de búsqueda que crearemos funcionará de la siguiente manera; primero le asignaremos un nombre, que en mi caso va a ser 'Noticias' y en el apartado de 'Sitios web en los que buscar' añadiremos todos los dominios en donde queremos que se busquen nuestras consultas (en mi caso voy a añadir La voz de Galicia, El mundo y El pais):

![](https://github.com/emiliogf10/Proyecto-FDC/blob/30737cd6d7de06f79c049b232b0d4dc66d0d235d/Hacking_%C3%89tico/h6.png)

Después podemos habilitar la opción de buscar en toda la web en vez de buscar en los domios específicos que hemos especificado y también podemos habilitar la búsqueda por imágenes y la búsqueda segura:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/30737cd6d7de06f79c049b232b0d4dc66d0d235d/Hacking_%C3%89tico/h7.png)

Le damos a crear y listo:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/30737cd6d7de06f79c049b232b0d4dc66d0d235d/Hacking_%C3%89tico/h8.png)

Aqui, tendríamos que darle a personalizar y nos aparecerían un montón de propiedades para cambiarle como la región en donde queremos que nos busque, los sitios que queremos que nos excluya (en caso de buscar en toda la web) o el idioma. A nosotros lo que nos interesa es la URL pública para poder utilizarlo:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/30737cd6d7de06f79c049b232b0d4dc66d0d235d/Hacking_%C3%89tico/h9.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/30737cd6d7de06f79c049b232b0d4dc66d0d235d/Hacking_%C3%89tico/h10.png)

Si le damos click al enlace vemos que nos redirige a nuestro buscador sencillo. Si buscamos, por ejemplo 'madrid' nos aparecerá lo siguiente (evidentemente aparecerán también anuncios relacionados):

![](https://github.com/emiliogf10/Proyecto-FDC/blob/30737cd6d7de06f79c049b232b0d4dc66d0d235d/Hacking_%C3%89tico/h11.png)

Nos aparecerán, en este caso, noticias relacionadas con la palabra 'madrid' sólo en 'El pais', 'El mundo' y 'La voz de Galicia'.

Por último en este apartado, vamos a mencionar la 'Google Hacking Database', que no es más que una base de datos de los 'Dorks' mencionados anteriormente que la gente sube:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/30737cd6d7de06f79c049b232b0d4dc66d0d235d/Hacking_%C3%89tico/h12.png)

En la barra de búsqueda podemos buscar los 'Dorks' por el término de búsqueda o abajo de todo irnos a las categorías:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/30737cd6d7de06f79c049b232b0d4dc66d0d235d/Hacking_%C3%89tico/h13.png)

Todos estos parámetros de búsqueda nos servirán para ampliar al máximo y especificar nuestra búsqueda de información en fuentes abiertas.

**DORKS EN BUSCADORES ANALISTAS**

En este aprtado vamos a ver los 'Dorks' más importantes en algunos navegadores generalistas. Empezaremos con el navegador Bing:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/30737cd6d7de06f79c049b232b0d4dc66d0d235d/Hacking_%C3%89tico/d1.png)

El siguiente navegador es DuckDuckGo que comparte los operadores habituales pero añade los ([Bangs](https://duckduckgo.com/bangs)), que no son más que atajos de teclado para hacer nuestra búsqueda más completa.

Los últimos son Yandex y Baidu (navegador por región ruso y chino respectivamente). Ambos comparten los operadores habituales.

![](https://github.com/emiliogf10/Proyecto-FDC/blob/30737cd6d7de06f79c049b232b0d4dc66d0d235d/Hacking_%C3%89tico/d2.png)

([Pincha en este enlace para ver los Dorks en estos navegadores]([https://drive.google.com/file/d/1GIfRKE0ctkOoqnc2lwGzYu5rh88T4hs8/view))

**BUSQUEDA INVERSA DE IMAGENES**

En este aprtado vamos a ver unos buscadores que se emplean para la búsqueda inversa de imágenes; y su utilidad es reconocer las noticias falsas, bulos, suplantaciones de marcas, etc.

Los buscadores tradicionales son Google Imágenes y Yandex Imágenes. Un buscador especializado es TinEye (se centra en el color y texto de la imagen).

Si, por ejemplo, buscamos el logo de OpenWebinars en Google y Yandex, nos van a aparecer resultados e imagenes similares; como las redes sociales, páginas en donde aparezca la imagen o incluso foros:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/30737cd6d7de06f79c049b232b0d4dc66d0d235d/Hacking_%C3%89tico/i1.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/30737cd6d7de06f79c049b232b0d4dc66d0d235d/Hacking_%C3%89tico/i2.png)

Finalmente, tenemos TinEye, que buscará en profundidad el logo exacto y nos dirá en donde se muestra:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/30737cd6d7de06f79c049b232b0d4dc66d0d235d/Hacking_%C3%89tico/i3.png)

**BUSCADORES TECNOLOGICOS**

En este aparatdo vamos a ver buscadores especializados en tecnología. El más usado es [Shodan](https://www.shodan.io).

[Pincha aqui para obtener más información sobre Shodan](https://www.xataka.com/basics/shodan-que-se-puede-usar-este-buscador-dispositivos-conectados-a-internet).

En este navegador también existen Dorks; para filtrar por ciudad, país, dominio, SO, título, etc. Los principales los vamos a encontrar en el fichero del apartado anterior: [pincha aquí para ver el documento](https://drive.google.com/file/d/1GIfRKE0ctkOoqnc2lwGzYu5rh88T4hs8/view).

Ahora, vamos a poner un ejemplo de todo esto. Voy a buscar servidores apache en la ciudad de Madrid (posiblemente requiera un registro previo totalmente gratuito):

![](https://github.com/emiliogf10/Proyecto-FDC/blob/8ba3041f90be8717dd2bb7dbdfacb7c157ea9e76/Hacking_%C3%89tico/bt1.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/8ba3041f90be8717dd2bb7dbdfacb7c157ea9e76/Hacking_%C3%89tico/bt2.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/8ba3041f90be8717dd2bb7dbdfacb7c157ea9e76/Hacking_%C3%89tico/bt3.png)

Podemos ver que en la consulta ponemos primero el tipo de servidor que queremos buscar y despues ponemos la ciudad con el Dork city:Madrid. Podemos observar que nos aparecen 81743 banners (datos que el servidor le devuelve al cliente) de servidores apache en Madrid, con sus IP, nombres de las empresas,si tienen certificado SSL, etc.

Otros buscadores tecnológicos son:
1. [Zoomeye](https://www.zoomeye.org) -> Es un buscador chino, que necesita registro previo y es muy similar al Shodan.
2. [Mr.Looquer](https://mrlooquer.com) -> Está desarrollado por expertos en ciberseguridad españoles, centrado en IPV6, uso de [wildcards](https://apunteimpensado.com/wildcards-que-son-para-que-sirven/) y expresiones regulares (esta opción sólo si tienes cuenta premium).

**BUSCADORES EN DEEP&DARK WEB**

En este apartado vamos a tratar la búsqueda en buscadores de Deep y Dark web. A continuación dejo una imagen que define lo que es cada concepto (los porcentajes son incorrectos; debido a que no se puede saber lo grande que es la Deep y Dark web, incluso la surface web):

![](https://github.com/emiliogf10/Proyecto-FDC/blob/8ba3041f90be8717dd2bb7dbdfacb7c157ea9e76/Hacking_%C3%89tico/dweb1.png)

## Dia 18/03/2024

Antes de conocer a los buscadores más usados, mencionaremos al navegador [TOR](https://www.xataka.com/basics/red-tor-que-como-funciona-como-se-usa), que es un software diseñado para permitir a los usuarios navegar por internet de forma segura y anónima (evidentemente no al 100%; depende de cómo se configure y cómo se use el navegador TOR).

Los principales buscadores son:

1. AHMIA -> Tiene acceso mediante TOR O I2P (capa de abstracción) mediante este enlace : msydqstlz2kzerdg.onion. También tiene acceso mediante los navegadores comunes mediante [este enlace](https://ahmia.fi/).
2. TORCH -> Tiene acceso mediante TOR O I2P (capa de abstracción) mediante este enlace : xmh57jrzrnw6insl.onion. También tiene acceso mediante los navegadores comunes mediante [este enlace](http://www.torchtorsearch.com/). A diferencia de AHMIA, TORCH dispone de búsqueda avanzada y de medio millón de enlaces .onion.

Otros buscadores son [Onion Link](http://onion.link/), Not Evil, Grams o Candle. Los buscadores que no tienen enlace, es porque tienen enlaces .onion, por lo tanto es necesario instalar el navegador TOR para acceder a ellos.

Es importante mencionar también, un repositorio en donde hay muchos enlaces .onion de diferentes temas. Vamos a ver la Hiden Wiki y para eso descargamos el navegador TOR en una máquina virtual y lo abrimos:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/643d856c05fc814092817f4413992921b9463562/Hacking_%C3%89tico/tor1.png)

Vemos que es como un navegador normal. Activamos la opción de onionizar y pegamos la ruta "zqktlwi4fecvo6ri.onion":

![](https://github.com/emiliogf10/Proyecto-FDC/blob/643d856c05fc814092817f4413992921b9463562/Hacking_%C3%89tico/tor2.png)

Vemos que el resultado nos lleva como a una 'wikipwdia' llamada The Hidden Wiki. También podemos comprobar que nuestra conexión salta por 3 nodos de 3 países diferentes para enmascarar nuestra identidad. Entramos en la Hidden Wiki y nos aparece lo siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/643d856c05fc814092817f4413992921b9463562/Hacking_%C3%89tico/tor3.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/643d856c05fc814092817f4413992921b9463562/Hacking_%C3%89tico/tor4.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/643d856c05fc814092817f4413992921b9463562/Hacking_%C3%89tico/tor5.png)

Como podemos apreciar es un repositorio en donde aparecen infinidad de enlaces ordenados por temas. Otro repositorio de este tipo es TorLinks:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/643d856c05fc814092817f4413992921b9463562/Hacking_%C3%89tico/tor6.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/643d856c05fc814092817f4413992921b9463562/Hacking_%C3%89tico/tor7.png)

En donde vemos que entro otros enlaces, aparecen los buscadores antes mencionados con una breve descripción.

**RETO: HACKING CON BUSCADORES**

En este apartado vamos a resolver un reto en el que tenemos que comprobar si la información de un tweet es verídica.

![](https://github.com/emiliogf10/Proyecto-FDC/blob/643d856c05fc814092817f4413992921b9463562/Hacking_%C3%89tico/reto1.png)

El tuit : https://twitter.com/PabloPardo1/status/1008923855954567170

Solución: Dado que el profesor no presenta ninguna solución, yo propongo la mia.

Utilizando la búsqueda inversa de imágenes, guardo la imagen del tweet en mi PC y utilizo Google Imágenes, Yandex y TinEye para buscar coincidencias:

Google Imágenes:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/643d856c05fc814092817f4413992921b9463562/Hacking_%C3%89tico/reto2.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/643d856c05fc814092817f4413992921b9463562/Hacking_%C3%89tico/reto3.png)

Yandex:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/643d856c05fc814092817f4413992921b9463562/Hacking_%C3%89tico/reto4.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/643d856c05fc814092817f4413992921b9463562/Hacking_%C3%89tico/reto5.png)

TinEye:

En TinEye apliqué el filtro de las más antiguas y los resultados son los siguientes:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/643d856c05fc814092817f4413992921b9463562/Hacking_%C3%89tico/reto6.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/643d856c05fc814092817f4413992921b9463562/Hacking_%C3%89tico/reto7.png)

Dados los resultados de la búsqueda, mi conclusión es que el tweet es una 'fake new' debido a que las noticas más antiguas con esa foto datan del año 2014, año en el que el presidente de EEUU era Barack Obama:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/643d856c05fc814092817f4413992921b9463562/Hacking_%C3%89tico/reto8.png)

[Noticia de 2014, El Pais](https://elpais.com/internacional/2014/06/21/album/1403368866_679675.html)
[Infobae](https://www.infobae.com/america/eeuu/2018/06/21/la-foto-de-2014-que-se-viralizo-como-una-supuesta-evidencia-de-la-actual-crisis-con-los-inmigrantes-en-la-frontera-de-eeuu-y-mexico/)
[Ok Diario](https://okdiario.com/internacional/utilizan-unas-imagenes-ninos-enjaulados-2014-bajo-mandato-obama-atacar-trump-2456031)
[Noticia DE 2014, The Objective](https://okdiario.com/internacional/utilizan-unas-imagenes-ninos-enjaulados-2014-bajo-mandato-obama-atacar-trump-2456031)

## _METADATOS_

Los [metadatos](https://www.powerdata.es/metadatos) no son más que datos que describen otros datos. Los metadatos están en todo tipos de ficheros y son dificilmente visibles. Un resumen un poco más completo sería el siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/643d856c05fc814092817f4413992921b9463562/Hacking_%C3%89tico/meta1.png)

Para poner un ejemplo, una fotografía puede contener metadatos de dimensión, si se ha activado el flash, gps, fecha de captura, modelo de cámara, etc.

Para ver esto, creamos un fichero o con uno que tengamos (en mi caso una presentación de powerpoint), damos click derecho y propiedades. En el apartado de detalles se nos muestra un montón de información:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/643d856c05fc814092817f4413992921b9463562/Hacking_%C3%89tico/meta2.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/643d856c05fc814092817f4413992921b9463562/Hacking_%C3%89tico/meta3.png)

Para eliminar esta información, le damos a la opción en azul de quitar propiedades e información personal y se nos abrirá otra ventana:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/643d856c05fc814092817f4413992921b9463562/Hacking_%C3%89tico/meta4.png)

En donde seleccionamos los datos que queremos eliminar o la opción de seleccionar todos (se eliminarán todos los que se puedan porque hay metadatos como la fecha de creación, tiempo de edición, último guardado, etc que no se pueden eliminar):

![](https://github.com/emiliogf10/Proyecto-FDC/blob/643d856c05fc814092817f4413992921b9463562/Hacking_%C3%89tico/meta5.png)

Y vemos que ya no aparecen.

**HERRAMIENTAS PARA LA EXTRACCIÓN DE METADATOS: ExifTool**

En esta lección vamos a hablar de una herramienta específica para la extracción de metadatos; su nombre es ExifTool.

Esta herramienta nos permite obtener, editar y eliminar metadatos de casi la totalidad de todos los ficheros. La podemos utilizar tanto en linea de comandos como en entorno gráfico. Para ello, antes de nada instalamos la herramienta que viene adjunta al curso (son dos zip, que hay que extraer):

![](https://github.com/emiliogf10/Proyecto-FDC/blob/643d856c05fc814092817f4413992921b9463562/Hacking_%C3%89tico/exif1.png)

En un zip solo hay un archivo, que tendremos que mover a la carpeta del otro zip (ya extraído), como se ve en la imagen anterior.
A continuación ejecutamos **en modo administrador** el ExifToolGUI:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/643d856c05fc814092817f4413992921b9463562/Hacking_%C3%89tico/exif2.png)

## Dia 19/03/2024

Vemos a ver por ejemplo un archivo de texto que voy a crear, le voy a introducir una frase y lo guardaré como prueba.txt:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a58f0acf4f232939a16a776e3a26045bfe8638b4/Hacking_%C3%89tico/exif3.png)

Vemos que el programa se divide en varias ventanas; la de la ruta (en donde se puede ver todo el árbol de directorios de nuestro PC), la de la previsualización (que en este caso está vacia porque es un archivo de texto llano), la de los ficheros (en donde eliges el fichero) y la de los metadatos. En esta última, nos vamos a la pestaña de ALL para ver todos los metadatos asociados a este fichero. En este caso hay muy pocos metadatos, pero en otros puede haber infinidad de ellos. Ahora vamos a ver el caso de una imagen; en donde vamos a ver que aparecen muchos más metadatos:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a58f0acf4f232939a16a776e3a26045bfe8638b4/Hacking_%C3%89tico/exif4.png)

Vemos que en la ventana de previsualización ya vemos la imagen y en la ventana de metadatos ya aparecen bastantes más que en el caso anterior. Esta imagen fue descargada de internet, por lo tanto seguramente le hayan eliminado metadatos importantes; pero si la foto estuviera intacta, podrían aparecer datos como el modelo del móvil, la marca, la latitud y longitud de donde se sacó la foto, el sistema operativo, etc.

Con esto queremos sacar una conclusión y es que hay que tener mucho cuidado con las imágenes que se envian, por ejemplo por correo. Dichas imágenes tienen diversos metadatos que alguien de forma maliciosa puede usar en nuestra contra. Por otra parte decir que las redes sociales suelen borrar estos metadatos cuando subimos las fotos.

Volviendo a la herramienta, en el apartado de workspace, podremos editar los metadatos que nos interesen o eliminarlos. Después guardamos los cambios y las personas que vean esta foto, la verá con los metadatos que nosotros queremos:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a58f0acf4f232939a16a776e3a26045bfe8638b4/Hacking_%C3%89tico/exif5.png)

**HERRAMIENTAS PARA LA EXTRACCIÓN DE METADATOS: Foca**

En este apartado vamos a utilizar la herramienta Foca (Fingerprint Organizations with Collected Archives) en español 
Organizaciones de huellas dactilares con archivos recopilados, es una herramienta que ha sido creada por ElevenPaths, 
ciberseguridad de Telefónica. Esta herramienta nos permitirá la descarga, extracción y análisis de metadatos de un dominio. También podemos utilizarla como ExifTool, subiendo un archivo local y analizando sus metadatos (sólo vamos a ver la primera opción). Foca emplea 3 buscadores: Google, Bing y DuckDuckGo (antiguamente Exalated). Podemos indicarle que busque en los 3 o que busque en 1 o 2. A continuación dejo 2 links, uno de descarga y otro de Github:

[Link de descarga](https://cybersecuritycloud.telefonicatech.com/innovacion-labs/tecnologias-innovacion/foca)

[Github ElevenPaths: Foca](https://github.com/ElevenPaths/FOCA)

Desde el github, descargamos la útima versión de Foca y descomprimimos el zip. Hay que comprobar que tuvieramos todos los requisitos instalados (están en el apartado de 'Requisites' en el readme de github y en mi caso si que tengo todo instalado) y procederemos a abrir el ejecutable:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a58f0acf4f232939a16a776e3a26045bfe8638b4/Hacking_%C3%89tico/foca1.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a58f0acf4f232939a16a776e3a26045bfe8638b4/Hacking_%C3%89tico/foca2.png)

Lo primero que tenemos que hacer es crear un proyecto. Para ello, nos vamos a la primera opción de la esquina superior izquierda, y seleccionamos new project (o simplemente en la pestaña principal). Primero le damos un nombre (en mi caso Asus), y seleccionamos el dominio que queremos analizar; en mi caso asus.com. Después elegimos la ruta en donde se nos creará el proyecto y le damos a crear:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a58f0acf4f232939a16a776e3a26045bfe8638b4/Hacking_%C3%89tico/foca3.png)

Podemos ver que en la interfaz principal de Foca, en custom search, esta búsqueda se basan en los Dorks mencionados en apartados anteriores; utilizando site:dominio que le hemos dicho y filetype:extensión de archivo:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a58f0acf4f232939a16a776e3a26045bfe8638b4/Hacking_%C3%89tico/foca4.png)

También podemos ver en la esquina superior derecha, tres checkboxes para seleccionar en qué navegadores de los 3 mencionados anteriormente buscará:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a58f0acf4f232939a16a776e3a26045bfe8638b4/Hacking_%C3%89tico/foca5.png)

Y por supuesto, también tendremos la opción de seleccionar el tipo de archivos a buscar:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a58f0acf4f232939a16a776e3a26045bfe8638b4/Hacking_%C3%89tico/foca6.png)

Ahora, le damos al botón de search y se iniciará la búsqueda:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a58f0acf4f232939a16a776e3a26045bfe8638b4/Hacking_%C3%89tico/foca7.png)

## Dia 20/03/2024

Vemos que nos encuentra un montón de archivos; en concreto 24. A continuación procedemos a descargarlos todos (botón derecho encima de cualquier fichero y seleccionamos la opción 'Download all'):

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a58f0acf4f232939a16a776e3a26045bfe8638b4/Hacking_%C3%89tico/foca8.png)

Ahora vamos a extraer todos los metadatos de los ficheros (botón derecho encima de cualquier fichero y seleccionamos la opción 'Extract all metadata') y vamos a ver cómo se clasifican según su categoría en el árbol de la izquierda:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a58f0acf4f232939a16a776e3a26045bfe8638b4/Hacking_%C3%89tico/foca9.png)

En este punto, ya tendremos todos los metadatos extraídos y deberemos analizarlos todos.

**RETO: METADATOS**

El reto es el siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a58f0acf4f232939a16a776e3a26045bfe8638b4/Hacking_%C3%89tico/retoMeta1.png)

Deberemos verificar la veracidad del tweet aplicando Google Hacking.

Mi solución: El profesor no plantea una solución; asi que ahí va la mia. Primeramente fui a Google y busqué el documento del Plan Económico-Financiero 2012-2014 de la comunidad de Madrid. La idea es ver cuál es exactamente la fecha de creación del mismo para saber cuándo se conocieron estos hechos. Abriendo el archivo con ExifTool, veo lo siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a58f0acf4f232939a16a776e3a26045bfe8638b4/Hacking_%C3%89tico/retoMeta2.png)

Veo que la fecha exacta de creación del documento es el 14 de Mayo de 2012, que cuadra en un Lunes. Dicho esto, puedo decir que la información del tweet es falsa; debido a que el consejero dice que en cuanto el documento fue definitivo (el Lunes 14 de Mayo), dio una rueda de prensa, pero añade que fue un Viernes, no el Lunes 12 de la fecha de creación del documento.

## _HERRAMIENTAS_

**ENTORNO**

En este apartado, vamos a configurar el entorno. En este caso, voy a instalar una máquina virtual llamada Huron; que es parecida a la máquina que nos dice en el video que es Buscador de Michael Bazzell y se puede descargar desde [aquí](https://github.com/HuronOsint/OsintDistro). La máquina ya viene configurada con todas las herramientas, pero vamos a ejecutar 'sudo apt-get update' y 'sudo apt-get upgrade' para actualizar nuestra máquina.

Esta máquina cuenta con herramientas como TOR browser, Maltego (herramienta de minería de datos de OSINT), Creepy (herramienta de geolocalización en Twitter, ahora X) o Tinfoleak (herramienta enfocada a la extracción de información de Twitter).

Con la máquina ya lista, podemos ir a por el siguiente apartado.

**OSR FRAMEWORK**

## Dia 22/03/2024

En este apartado vamos a tratar una de las herramientas más completas para extraer información en OSINT. Dicha herramienta es OSRFramework (Open Sources Research Framework). Está desarrollada por Féix Brezo, (@febrezo) y Yaiza Rubio, (@yrubiosec), grupo i3Visio, analistas de ElevenPaths (ciberseguridad de Telefonica).

Vamos a poder trabajar con hasta 279 plataformas diferentes. Para ello, necesitaremos que en nuestra máquina esté instalado python2.7 (en hurón ya viene por defecto). 

En caso de que nuestrá máquina no tuviera OSRFramework, iremos a su [github](https://github.com/i3visio/osrframework) y descargaremos el .zip.

Cuando tengamos la herramienta lista, tendrá una serie de funcionalidades que podremos utilizar:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/919843c02fe3978d43851958f10f51700dea5550/Hacking_%C3%89tico/osr1.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/919843c02fe3978d43851958f10f51700dea5550/Hacking_%C3%89tico/osr2.png)

Podemos acceder a las ayudas de los distintos scripts mediante el nombre del script y '--help'; por ejemplo **'usufy --help'**.

Vemos que tenemos distintas funcionalidades; la primera de ellas es la de encontrar un nombre de usuario en las plataformas antes mencionadas (hasta 279). Son las siguientes:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/919843c02fe3978d43851958f10f51700dea5550/Hacking_%C3%89tico/osr3.png)

## Dia 23/03/2024

Y para buscar por ejemplo en dos de ellas, aplicaremos el siguiente comando (en mi caso voy a utilizar instagram, en donde si que tiene que haber 1 resultado y coinbase en donde no):

![](https://github.com/emiliogf10/Proyecto-FDC/blob/919843c02fe3978d43851958f10f51700dea5550/Hacking_%C3%89tico/osr4.png)

En donde el comando es general y solo cambias el nombre de usuario y la plataforma. Se usa usufy.py porque es el script que busca el nombre de usuario que le indicas y en la plataforma también indicada. El resultado es el siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/919843c02fe3978d43851958f10f51700dea5550/Hacking_%C3%89tico/osr5.png)

Vemos que el resultado es cómo esperabamos y hay una coincidencia. Vamos a ver el resultado con la plataforma coinbase:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/919843c02fe3978d43851958f10f51700dea5550/Hacking_%C3%89tico/osr6.png)

El resultado también es el esperado y no hay ninguna coincidencia. Este script también tiene la posibilidad de buscar en todas las plataformas; poniendo all. También se nos creará un fichero en nuestra carpeta personal con los resultados (se llamará profiles.csv).

Seguidamente vamos con el siguiente script; que es el alias_generator. Este script nos crea diferentes 'alias' a partir de información que le damos. Ejecutamos el siguiente comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/919843c02fe3978d43851958f10f51700dea5550/Hacking_%C3%89tico/osr7.png)

En donde nos empezaran a pedir una serie de datos. Si no queremos introducir algunos campos, lo dejamos en blanco y le damos a enter. En el último apartado, añadimos cosas relacionadas separadas por ',' (en mi caso voy a poner colegiovivas):

![](https://github.com/emiliogf10/Proyecto-FDC/blob/919843c02fe3978d43851958f10f51700dea5550/Hacking_%C3%89tico/osr8.png)

Y le damos a enter para que nos de los resultados (se creará un archivo en nuestra carpeta personal con los resultados):

![](https://github.com/emiliogf10/Proyecto-FDC/blob/919843c02fe3978d43851958f10f51700dea5550/Hacking_%C3%89tico/osr9.png)

Vemos que creó 146 resultados y que creó un fichero llamado 'output.txt':

![](https://github.com/emiliogf10/Proyecto-FDC/blob/919843c02fe3978d43851958f10f51700dea5550/Hacking_%C3%89tico/osr10.png)

Y los alias son los siguientes (captura de la consola):

![](https://github.com/emiliogf10/Proyecto-FDC/blob/919843c02fe3978d43851958f10f51700dea5550/Hacking_%C3%89tico/osr11.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/919843c02fe3978d43851958f10f51700dea5550/Hacking_%C3%89tico/osr12.png)

Vemos que creó diferentes combinaciones para la información que le dimos previamente (no se muestran todos los resultados).

El siguiente script es mailfy.py; que lo que hace es enfrentar el nombre que le damos con hasta 28 provedores de correo diferentes y comprobar si la cuenta se encuentra en leaks (esto significa comprobar si la cuenta ha estado comprometida en una brecha de seguridad). El problema de este script es que la API de la web en las que hace las consultas, ahora es de pago, con lo cual no vamos a poder acceder. Lo que si que voy a mostrar es el comando y las combinaciones:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/919843c02fe3978d43851958f10f51700dea5550/Hacking_%C3%89tico/osr13.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/919843c02fe3978d43851958f10f51700dea5550/Hacking_%C3%89tico/osr14.png)

Lo que haría a continuación es comprobar una a una si la cuenta se encuentra en leaks y nos lo mostraría. A parte, nos mostraría en qué sitios hubo una filtración y se añadirían los resultados al fichero creado en el script usufy.py.

Vamos con el siguiente script que es searchfy.py. Lo que hace este script; es básicamente, buscar un nombre y apellidos en 6 plataformas diferentes:
Las plataformas son las siguientes:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/919843c02fe3978d43851958f10f51700dea5550/Hacking_%C3%89tico/osr15.png)

Y el comando sería el siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/osr23.png)

## Dia 24/03/2024

En donde primero iría el script searchfy.py, después las plataformas en donde queremos buscar las coincidencias (en mi caso en todas) y por último el nombre y apellidos que queremos buscar. El resultado tendría que arrojar una seria de perfiles en instagram que tuvieran el nombre Emilio Garcia en su biografía por ejemplo (da igual que el nombre de usuario sea distinto); pero el script me dice que no hubo coincidencias.

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/osr16.png)

El siguiente script es phonefy.py; que lo que haces es básicamente ver si un número de teléfono dado está reportado como spam teléfonico. El comando es el siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/osr17.png)

Y le damos a enter para que nos diga si el número es SPAM:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/osr18.png)

Efectivamente, este número está reportado en 3 plataformas como spam (por comentarios de usuarios que han recibido llamadas del mismo número). Estos resultados los añade también al archivo profiles.csv.

El siguiente script es entify.py, que busca expresiones regulares de un sitio web (se pueden buscar hasta 12 tipos de expresiones regulares diferentes). Vamos a ejecutar el siguiente comando:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/osr19.png)

Y el resultado sería el siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/osr20.png)

El resultado también podriamos verlo en el archivo profiles.csv.

El último script que vamos a ver es domainfy.py; que nos va a enfrentar un nombre con hasta 844 [TLD](https://kinsta.com/es/base-de-conocimiento/que-es-un-tld/) diferentes y nos va a decir si existe alguno. En mi caso voy a ejecutar el script con el nombre colegiovivas porque sé que existe un dominio denominado colegivivas.com:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/osr21.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/osr22.png)

Vemos que efectivamente aparece el dominio colegiovivas.com. El resultado se guardará en profiles.csv.

OSRFramework dispone de más funcionalidades como la de crear tu propio script en python con su mismo formato y añadirlo (esto es para gente más avanzada y que necesita más funcionalidades de las que ya tiene la herramienta).

**MALTEGO**

En este apartado vamos a ver una herramienta que ya tenemos en nuestra máquina hurón. La herramienta en cuestión es Maltego; que nos servirá para encontrar relaciones entre piezas de información ubicadas en diversas fuentes en internet (minería de datos). Esta herramienta utiliza un concepto que es **Transformadas**, que no es más que una aplicación que va a realizar determinadas acciones en función de unos datos de entrada.

Vamos a utilizar la versión Comunnity (CE,versión gratuíta) y es necesario el registro previo. Por lo tanto, nos creamos una cuenta y actualizamos a la última versión de Maltego.

Cuando abrimos maltego por primera vez, nos aparece una ventana en donde descargar trandsformadas; nosotros por defecto vamos a descargar las que nos dice en el curso (sólo las que están disponibles):

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/maltego1.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/maltego2.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/maltego3.png)

En total descargamos 5 paquetes. Ahora vamos a crear una nueva investigación; vamos al menú superior, y en la primera opción 'Investigate', le damos al botón 'New' y se nos creará un nuevo proyecto:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/maltego4.png)

Ahora tenemos que seleccionar sobre qué vamos a trabajar; en nuestro caso, un dominio. Por lo tanto, en el menú de la izquierda buscamos 'Domain' y arrastramos el objeto a la hoja de proyecto:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/maltego5.png)

Como podemos ver, yo ya le cambié el dominio (hacemos doble click encima del dominio y le ponemos openwebinars). Ahora tenemos que seleccionar qué queremos que haga. Si le damos click derecho encima del dominio, nos aparecerán las transformadas que hemos descargado anteriormente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/maltego6.png)

Nostros como primera prueba, vamos a ir a la opción de 'standard transforms' (que son las transformadas por defecto de Maltego) y vamos a escoger la opción 'Domain owner detail', que nos va a dar todos los detalles del dueño del dominio (si no encontramos la opción, la podemos buscar en la barra de búsqueda). Para que empiece a correr el proyecto, tendremos que darle a la flecha que tiene cada opción (Run all):

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/maltego7.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/maltego8.png)

Vemos que el resultado es un gráfico, pero podemos verlo en formato lista dándole en el menú vertical de 'View' a la opción 'List View':

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/maltego9.png)

## Dia 26/03/2024

Vemos que de la información recopilada, podemos encontrarnos con números de teléfono, direcciones de correo y organizaciones o compañías. Si queremos ver de donde viene toda esta información, hacemos click sobre el dominio openwebinars y nos vamos al menú inferior de la derecha. En donde pone 'WHOIS info', hacemos click en los puntos suspensivos:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/maltego10.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/maltego11.png)

Aqui vemos que tenemos la información categorizada; y es posible gracias a laz expresiones regulares que se ejcutan sobre el dominio y extraen la información.

Ahora vamos a ejecutar todas las transformadas de Maltego del dominio openwebinars.com (nos va a ofrecer información sobre la infraestructura del dominio). Para ello, borramos todos los datos anteriores y nos quedamos sólo con el dominio. Al igual que antes, click derecho encima del dominio y esta vez elegimos la primera opción; 'All transforms' y le damos a la flechita. El resultado será el siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/maltego12.png)

Vemos que no se puede ver toda la información en formato gráfico, por lo que vamos a optar por verla en formato tabla para que sea más cómodo:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/maltego13.png)

La información que nos da ahora, es mucho más extensa que la que nos dio antes; ahora nos da subdominios, más compañías, más nombres de dominio y más direcciones de correo (entre otra información). El objetivo de esta herramienta, es ir recavando inforamción y ejecutando más transformadas de la información que nos interese; y de ahí, sacar conclusiones.

No solo se puede trabajar con dominios, se puede trabajar con infinidad de cosas y nosotros ahora vamos a probar con un alias (nombre de usuario). Para ello, yo en mi caso, creo un nuevo proyecto y busco en la barra de búsqueda del menú de la izquierda 'Alias', y lo añado al proyecto:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/maltego14.png)

Yo voy a buscar información, por ejemplo, del usuario 'colegiovivas', y en concreto las redes sociales. Para ello, tenemos que instalar un nuevo tipo de transformadas de tipo redes sociales. Para ello nos vamos al menú principal, filtramos por social media e instalamos las de Google:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/maltego15.png)

Ahora volvemos a nuestro proyecto, click derecho encima del Alias y escogemos las transformadas de Google. Escogemos la segunda opción (To social media profiles) y el reultado es el siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/maltego16.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/maltego17.png)

Vemos que nos ofrece varios resultados, entre ellos la cuenta oficial de instagram y twitter. También vemos que aparecen resultados que no parecen ser de nuestra búsqueda, y para investigar si son cuentas que nosotros buscamos, dariamos doble click encima de la cuenta que deseamos investigar y nos vamos al apartado de properties:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/maltego18.png)

Vemos que nos aparece la url del canal, asi como el nombre y alias.

Vemos que podemos sacar infinidad de información de un montón de cosas y que la aplicación está muy completa y muy conseguida. Lo malo, es que tiene un límite de transformadas y si quieres más, tienes una suscripción de pago.

**REPOSITORIOS OSINT**

En este apartado veremos repositorios que nos servirán en nuestra búsqueda en OSINT. Dejo los links a continuación:

1. IntelTechniques -> (Pincha aqui para ir a su web)[https://inteltechniques.com/menu.html]
2. OSINTFramework -> (Pincha aqui para ir a su web)[http://osintframework.com/]
3. Ciberpatrulla -> (Pincha aqui para ir a su web)[https://ciberpatrulla.com/links/]

Es importante destacar la primera web; en donde en la pestaña 'Tools' tenemos multitud de herramientas para extraer información. Podemos encontrar la búsqueda reversa de imágenes, búsqueda de información de perfiles de instagram y otras redes sociales, búqueda de información de emails, etc. Estos repositorios podrían utilizarse o consultarse cuando por ejemplo no sabemos a qué fuente acudir para extraer información sobre algo.

**WHOIS Y DIRECCIONES IP**

En este apartado vamos a tratar herramientas para analizar información [WHOIS](https://www.redeszone.net/tutoriales/internet/que-es-whois/) y direcciones IP.

Hay multitud de herramientas, pero en este caso vamos a ver DomainTools. Accedemos a [esta dirección](https://whois.domaintools.com/) y buscamos por ejemplo el dominio colegiovivas.com:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/whois1.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/whois2.png)

Vemos que nos aparece información como fecha de creación de dominio, IP, Localización del dominio, estado del dominio, etc. En la parte derecha, también podemos ver que se comparó el nombre del dominio con varios TLD y nos dice si cada uno de ellos está ocupado o no (mínimo el dominio colegiovivas.com tiene que estar ocupado):

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/whois3.png)

También podemos ver la última captura que tiene DomainTools sobre el sitio web:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/whois4.png)

También dispone de muchas opciones de pago, por ejemplo historial de hosting, historial de fechas, reversa de IP, etc.

Otras herramientas en cuanto a información WHOIS son [MXToolBox](https://mxtoolbox.com/) que por ejemplo nos dejará hacer 'Blacklisting' (nos dirá si una IP está adherida a una lista de SPAM) y analizar cabeceras de correo electrónico. Otra herramienta es [ViewDNS.info](https://viewdns.info/) que ofrece opciones gratuítas frente a DomainTools y permite el uso de comodines(*).

En cuanto a información de direcciones IP, tenemos la herramienta [IP2Location](https://www.ip2location.com/)  que nos permitirá la geolocalización exacta de laS coordenadas de la dirección IP e información detallada del ISP.hosting en donde esté alojada la dirección IP.

**RETO: HERRAMIENTAS**

En este apartado, como bien dice el título, nos proponen el siguiente reto:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/rh1.png)

[Enlace del tweet](https://twitter.com/JulianC92181512/status/831214812592033798)

Mi solución: Lo primero que hago es buscar el [evento](https://www.meetup.com/es-ES/Growth-Hack-Spain-Meetup/events/235930447/). Vemos que el evento empezaba a las 19h y terminaba a las 22h y efectivamente era en el Campus de Madrid:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/rh2.png)

El tweet del usuario Julian es el mismo día del evento, 53 minutos más tarde de que éste empezara. La charla trataba sobre búsqueda avanzada de Google: Google Hacking. En los comentarios, veo que el primero deja un link con la presentación del curso, pero el acceso está restringido en Google Drive:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/rh3.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/rh4.png)

Igualmente, compruebo quién es ese usuario y busco en la lista de personas participantes en el evento. El resultado es que ese usuario es uno de los organizadores del evento:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/a5aaba60bfa9977b2cfa12aab50cb4daba2d930c/Hacking_%C3%89tico/rh5.png)

En la lista de asistentes al evento, busco a Julián y no encuentro a ninguno, lo que me lleva a pensar lo siguiente: El usuario posteó ese tweet diciendo que le estaban enseñando a robar datos a Paginas Amarillas, entendiendo erróneamente el concepto de Google Hacking (que lo único que hace es buscar en profundidad mediante Dorks información que **es pública** y por lo tanto no estás robando). Además, como dije antes, el usuario no aparece en la lista de asistentes, pero a mi forma de entender, la única manera de acceder a la presentación del curso es con un correo y este correo tenía que estar en una 'lista' para que fuera aceptado (solo podrían ver la presentación los asistentes del curso). Con lo cual determino que el usuario SI acudió a dicho evento.


## _MONITORIZACION EN OSINT_

**HERRAMIENTAS DE MONITORIZACION OSINT**

En este apartado vamos a mencionar unas herramientas para la monitorización en OSINT. La mayoría de herramientas se proveen de Twitter por dos motivos: facilidad de la API (poco restrictiva y se le puede sacar mucho partido) y por su finalidad (la finalidad de twitter no es sòlo para exponer críticas y demás; sino que se publican noticias, información sobre eventos e incluso debates sobre algún tema.

## Dia 28/03/2024

En el momento de la realización del curso, había dos herramientas similares **gratuitas**, pero ahora mismo las dos son de pago. He buscado alguna herramienta similar, pero no he encontrado ninguna que haga lo mismo que las otras dos, que son [TweetDeck](https://pro.twitter.com/i/premium_sign_up?referring_page=pro) y [HootSuite](https://www.hootsuite.com/en-gb/plans). Además, todas estas herramientas, que son complementarias y te ayudan a tener un informe de tus cuentas, son de pago.

**SERVICIOS DE ALERTA**

En este apartado vamos a ver herramientas para monitorizar en sitios web, blogs, foros, etc.

Primero tenemos 'Google alerts': Servicio de alertas de Google. Solamente necesitas una cuenta de Google y es muy intuitivo y fácil de utilizar. Pincha [aqui](https://www.google.es/alerts) para crear una alerta de Google. Pinchamos en el enlace anterior y nos aparece lo siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/4984a54e836e1a10e248b9dc102435f99a9a87a7/Hacking_%C3%89tico/alertas1.png)

En esta barra de búsqueda se pueden utilizar tanto operadores booleanos como Dorks. Por lo tanto, vamos a crear una alerta:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/4984a54e836e1a10e248b9dc102435f99a9a87a7/Hacking_%C3%89tico/alertas2.png)

Voy a crear una alerta para 'curso' y 'osint'. Esto quiere decir que todas las publicaciones que contengan estas dos palabras se me mostrarán. También vemos que tenemos una pestaña de 'Mostrar opciones'. La vamos a desplegar para ver que contiene:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/4984a54e836e1a10e248b9dc102435f99a9a87a7/Hacking_%C3%89tico/alertas3.png)

Vemos que tenemos opciones como la frecuencia, las fuentes de donde queremos la información (noticias, blogs, videos, foros, etc), el idioma y el correo a donde queremos que nos mande la alerta. En mi caso, creé una alerta asi:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/4984a54e836e1a10e248b9dc102435f99a9a87a7/Hacking_%C3%89tico/alertas4.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/4984a54e836e1a10e248b9dc102435f99a9a87a7/Hacking_%C3%89tico/alertas5.png)

## Dia 30/03/2024

Y vemos que ya nos aparece la alerta creada. La alerta, por supuesto se puede modificar con otros datos por ejemplo. En caso de que hubiera una alerta, nos mandarían un correo electrónico a la dirección que le hemos indicado con duchos datos.

Una herramienta parecida a Google Alerts es TalkWalker Alerts. La diferencia es que TalkWalker Alerts también nos va a incluir Twitter (ahora X). Pincha [aqui](https://www.talkwalker.com/alerts) para crear una alerta en TalkWalker Alerts. Vemos que la interfaz es la siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/4984a54e836e1a10e248b9dc102435f99a9a87a7/Hacking_%C3%89tico/alertas6.png)

Le damos a crear alerta, y nos aparece los siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/4984a54e836e1a10e248b9dc102435f99a9a87a7/Hacking_%C3%89tico/alertas7.png)

Aqui, evidentemente, escribimos los criterios de alerta, al igual que hicimos antes con Google Alert:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/4984a54e836e1a10e248b9dc102435f99a9a87a7/Hacking_%C3%89tico/alertas8.png)

Y le damos a crear alerta:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/4984a54e836e1a10e248b9dc102435f99a9a87a7/Hacking_%C3%89tico/alertas9.png)

Vemos que ya nos aparece la alerta creada. Esta herramienta requiere que confirmes el correo electrónico o que te crees una cuenta.

## _PRIVACIDAD Y ANONIMATO_

**CREACION DE IDENTIDAD ANONIMA Y DIGITAL**

En este apartado vamos a tratar, como bien dice el título, de la creación de identidad anónima y digital. Primeramente vamos a ver un resumen de todo lo que necesitamos saber:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/4984a54e836e1a10e248b9dc102435f99a9a87a7/Hacking_%C3%89tico/privacidad1.png)

La idea de cuanto realizamos una ciberinvestigación, es la de dejar el menor número de huellas digitales posibles, adoptando una serie de medidas y partiendo de que dejar un rastro de cero huellas digitales es **IMPOSIBLE**. Para ello, dispondremos de correos electrónicos, números de teléfono, perifles de redes sociales, etc sin ningún dato personal y exclusivamente dedicado a cada investigación. Después entra el tema de la organización, que es muy importante y debemos cumplir. Necesitaremos un mapa de fuentes **finito**, es decir, que no sea muy extenso para poder ser más eficientes y rápidos en nuestra investigación. Por último, es recomendable tener un documento en donde poder plasmar toda la información que vamos recavando (preferiblemente en formato excel) y anotar toda la información sobre la investigación (objetivo, pasos a seguir, herramientas que se van a emplear, perfiles de RRSS que se van a utilizar, resultados y borrado de huellas).

Lo siguiente es la creación de las RRSS y el correo:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/4984a54e836e1a10e248b9dc102435f99a9a87a7/Hacking_%C3%89tico/privacidad2.png)

En donde básicamente nos dice que la creación de perfiles en RRSS tiene que ser lo más realista posible, alimentándola con amigos, publicaciones y foto de perfil.
La foto de perfil puede ser sacada de un banco de imágenes gratuítas (para no vulnerar ningún derecho) , pero OJO, tendremos que editarla con algún editor para que no sea resultado de una búsqueda inversa de imágenes (cambiar el color de pelo, color de ojos, color de piel, etc).

Para todo esto, existe una herramienta que es FakeNameGenerator y que nos servirá para crear un perfil falso según los datos que le demos. El enlace es [este](https://es.fakenamegenerator.com/) y aparecería lo siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/4984a54e836e1a10e248b9dc102435f99a9a87a7/Hacking_%C3%89tico/privacidad3.png)

Le damos a opciones avanzadas y nos aparece esto:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/4984a54e836e1a10e248b9dc102435f99a9a87a7/Hacking_%C3%89tico/privacidad4.png)

En mi caso, le voy a dar un nombre español, con sexo masculino y con una edad comprendida entre 32 y 40 años de edad:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/4984a54e836e1a10e248b9dc102435f99a9a87a7/Hacking_%C3%89tico/privacidad5.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/4984a54e836e1a10e248b9dc102435f99a9a87a7/Hacking_%C3%89tico/privacidad6.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/4984a54e836e1a10e248b9dc102435f99a9a87a7/Hacking_%C3%89tico/privacidad7.png)

Vemos que nos crea un perfil súper completo con los datos que le hemos dado. Vemos también, que nos creó una dirección de correo electrónico desechable, que podemos utilizar sin ningún problema para recibir mensajes:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/4984a54e836e1a10e248b9dc102435f99a9a87a7/Hacking_%C3%89tico/privacidad8.png)

En el caso del email, podemos utilizar el que nos crea FakeNameGenerator o disponer de un [ProtonMail](https://proton.me/mail), que facilitándole un usuario y una contraseña, nos proveerá de un correo electrónico cifrado. Si queremos un email un poco más real, con un dominio gmail.com por ejemplo, tendremos que crear un correo desde una VPN (para enmascarar nuestra ubicación), sin datos personales y con un correo de recuperación desechable o ProtonMail. En caso de que nos pidiera algún número de teléfono, tendríamos que recurrir a un número de teléfono virtual, que es de pago. Esto se debe a que si optamos por un número de teléfono temporal (gratuíto), éste seguramente fue utilizado por más gente y estará baneado, con lo cual, tardaremos muchísmo tiempo en encontrar un número de teléfono que nos sirva.

**ENMASCARANDO NUESTRA IDENTIDAD**

En este apartado vamos a enmascarar nuestra identidad. Primeramente, hay que decir que todas las investigaciones deben hacerse desde una máquina virtual (la MV se ejecuta en memoria y  por lo tanto no afectará a nuestro sistema anfitrión y además, cuando terminemos nuestra investigación, borraremos la MV y con ella todas las huellas digitales).

Es recomendable también, disponer de una memoria USB en la que tengamos aplicaciones para trabajar en diferentes equipos distintos al propio. Por ejemplo, podemos tener un SO llamado [Tails](https://tails.boum.org/) que se centra en la privacidad, el navegador TOR y la sincronización de nuestro navegador web (poniendo la contraseña podremos acceder a nuestros marcadores, pestañas, etc).

La idea de todo esto es enmascarar nuestra IP:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/4984a54e836e1a10e248b9dc102435f99a9a87a7/Hacking_%C3%89tico/privacidad9.png)

Como bien apreciamos en la foto anterior, podremos enmascarar nuestra IP de 3 formas: un [Proxy](https://www.xataka.com/basics/que-es-un-proxy-y-como-puedes-utilizarlo-para-navegar-de-forma-mas-anonima), el navegador TOR (menos recomendable porque al utilizar tanto enrutamiento, se disminuye la velocidad de navegación) y la opción más recomendada que es una [VPN](https://www.xataka.com/basics/que-es-una-conexion-vpn-para-que-sirve-y-que-ventajas-tiene). Hoy en dia existen diversas VPN, pero algunas de las mejores son NordVPN o ProtonVPN.

Hay que hacer mención también a la volatilidad de la información; hay información en internet que puede durar unos pocos minutos o incluso segundos. Por ello, es importante obtener alguna evidencia de donde se saca esa información. Para ello, vamos a utilizar una herramienta que se llama [eGarante](https://www.egarante.com), en donde le enviaremos el link del sitio web que queramos certificar y se generará un pdf con un sello temporal. Este documento tiene validez judicial.

Por último, también vamos a mencionar que es importante tener un buen gestor de contraseñas en donde sólo tengas que saber una contraseña maestra, y de ahí acceder a un contenedor que contenga el resto de contraseñas. Uno de los mejores gestores de contraseñas es [Bitwarden](https://bitwarden.com).

## Dia 01/04/2024

# QUINTO TALLER : HERRAMIENTAS AVANZADAS PARA BUSQUEDAS OSINT (Tiempo de realización: 2 dias aproximadamente)

En este taller seguiremos con la búsqueda en OSINT (curso anterior). Muchos de los apartados que comentan en este video, ya lo expliqué en el curso anterior, por lo tanto lo obviaremos y nos centraremos en los apartados novedosos. También, mencionaremos el nombre de muchas herramientas que nos servirían para la búsqueda avanzada para OSINT en diferentes plataformas, pero no veremos su uso.

Para empezar, tenemos como novedad nuevos buscadores: [Uvrx](http://www.uvrx.com/es/),[Alltheinternet](https://www.alltheinternet.com/) y [SearchEngineColos
sus](https://www.searchenginecolossus.com/).

Después tenemos más distribuciones OSINT (máquinas virtuales con las herramientas necesarias ya preinstaladas) a parte de la ya nombrada anteriormente Hurón; que son [Osintux](https://www.osintux.org/descargas) y [CsiLinux](https://csilinux.com/csi-linux-downloads/).

**EMAILS**

Ahora, en el apartado de verificación de emails, tenemos 2 herramientas destacadas: 

1. [verifyemailaddres](https://www.verifyemailaddress.org) que verifica si un email es válido.
2. [EmailSherlock](https://www.emailsherlock.com) que busca emails en varios servicios.

**GEOLOCALIZACION**

Ahora vamos a mencionar 3 herramientas de geolocalización: 

1. [tracemyip](https://tools.tracemyip.org) que lo que hace es localizar la IP que le mandes.
2. Creepy que es una herramienta incluida en Hurón Y Osintux y sirve para localizar el origen de un tweet.
3. [Google Maps](https://www.google.es/maps/preview) que localiza por coordenadas entre otras cosas.


**IMAGENES**

El siguiente apartado trata sobre la búsqueda de imágenes. Vamos a mencionar 2 herramientas muy interesantes: 

1. [loc.alize](https://loc.alize.us) que sirve para la localización de fotos.
2. [pictriev](http://www.pictriev.com) que sirve para el reconocimiento facial.

**FACEBOOK**

En este apartado vamos a mencionar 4 herramientas para Facebook bastante útiles: 

1. [lookup-id](lookup-id.com) es una herramienta utilizada para saber el ID de un usuario de Facebook
2. [intelx.io/tools](https://intelx.io/tools?tab=facebook) que es como una búsqueda avanzada por filtros
3. [fdown](https://fdown.net/es/) que es un descargador de videos de Facebook
4. [whopostedwhat](whopostedwhat.com) que es un buscador de perfilesy palabras clave.

**INSTAGRAM**

En este apartado vamos a ver 3 herramientas para Instagram:

1. [picuki](https://www.picuki.com) que sirve para analizar un perfil de Instagram.
2. [toopics](toopics.com) que al igual que picuki, sirve para analizar un perfil de Instagram.
3. [downloadgram](https://downloadgram.org) que sirve para descargar fotos y videos de Instagram.

**TWITTER (X)**

En este apartado vamos a ver 3 herramientas para Twitter:

1. [tinfoleak](https://tinfoleak.com) es una herramienta para analizar un perfil de Twitter.
2. [onemilliontweetmap](onemilliontweetmap.com) es una herramienta para el mapeo de tweets.
3. MentionMap es una herramienta incluida en las distros de OSINT que sirve para el mapeo de tweets al igual que onemilliontwwetmap.

**PERSONAS**

En este apartado vamos a mencionar 4 herramientas para buscar personas:

1. [pipl](https://pipl.com) es una herramienta que sirve para buscar personas.
2. [lullar](https://com.lullar.com) es una heramienta que sirve para buscar perfiles de personas.
3. [Webmii](https://webmii.com) es una herramienta para buscar personas mediante nombre y apellido.
4. [abc telefonos](https://www.abctelefonos.com) herramienta para buscar personas.

**SOFTWARE**

En este apartado vamos a mencionar sólo una herramienta porque las demás ya las vimos en cursos y talleres anteriores (Foca, Osint Framework y Maltego).

La nueva herramienta de esta apartado se llama [Reaper.social](https://reaper.social) y nos permitirá recopilar y analizar información de redes sociales.

**TELEFONO**

## Dia 03/04/2024

En este apartado vamos a mencionar 3 herramientas que nos van a ayudar a conseguir información sobre teléfonos:

1. [Imei.info](https://www.imei.info) que nos servirá para saber la información del IMEI de un teléfono.
2. [Sync.me](https://sync.me) que nos servirá para buscar números de teléfono.
3. [truecaller](https://www.truecaller.com) nos servirá, al igual que sync.me, a buscar números de teléfono.

Hemos acabado la parte 'teórica' en donde mencionamos algunas herramientas a mayores para nuestras búsquedas en OSINT. Ahora probaremos alguna herramienta, pero las que ya probamos en apartados anteriores, las pasaré por alto:

1. [Maltego](#maltego)
2. [OSR Framework](#osr-framework)

Vamos a ver como es la vista de OSR Framework desde el cliente web:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f6eabb5ef8b1940167bd8b957cf9f48f7f86a1b2/Hacking_%C3%89tico/osrweb1.png)

Vemos que tenemos un montón de apartados, con diferentes herramientas para buscar información. Voy a desplegar dos apartados: Metadata (metadatos) y Domain Name (Nombres de dominio). Veremos que tendremos herramientas ya vistas (pueden ser apps o sitios web) y otras que no:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f6eabb5ef8b1940167bd8b957cf9f48f7f86a1b2/Hacking_%C3%89tico/osrweb2.png)

En el cliente web, simplemente abririamos el aprtado que nos interese y seleccionaríamos la herramienta que nos interese para obtener información sobre ese tema.

## Dia 07/04/2024

Ahora vamos a probar la herramienta 'Pictriev', que nos buscará reconociendo la cara e que hay en la foto. Para ello, buscamos y descargamos una foto cualquiera de una persona y la guardamos en nuestro equipo. A continuación nos vamos al sitio web de Pictriev y subimos la imagen:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f6eabb5ef8b1940167bd8b957cf9f48f7f86a1b2/Hacking_%C3%89tico/pictriev1.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f6eabb5ef8b1940167bd8b957cf9f48f7f86a1b2/Hacking_%C3%89tico/pictriev2.png)

Vemos que nos dice que es un hombre 100% seguro y que tiene más o menos 40 años.

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f6eabb5ef8b1940167bd8b957cf9f48f7f86a1b2/Hacking_%C3%89tico/pictriev3.png)

Ahora vemos que nos dice con qué porcentaje se parece a ciertas personas 'famosas'.

Por último, veremos la herramienta 'Picuki', que simplemente busca perfiles en instagram con el nombre que le damos. Para ello accedemos al sitio web de Picuki y escribimos el nombre que queremos averiguar:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f6eabb5ef8b1940167bd8b957cf9f48f7f86a1b2/Hacking_%C3%89tico/picuki1.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f6eabb5ef8b1940167bd8b957cf9f48f7f86a1b2/Hacking_%C3%89tico/picuki2.png)

Vemos que nos aparecen un montón de perfiles. Si entramos en uno cualquiera, vemos que también podemos ver sus estadísticas pulsando en el botón 'stats' en la parte superior derecha:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f6eabb5ef8b1940167bd8b957cf9f48f7f86a1b2/Hacking_%C3%89tico/picuki3.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f6eabb5ef8b1940167bd8b957cf9f48f7f86a1b2/Hacking_%C3%89tico/picuki4.png)

También, podremos editar una publicación y despues descargarla y guardarla:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f6eabb5ef8b1940167bd8b957cf9f48f7f86a1b2/Hacking_%C3%89tico/picuki5.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f6eabb5ef8b1940167bd8b957cf9f48f7f86a1b2/Hacking_%C3%89tico/picuki6.png)

# SEGUNDO CURSO : CURSO DE HACKING WEB (Tiempo de realización: 7 dias aproximadamente)

## _INTRODUCCION__

**QUE ES EL HACKING**

En este apartado, vamos a introducir el curso definiendo la palabra clave 'Hacking'. **El hacking es la búsqueda y explotación de vulnerabilidades de seguridad en sistemas o redes**. Como bien dice la definición, vamos a encontrar vulnerabilidades de seguridad y aprovecharlas para atacar un sistema o una red. Se puede aplicar el hacking a prácticamente todos los sistemas que estén conectados a una red; como pueden ser servidores, apps móviles, aplicaciones web, bases de datos, etc. En este curso, nos centraremos en el hacking en aplicaciones web.

El referente del Hacking web es la OWASP (Open Web Application Security Project), que es una organización sin ánimo de lucro cuyo objetivo es mejorar la seguridad del software. 

## _INYECCIONES DE CODIGO__

**SQL INJECTION**

En este apartado vamos a hacer un ataque, por lo que se llama 'SQL Injection'. Este término se refiere a la manipulación de una consulta SQL para lograr un resultado diferente al objetivo con el que fue diseñada, es decir, vamos a introducir código para modificar la consulta SQL que hace la propia web para obtener un resultado diferente.

Como ejemplo, vamos a poner un login de un sitio web:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f6eabb5ef8b1940167bd8b957cf9f48f7f86a1b2/Hacking_%C3%89tico/sqli1.png)

En el sitio web, cuando introduces tu usuario y contraseña y le das a acceder, internamente hace 1 consulta SQL:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f6eabb5ef8b1940167bd8b957cf9f48f7f86a1b2/Hacking_%C3%89tico/sqli2.png)

Vemos que simplemente está seleccionado de la tabla 'users', el o los usuarios que tengan el usuario y la contraseña dados en los textbox.

Ahora vamos a ver, qué pasa con la siguiente imagen:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f6eabb5ef8b1940167bd8b957cf9f48f7f86a1b2/Hacking_%C3%89tico/sqli3.png)

Vemos que le ponemos como usuario “999' or ‘1’ = ‘1’ --” que es una inyección de código SQL, y una contraseña cualquiera. A continuación, vamos a ver la consulta que se forma cuando le damos al botón acceder:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f6eabb5ef8b1940167bd8b957cf9f48f7f86a1b2/Hacking_%C3%89tico/sqli4.png)

Podemos ver que es la misma consulta que antes, pero ahora en el usuario hay una booleana que compara si el usuario es '999' o '1' = '1' (que es una condición que SIEMPRE se va a cumplir). A esto hay que añadirle los dos guiones al final, que quieren decir que lo que venga después de ellos, sea ignorado (comentario SQL en bases de datos ORACLE).

Dicho esto, la consulta nos va a dar todos los registros de la tabla (porque todos cumplen la condición de '1' = '1') y nos vamos a loguear con el primero de ellos sin tener ni usuario ni contraseña.

Como hemos dicho antes, lo que hemos hecho fue modificar la consulta que hace el sitio web para obtener un resultado diferente al que estaba diseñado la consulta inicialmente.

Cómo podemos evitar esto? La manera de que esto no ocurra, es saneando los datos que introduce el usuario:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f6eabb5ef8b1940167bd8b957cf9f48f7f86a1b2/Hacking_%C3%89tico/sqli5.png)

Lo que no se debe hacer, es concatenar directamente lo que nos introduce el usuario, sino que debemos sanearlo antes (por ejemplo eliminar las comillas simples, comillas, comentarios de linea, etc) y después concatenarlo. 

**SQL INJECTION: ATAQUE EN LOGIN**

En este apartado, vamos a hacer un ataque por SQL Injection sobre un formulario de login en una aplicación. Esto se podría poner en práctica con páginas que no tengan saneado de datos del usuario. Para ello tenemos que instalar Xampp (en mi caso la version 5.6.3, porque el formulario que nos proporcionan en el curso puede tener funciones que en versiones más nuevas estén obsoletas) e importar las bases de datos que vamos a utilizar. 

## Dia 08/04/2024

Dicho esto, movemos la carpeta del foro a la carpeta 'htdocs' y nos vamos al panel de Xampp para iniciar el Apache y el MySql:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/72745fb61b7cbad7d5d495ad8473482ed4f93c6f/Hacking_%C3%89tico/sqli6.png)

Hecho esto, nos vamos a nuestro navegador y ponemos nuestra dirección IP + /foro:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/72745fb61b7cbad7d5d495ad8473482ed4f93c6f/Hacking_%C3%89tico/sqli7.png)

Y vemos que ya nos lleva al login de un foro. Ahora vamos a proceder a entrar sin ninguna credencial. Para ello, vamos a escribir en el campo de usuario, lo que vimos anteriormente con una contraseña cualquiera:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/72745fb61b7cbad7d5d495ad8473482ed4f93c6f/Hacking_%C3%89tico/sqli8.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/72745fb61b7cbad7d5d495ad8473482ed4f93c6f/Hacking_%C3%89tico/sqli9.png)

Y vemos que nos hemos logueado correctamente con el usuario 'Prabhu Bhakta', que es el primero de los usuarios de la base de datos. 

![](https://github.com/emiliogf10/Proyecto-FDC/blob/72745fb61b7cbad7d5d495ad8473482ed4f93c6f/Hacking_%C3%89tico/sqli10.png)

Es importante mencionar, que en este caso al final de la condición, hemos puesto un '#' porque es así como se denominan los comentarios de línea en MySql.

**SQL INJECTION: OBTENCION DE DATOS**

En este apartado vamos a ver un ataque de SQL Injection en otro tipo de formulario y como objetivo la obtención de datos. 

Para ello, vamos a utilizar otro formulario que nos dan en el curso. Tenemos que meter su carpeta en la carpeta htdocs e ir a nuestro navegador (en la siguiente imagen se muestra la carpeta 'demos', que es la de este formulario, y la carpeta 'foro' que es la del formulario anterior, las dos dentro de la carpeta htdocs):

![](https://github.com/emiliogf10/Proyecto-FDC/blob/72745fb61b7cbad7d5d495ad8473482ed4f93c6f/Hacking_%C3%89tico/sqli11.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/72745fb61b7cbad7d5d495ad8473482ed4f93c6f/Hacking_%C3%89tico/sqli12.png)

Ya en este sitio web, vamos a proceder a obtener información mediante SQL Injection. Este formulario, funciona de la siguiente manera:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/72745fb61b7cbad7d5d495ad8473482ed4f93c6f/Hacking_%C3%89tico/sqli13.png)

Tenemos 4 articulos guardados en la base de datos, con sus respectivos precios. Al escribir el articulo que queremos y darle al botón 'Enviar', nos dirá en una tabla el precio del producto:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/72745fb61b7cbad7d5d495ad8473482ed4f93c6f/Hacking_%C3%89tico/sqli14.png)

Al igual que antes, vamos a escribir lo siguiente para modificar la consulta sql interna y que nos muestre todos los datos:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/72745fb61b7cbad7d5d495ad8473482ed4f93c6f/Hacking_%C3%89tico/sqli15.png)

## Dia 13/04/2024

Vemos que como la condición de '1' = '1' SIEMPRE se va a cumplir, nos muestra todos los datos de la tabla. Ahora vamos a escribir el siguiente código a ver que pasa:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/035e2d27084846d29be404ddb22f6e6aa59ae787/Hacking_%C3%89tico/sqli16.png)

En donde podemos ver que estamos utilizando un union para enlazar la primera consulta que no es más que algo que hemos escrito aleatoriamente al igual que antes, y una consulta en donde seleccionamos dos columnas: la primera que es null y sería el nombre del producto y la segunda con la cadena 'hola' que sería la del precio del producto. Vamos a ver el resultado:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/035e2d27084846d29be404ddb22f6e6aa59ae787/Hacking_%C3%89tico/sqli17.png)

Nos dice claramente, que con el 'SELECT' hemos seleccionado el número de columnas incorrecto. Esto se debe a que el número de columnas a un lado y al otro del UNION tienen que ser iguales. Nosotros, hemos puesto la columna que devuelve null y la columna con la cadena 'hola', pensando que la consulta original sólo devuelve dos campos; pero resulta que puede ser que se devuelva otro campo que no se represente. Para todo ello, añadimos un campo más que sea null, pensando que sean 3 campos los que devuelve:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/035e2d27084846d29be404ddb22f6e6aa59ae787/Hacking_%C3%89tico/sqli18.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/035e2d27084846d29be404ddb22f6e6aa59ae787/Hacking_%C3%89tico/sqli19.png)

Vemos que efectivamente, devuelve 3 campos y que sería el primero el que no se representaría porque la cadena 'hola' se ha movido a la columna del nombre del producto. Para que nos sirve todo esto? Pues bien, lo primero para saber que el formulario, efectivamente es vulnerable a un ataque de SQL Injection y segundo para sacar datos que nos interesen:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/035e2d27084846d29be404ddb22f6e6aa59ae787/Hacking_%C3%89tico/sqli20.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/035e2d27084846d29be404ddb22f6e6aa59ae787/Hacking_%C3%89tico/sqli21.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/035e2d27084846d29be404ddb22f6e6aa59ae787/Hacking_%C3%89tico/sqli22.png)

Podemos ver que acabamos de sustraer todas las tablas de la base de datos. En la columna de nombre de producto nos va a mostrar el nombre de la base de datos y en la columna de precio del producto nos va a mostrar las columnas de esa base de datos; por lo tanto podemos ver que tenemos una base de datos llamada 'CHARACTER_SETS' que tiene cuatro columnas, otra llamada 'COLLATIONS' que tiene 6 columnas, etc. La mayoría de estas bases de datos son del catálogo se MySql. Ahora vamos a ver todas las tablas y sus bases de datos y para ello ejecutamos la siguiente consulta:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/035e2d27084846d29be404ddb22f6e6aa59ae787/Hacking_%C3%89tico/sqli23.png)

En donde vemos que seguimos usando el UNION y a su derecha tenemos una consulta que nos devuelve 3 campos al igual que la consulta original. La diferencia está en el segundo campo, en donde utilizamos una función para delimitar el nombre de la base de datos y 
el nombre de la tabla en este caso por un punto. Ejecutamos la consulta:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/035e2d27084846d29be404ddb22f6e6aa59ae787/Hacking_%C3%89tico/sqli24.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/035e2d27084846d29be404ddb22f6e6aa59ae787/Hacking_%C3%89tico/sqli25.png)

Y vemos que efectivamente, en la columna de lo que sería el nombre del producto, tenemos 'nombre de bd.nombre de tabla' y en la columna de lo que sería el precio del producto tenemos, al igual que antes el nombre de la columna.

Ahora vamos a mostrar los usuarios del foro del apartado anterior y sus contraseñas. Para ello ejecutamos la siguiente consulta:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/035e2d27084846d29be404ddb22f6e6aa59ae787/Hacking_%C3%89tico/sqli26.png)

En donde vemos que básicamente siempre es la misma consulta, pero en este caso seleccionamos las columnas 'username' y 'password' de la tabla users y base de datos forum. Vamos a ver el resultado:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/035e2d27084846d29be404ddb22f6e6aa59ae787/Hacking_%C3%89tico/sqli27.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/035e2d27084846d29be404ddb22f6e6aa59ae787/Hacking_%C3%89tico/sqli28.png)

Vemos que efectivamente, nos muestra en la primera columna el nombre del usuario y el la segunda columna su respectiva contraseña.

**CROSS-SITE SCRIPTING (XSS)**

En este apartado vamos a ver que es el XSS (Cross-Site Scripting). XSS no es más que una vulnerabilidad que permite la ejecución de un script en el lado cliente al visualizar una página web. Es decir, una vez logueados como hemos hecho antes en el foro, nos vamos a ir al apartado de crear topic, arriba a la derecha:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/035e2d27084846d29be404ddb22f6e6aa59ae787/Hacking_%C3%89tico/sqli29.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/035e2d27084846d29be404ddb22f6e6aa59ae787/Hacking_%C3%89tico/sqli30.png)

En la zona de 'Topic Title' de arriba le ponemos un nombre (en mi caso post) y en la zona de abajo es en donde vamos a escribir nuestro código de prueba, que va a ser el siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/035e2d27084846d29be404ddb22f6e6aa59ae787/Hacking_%C3%89tico/sqli0.png)

No es más que un alert, en donde nos aparecerá el mensaje 'hola'. Le damos a submit y se crea el topic. Nos vamos a la pestaña principal y vemos nuestro topic creado:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/035e2d27084846d29be404ddb22f6e6aa59ae787/Hacking_%C3%89tico/sqli31.png)

Si le damos al topic, nos aparecerá lo siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/035e2d27084846d29be404ddb22f6e6aa59ae787/Hacking_%C3%89tico/sqli32.png)

Vemos que, efectivamente nos aparece una ventana emergente con el mensaje 'hola'.

Cómo podemos evitar esto? Al igual que antes, es muy importante sanear los datos que introduce el usuario y no concatenar directamente la consulta. 

**TIPOS DE CROSS-SITE SCRIPTING**

En este apartado vamos a ver los 2 tipos de Cross-Site Scripting que existen:

1. **XSS almacenado** -> Es el tipo de Cross-Site Scripting que vimos en el apartado anterior y es aquel en el que la inyección de código se almacena en la base de datos. Quizás sea el más peligroso de los dos. En el apartado anterior, lo que pasó es que nosotros al crear un nuevo topic y escribir el código en uno de sus apartados, este topic con su respectivo código se almacenó en la base de datos y posteriormente cuando le hicimos click encima para verlo, el navegador interpretó ese código como un script y lo ejecutó. El esquema sería el siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/035e2d27084846d29be404ddb22f6e6aa59ae787/Hacking_%C3%89tico/sqli33.png)

En donde tenemos un atacante que inyecta código, se guarda en la base de datos y por último, un usuario que desde su equipo va a cargar la información que hay en la base de datos y su navegador lo interpretará como un script.

2. **XSS reflejado** -> En este caso, el código no se guarda en la base de datos; sino que el atacante realiza la inyección y consigue que el usuario acceda a la página con el código ya inyectado. Vamos a poner como ejemplo, un formulario que te pida tu nombre y te lo muestre. Tu lo que haría sería escribir tu nombre y darle al botón de enviar. Al darle al botón, el formulario ya tiene el código inyectado y te mostraría la cadena 'hola' (si tomamos como ejemplo el script del apartado anterior). Este método ya sería algo más difícil, ya que tendría que ser que el usuario entrase en el formulario, por ejemplo mediante un enlace. El esquema es el siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/035e2d27084846d29be404ddb22f6e6aa59ae787/Hacking_%C3%89tico/sqli34.png)

## Dia 14/04/2024

**ATAQUE :CROSS-SITE SCRIPTING (XXS)**

En apartados anteriores vimos vulnerabilidades en donde como ejemplo, mostrabamos la cadena 'hola' mediante un alert. Esto a priori, parece que no es peligroso, pero vamos a ver qué ocurre si por ejemplo utilizamos la función de javascript **window.location**. Mediante esta función podemos efectuar un 'robo se desión' (en este apartado, el usuario utilizaría 'document.cookie' para extraer las cookies del usuario y de ahí las credenciales de inicio de sesión. Hay que decir que el usuario tiene que estar ya logueado con su cuenta y hay que hacerlo rápido para que la sesión no se cierre y sea todo en vano), 'Phishing' (el atacante redirige al usuario a una página falsa que es idéntica al sitio web que quiere acceder para capturar las credenciales de inicio de desión del usuario) y 'Deface' (este apartado no es más que inutilizar una función de un sitio web. Por ejemplo, si en un formulario de inicio de sesión, en el apartado del nombre le ponemos un window.location con la URL de Google, un administrador del sitio web cuando entre y por ejemplo quiera ver los usuarios, cuando se cargue el script de nuestro nombre, lo llevará a Google. Esto supondría que el administrador tuviera que buscar de donde viene el problema).

Vamos a hacer un ejemplo en el foro. Vamos a crear un nuevo topic, llamado cookie y en donde le vamos a introducir el siguiente código:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/04317c2c5b6a7ece07a2513c16da57628a8b5133/Hacking_%C3%89tico/sqli35.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/04317c2c5b6a7ece07a2513c16da57628a8b5133/Hacking_%C3%89tico/sqli36.png)

Vamos a darle al topic que hemos creado a ver que nos sale:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/04317c2c5b6a7ece07a2513c16da57628a8b5133/Hacking_%C3%89tico/sqli37.png)

Vemos que nos da el ID de inicio de sesión del usuario que está logueado en ese momento. Esto, podríamos redirigirlo a una página web nuestra y guardarlo en un archivo, por ejemplo. Ahora vamos a hacer el ejemplo, de redirigir a Google. Crearíamos un nuevo topic, en mi caso con el nombre 'google' y le pondríamos el siguiente código:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/04317c2c5b6a7ece07a2513c16da57628a8b5133/Hacking_%C3%89tico/sqli38.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/04317c2c5b6a7ece07a2513c16da57628a8b5133/Hacking_%C3%89tico/sqli39.png)

Vamos a ver lo que pasa si hacemos click:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/04317c2c5b6a7ece07a2513c16da57628a8b5133/Hacking_%C3%89tico/sqli40.png)

Nos redirigiría automáticamente a la página web de Google. Ahora mismo, estoy poniendo el código, en la parte de abajo del topic, pero qué pasaría si lo pusiera en el título del topic? La respuesta es sencilla; el foro, en su página principal carga todos los topics con su título, con lo cual, nada más acceder a la página principal se interpretaría ese script como código y se redirigiría a Google. Con esto, estaríamos inutilizando la página principal del foro con un ataque Deface de XSS reflejado.

## _FICHEROS__

**UNRESTRICTED FILE UPLOAD**

En esta lección vamos a ver vulnerabilidades sobre el tratamiento de los ficheros. En este apartado en concreto, vamos a empezar con la subida de ficheros sin restricciones (por ejemplo un formulario en el que haya subida de ficheros, sin restricciones de tamaño, extensión ni tipo).

Los tipos de ataque que nos pueden hacer son:

1. Fichero malicioso ejecutable en servidor (.jsp, .php, etc).
2. Fichero de gran tamaño (puede inutilizar el servidor al colaprsarlo con un fichero demasiado grande).
3. Fichero con nombre/ruta sensible (sobreescribe fichero del sistema).

Para controlar todo esto, deberían restringirse ciertas extensiones o incluso aceptar sólo un tipo de extension, con un tamaño máximo determinado y saneando el nombre (aqui, incluso podrían hacer alguna inyección de código como las vistas en apartados anteriores). Por último, debería descargar el fichero y no guardarlo en el servidor para que éste, si es un fichero malicioso no lo interprete como tal.

**ATAQUE: UNRESTRICTED FILE UPLOAD**

Lo que vamos a hacer en este apartado es un ataque de tipo Unrestricted File Upload. Para ello, necesitamos un formulario que permita la subida de ficheros. Los pasos a ejecurar van a ser los siguientes:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/04317c2c5b6a7ece07a2513c16da57628a8b5133/Hacking_%C3%89tico/ufu1.png)

Para el primer paso, entramos en un formulario super básico en donde sólo te permite la subida de ficheros:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/04317c2c5b6a7ece07a2513c16da57628a8b5133/Hacking_%C3%89tico/ufu2.png)

Ahora tocaría subir un fichero. En el curso, ya nos dan el fichero 'shell.php', que no es más que una consola que nos va a permitir ejecutar comandos sobre el servidor atacado. Dicho esto, subimos el archivo:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/04317c2c5b6a7ece07a2513c16da57628a8b5133/Hacking_%C3%89tico/ufu3.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/04317c2c5b6a7ece07a2513c16da57628a8b5133/Hacking_%C3%89tico/ufu4.png)

Ahora simplemente cambiamos la ruta que tenemos a la que nos dice, y ya podemos entrar en la shell:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/04317c2c5b6a7ece07a2513c16da57628a8b5133/Hacking_%C3%89tico/ufu5.png)

Ya estamos en la shell para poder ejecutar comandos sobre el servidor atacado. Desde aquí, si hacemos por ejemplo un 'ls', podemos ver el contenido de las carpetas del servidor y navegar por ellas:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/04317c2c5b6a7ece07a2513c16da57628a8b5133/Hacking_%C3%89tico/ufu6.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/04317c2c5b6a7ece07a2513c16da57628a8b5133/Hacking_%C3%89tico/ufu7.png)

También podemos crear ficheros, borrar ficheros, crear directorios e incluso tumbar la base de datos o borrar el servidor.

**LOCAL FILE INCLUSION**

En este apartado vamos a tratar la vulnerabilidad LocaL File Inclusion (Manipulación de la ruta a un fichero del servidor que es cargado mediante la aplicación para acceder a otro de forma fraudulenta). Esto se puede evitar, restringiendo el nombre y la ruta a acceder. En el siguiente apartado vamos a ver un ejemplo de ello.

**ATAQUE: LOCAL FILE INCLUSION**

En este apartado vamos a ver un ejemplo de un ataque Local File Inclusion. Para ello, tendremos una web con 3 enlaces diferentes que cargaran archivos diferentes:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/04317c2c5b6a7ece07a2513c16da57628a8b5133/Hacking_%C3%89tico/lfi1.png)

Cada enlace, como ya dije, abre un archivo totalmente diferente. Estas peticiones son de tipo 'get', que son las que se encargan de solicitar recursos como páginas web, imágenes o archivos desde un servidor web. Vamos a acceder al fichero 1 y a ver la URL:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/04317c2c5b6a7ece07a2513c16da57628a8b5133/Hacking_%C3%89tico/lfi2.png)

Vemos que al final de la URL, aparece 'informe=informe1.txt'. Lo que vamos a hacer es cambiar la ruta que va a la derecha del igual e indicarle el archivo que nosotros queramos. En mi caso, en el escritorio, me creo una carpeta llamada nueva con un archivo prueba.txt. La ruta quedaría asi:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/04317c2c5b6a7ece07a2513c16da57628a8b5133/Hacking_%C3%89tico/lfi3.png)

Y se mostraría lo siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/04317c2c5b6a7ece07a2513c16da57628a8b5133/Hacking_%C3%89tico/lfi4.png)

Tendremos acceso a cualquier archivo al cual el usuario del sistema que ha levantado la aplicación tenga permisos de lectura. En el caso que el usuario tuviera permisos de administrador, podriamos ver casi cualquier archivo de todo el servidor.

## _ROBO DE SESIONES__

En este apartado vamos a ver la vulnerabilidad de tipo 'Robo de sesión'. La vulnerabilidad de robo de sesión ocurre cuando un atacante obtiene acceso no autorizado a la sesión activa de un usuario en una aplicación web o sistema.

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f8964d8dbd1089f60fd8f98d4912cc67fadbe993/Hacking_%C3%89tico/robos1.png)

Un ejemplo sería el anterior, en donde un usuario se loguea en un sitio web con un ID de sesion 9976 por ejemplo y otro usuario se loguea en esa misma web con ID de sesión 9977. Si el usuario 1 le suma 1 a su ID de sesión, obtendría el ID de sesión del usuario 2 y accedería a su sesión.

El robo de sesión se evita mediante las siguientes reglas:

1. El ID de sesión debe ser creado en el servidor.
2. El ID debe ser único.
3. El ID debe ser aleatorio y no secuencial como en el ejemplo anterior.
4. El ID debe estar encriptado con algún sistema de encriptación.
5. El ID debe tener un tiempo de expiración para que se cierre automáticamente la sesión al cabo de un tiempo.
6. Solicitar contraseña del usuario actual para cambiar la contraseña.

## _ATAQUE: ROBO DE SESIONES__

En este apartado vamos a efectuar un ejemplo de un robo de sesión. Para ello vamos a utilizar una herramienta llamada [ZAP](https://www.zaproxy.org/download/). Esta herramienta la vamos a utilizar para capturar la petición que el usuario le manda al servidor y de ahí modificar el ID de sesión. En la aplicación, en el menú de opciones tenemos que habilitar la opción de que escuche en localhost y un puerto (ya viene por defecto localhost y el puerto 8080):

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f8964d8dbd1089f60fd8f98d4912cc67fadbe993/Hacking_%C3%89tico/robos2.png)

Ahora mismo, la herramienta está escuchando en ese puerto. Ahora nos vamos a ir a la configuración del navegador, que en mi caso va a ser Firefox y editamos las opciones de conexión del proxy:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f8964d8dbd1089f60fd8f98d4912cc67fadbe993/Hacking_%C3%89tico/robos3.png)

Ahora, nos toca añadirle un certificado SSL al navegador para que el proxy no rechace la conexión. Por defecto, la herramienta OWASP ZAP, crea un certificado SSL que podemos usar o crear otro nuevo. Para ello, nos vamos a Herramientas, Opciones y en las pestaña de Network nos vamos a Server Certificates:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f8964d8dbd1089f60fd8f98d4912cc67fadbe993/Hacking_%C3%89tico/robos4.png)

Y le damos al botón guardar:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f8964d8dbd1089f60fd8f98d4912cc67fadbe993/Hacking_%C3%89tico/robos5.png)

Ahora toca ir al navegador e importar dicho certificado. Para ello nos vamos a las opciones, y en la barra de búsqueda buscamos 'certificados':

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f8964d8dbd1089f60fd8f98d4912cc67fadbe993/Hacking_%C3%89tico/robos6.png)

Le damos a ver certificados y a importar. Vamos a la ubicación en donde lo habiamos guardado previamente y lo subimos:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f8964d8dbd1089f60fd8f98d4912cc67fadbe993/Hacking_%C3%89tico/robos7.png)

Con esto ya configurado, nos vamos a un formulario de prueba muy sencillo que nos dan en el curso:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f8964d8dbd1089f60fd8f98d4912cc67fadbe993/Hacking_%C3%89tico/robos8.png)

En donde simplemente introducimos 'user1' o 'user2' como usuarios y 'pass1' o 'pass2' como contraseña respectivamente y se mostrará el usuario:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f8964d8dbd1089f60fd8f98d4912cc67fadbe993/Hacking_%C3%89tico/robos9.png)

**PROBLEMAS CON EL PROXY (EL PROXY RECHAZA LAS CONEXIONES)**

**ME VEO VIDEO TUTORIAL DE [OWASP ZAP](https://www.google.com/search?q=tutorial+de+zap&sca_esv=ffa6a5b912f672b6&hl=es&source=hp&ei=dZ8mZsK6MO-Ki-gPtpq7sA0&iflsig=ANes7DEAAAAAZiathZMbyFm0zTT4-3UmIKb7yEyPrrIl&ved=0ahUKEwiCyay1rdaFAxVvxQIHHTbNDtYQ4dUDCBg&uact=5&oq=tutorial+de+zap&gs_lp=Egdnd3Mtd2l6Ig90dXRvcmlhbCBkZSB6YXAyBRAAGIAEMgUQABiABDIFEAAYgAQyBRAAGIAEMgUQABiABDIFEAAYgAQyBRAAGIAEMgUQABiABDIFEAAYgAQyBRAAGIAESLo3UABY1jZwDHgAkAEAmAHFAaAB0hmqAQQwLjI2uAEDyAEA-AEBmAImoAKkGqgCCsICCxAAGIAEGLEDGIMBwgIREC4YgAQYsQMY0QMYgwEYxwHCAgsQLhiABBixAxiDAcICCBAAGIAEGLEDwgIIEC4YgAQYsQPCAhQQLhiABBixAxiDARjHARiOBRivAcICDhAAGIAEGLEDGIMBGIoFwgIQEAAYAxjlAhjqAhiMAxiPAcICEBAuGAMY5QIY6gIYjAMYjwGYAwKSBwUxMi4yNqAHgqkB&sclient=gws-wiz#fpstate=ive&vld=cid:d6551148,vid:1InikJ74654,st:0)**

Ahora, vamos a empezar a capturar los datos con la herramienta, por lo tanto nos vamos a ella y le damos al icono verde que hay arriba de todo para empezar a capturar:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f8964d8dbd1089f60fd8f98d4912cc67fadbe993/Hacking_%C3%89tico/robos10.png)

Y nos vamos al formulario y le damos al link que dice 'ver datos':

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f8964d8dbd1089f60fd8f98d4912cc67fadbe993/Hacking_%C3%89tico/robos11.png)

Vemos que se queda cargando, y eso es porque se acaba de producir un punto de interrupción. Nos vamos a la herramienta, y vemos que nos aparece lo siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f8964d8dbd1089f60fd8f98d4912cc67fadbe993/Hacking_%C3%89tico/robos12.png)

En la pestaña de la derecha nos aparecen muchos datos, entre ellos nos aparecen el usuario y contraseña de esa sesión, pero lo más importante es el penúltimo dato que es el ID de sesión. Nosotros estamos logueados como user1, y vemos que el ID de sesión se llama igual que el usuario, pero qué pasa si cambio el ID a user2?:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f8964d8dbd1089f60fd8f98d4912cc67fadbe993/Hacking_%C3%89tico/robos13.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f8964d8dbd1089f60fd8f98d4912cc67fadbe993/Hacking_%C3%89tico/robos14.png)

Hemos accedido a la sesión del usuario 2. Este caso es un ejemplo de lo más simple que puede haber, pero en páginas web más grandes esto puede suponer un problema muy grande y por ello debemos añadirle una capa más de seguridad con las reglas mencionadas anteriormente.


## _FUERZA BRUTA__

En este apartado, a diferencia del anterior en donde capturabamos un ID de sesión, vamos a intentar conseguir las credenciales del usuario. La vulnerabilidad denominada 'Fuerza bruta' es aquel intento de averiguar una contraseña mediante la prueba consecutiva de todas las posibles combinaciones. Los tipos de ataque de fuerza bruta son:

1. **Fuerza bruta** -> Es la forma principal de conseguir las credenciales de un usuario probando todas las combinaciones de contraseñas posibles.
2. **Ataques de diccionario** -> Este tipo utiliza 'diccionarios' por ejemplo de las contraseñas más utilizadas, palabras más utilizadas, nombres, etc y por lo tanto es un conjunto finito de opciones.
3. **Fechas** -> Este tipo es más específico y podría entrar en los ataques de diccionario y comprueba fechas (hay mucha gente que utiliza una fecha como contraseña y le dan el formato de ddmmAAAA por ejemplo). Esto computacionalmente es muy viable ya que es una posiblidad de 1 entre cien millones.

La siguiente imagen sería un resumen básico de lo que sería un ataque por fuerza bruta:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/f8964d8dbd1089f60fd8f98d4912cc67fadbe993/Hacking_%C3%89tico/fb1.png)

Para evitar esto, a parte de una contraseña larga y robusta (longitud adecuada, combinación de caracteres, evitar palabras comunes, aletoriedad, etc), la web podría contar con un contador de reintentos (si introduces mal la contraseña un número de veces x, que se realice algún tipo de acción), un Captcha (asi, no se podrá automatizar un logueo) o un bloqueo de usuario.

## _ATAQUE: FUERZA BRUTA__




