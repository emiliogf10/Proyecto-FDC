# CARRERA DE ESPECIALISTA EN HACKING ETICO

# PRIMER CURSO : ANÁLISIS FORENSE BÁSICO EN SISTEMAS WINDOWS (Tiempo de realización: 5-6 dias aproximadamente)

## Dia 14/12/2023

Para esta carrera, opto por empezar creando una máquina virtual Windows 10 en Virtual Box. Con la máquina ya creada y configurada con las Guest adittions, configuración de red y demás, empiezo por el primer punto del primer curso:

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

   

## _CAPTURA MBR_

