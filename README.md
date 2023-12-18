# ESPECIALISTA EN HACKING ETICO
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

Para este punto, necesitaremos una herramienta llamada MUICacheView. La descargamos en nuestra máquina (en mi caso la descargo de la web de nirsfot) y la abrimos. Este es su aspecto al abrirla:

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

Ahora nos iriamos a la carpeta en donde nos hemos encontrado estos archivos y los intentariamos abrir **con permisos de administrador** con un programa llamado "Windows File Analyzer" (WFA Thumbs).La interfaz del programa seria la siguiente:

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

Para todo ello, utilizaremos una herramienta llamada "Dumpit", que hará un volcado de la memoria en formato .raw y del mismo tamaño que la memoria actual.La herramienta la lanzaremos **en modo administrador** y la apariencia es la siguiente:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ef531b1305171f2f4fa2b7371604670e17a5abd7/Hacking_%C3%89tico/volatil1.png)

En donde escribiremos la "y" y se nos hará el volcado en la carpeta en donde pone Destination:

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ef531b1305171f2f4fa2b7371604670e17a5abd7/Hacking_%C3%89tico/volatil2.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ef531b1305171f2f4fa2b7371604670e17a5abd7/Hacking_%C3%89tico/volatil3.png)

Vemos que, efectivamente, el archivo .raw es del tamaño de la RAM de la máquina virtual (3GB):

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ef531b1305171f2f4fa2b7371604670e17a5abd7/Hacking_%C3%89tico/volatil4.png)

![](https://github.com/emiliogf10/Proyecto-FDC/blob/ef531b1305171f2f4fa2b7371604670e17a5abd7/Hacking_%C3%89tico/volatil5.png)

Con el archivo .raw creado, solo faltaria analizarlo con herramientas forenses como Volatility o WinDbg.

## _CAPTURA DE LA MEMORIA DE PAGINACIÓN_


