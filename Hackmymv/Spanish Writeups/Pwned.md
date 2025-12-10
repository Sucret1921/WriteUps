# 1. Introducción

En este Writeup explico el proceso y los resultados de la práctica de ciberseguridad realizada sobre la máquina objetivo alojada en la plataforma HackMyVM (máquina: pwned).
El propósito del ejercicio fue aplicar técnicas de enumeración, análisis y explotación en un entorno controlado y autorizado para obtener las «flags» que confirman el compromiso de las cuentas objetivo. El trabajo se realizó con fines académicos y de aprendizaje, respetando el alcance y las normas de la plataforma.

# 2. Desarrollo del tema

Qué he aplicado mediante todo el desarollo he realizado una enumeración completa de la máquina objetivo para identificar servicios y posibles vectores de ataque.
He aplicado técnicas de fuzzing y escaneo para descubrir recursos ocultos o puntos de entrada.
También he tenido que acceder a servicios (FTP/SSH) y, con la información obtenida, escalar privilegios hasta obtener las flags previstas por la práctica.

# 2.1 Metodología

Para tener un orden de como enfocar las fases ha sido de la siguiente manera: reconocimiento → enumeración → explotación inicial → post-explotación/escalado → limpieza y documentación.
Herramientas utilizadas 
    • Herramientas de escaneo y enumeración de red y servicios (nmap)
    • Herramientas de fuzzing/web discovery para encontrar rutas/directorios(wFuzz)
    • Clientes FTP/SSH para interactuar con los servicios identificados. (FTP,id_rsa)

***

<img width="1200" height="649" alt="image" src="https://github.com/user-attachments/assets/abeac154-51b7-41ed-9830-cc155bbc84d9" />

> Captura de reconocimiento de la red con nmap para buscar la ip de la máquina (192.168.1.31).

<img width="914" height="838" alt="image" src="https://github.com/user-attachments/assets/552113f7-88d3-4095-a0fd-e3ffe2788495" />

> Captura buscando directorios ocultos para ver si hay alguna pista mediante

wFuzz tendremos que poner un diccionario cogeremos uno de los que ya existen
estándar.

<img width="800" height="485" alt="image" src="https://github.com/user-attachments/assets/a7cc251d-a00f-4017-9555-1e70ebb1c7f9" />

> Captura del diccionario secreto encontrado mediante hacer fuzzing a los directorios de la máquina

En la captura anterior podemos ver que un directorio oculto de los que detecta el diccionario es “hidden_text” que esconde un diccionario secreto, lo descargaremos y lo ejecutaremos con el wFuzz.

<img width="922" height="420" alt="image" src="https://github.com/user-attachments/assets/49e9e558-0ea3-4054-a782-5e63adc07047" />

> Captura del wFuzz con el diccionario encontrado, nos encuentra un nuevo directorio nuevo.

<img width="962" height="269" alt="image" src="https://github.com/user-attachments/assets/d7d8a4c1-9639-4c2c-911a-40b2dfece37a" />

> Captura del nuevo directorio de la máquina con labels de usuario y contraseña para poder enviar una consulta

Ponemos el nuevo directorio “pwnd.vuln” y podremos ver que pone un usuario y una contraseña y un botón para poner la consulta, al no encontrar nada inspecciono la página y me encuentro con los siguientes datos.

<img width="973" height="407" alt="image" src="https://github.com/user-attachments/assets/a34fd4d4-d104-4311-86fc-5e8a41f46796" />

> Captura de inspeccionar la página del navegador, con código php dentro de un comentario

Podemos apreciar que comentado hay una linea de “?php” por lo cual fijandose en el codigo da un usuario de ftp con una contraseña, anteriormente pudimos ver que el puerto 21 que es el protocolo ftp esta abierto entonces procederemos a entrar mediante ftp con línea de comando con el usuario y la contraseña que se ven.


<img width="918" height="744" alt="image" src="https://github.com/user-attachments/assets/9284d895-058d-400b-876f-faa5a600f85d" />

> Captura de la conexión al ftp de la máquina y despliegue de todos los comandos.

Lo que he hecho a resumidas cuentas en la máquina a sido acceder mediante ftp con la ip de la máquina a este he podido entrar gracias a la información anterior que estaba en el codigo php, dentro hago ls para poder ver donde estoy y encuentro que hay un directorio que se llama “/share” entro y me encuentro dos ficheros un “note.txt” donde habla de una tal “ariana”, en el otro fichero encuentro que hay un “id_rsa” que es una certificado de ssh con la contraseña para entrar.

<img width="875" height="569" alt="image" src="https://github.com/user-attachments/assets/6e82a1ba-6b76-4c28-ad21-2ff306035a39" />

> Captura entrando al usuario ssh de adriana y encontrando información relevante

Como hemos podido ver anteriormente al descargar el id.rsa sabiamos que era un certificado ssh entonces solo nos faltaria el usuario que la “nota.txt” nos daba la pista que era ariana, probé a entrar con estos datos mediante el comando de “ssh [nombre]@[ip] -i id_rsa” y pude entrar.
Una vez dentro del usuario indage por los directorios y pude encontrar el diario personal de Ariana, que esta deja una pista importante de que habia otro usuario que se llamaba Selena (aunque esto se podia ver igual si vas al “/home” se puede ver).
En el otro fichero del “user1.txt” podemos encontrar la flag del usuario y ahora empezaremos con la búsqueda del usuario root.

<img width="673" height="78" alt="image" src="https://github.com/user-attachments/assets/48e7dfa3-c157-4d6e-ac39-897a4f215335" />

> Captura del comando entrando como sudo con Selena al script de messenger.sh

En el directorio del /”home” hay un script “messenger.sh” que podriamos ejecutar con “sudo -u” de esta forma podremos ejecutar el script como si estuviermos desde el usuario de Selena.

<img width="919" height="395" alt="image" src="https://github.com/user-attachments/assets/d76f902e-98d0-426b-a92e-901e824f54a7" />

> Captura dentro del script con la ejecución de los comandos bash

El script que estamos ejecutando tiene una vulnerabilidad porque cuando nosotros enviamos un mensaje al remitente pero con la orden de ejecutarlo como Selena estaremos dentro de la shell de Selena, es decir que si abriamos en el mensaje que enviamos mandamos un “/bin/bash “podremos ejecutar dentro del script los comandos de este, en mi caso pongo un id para asegurarme que estamos como Selena y seguidamente procedo con un “script /dev/null -c bash” basicamente en este comando abro una nueva shell sin historial de comandos no se registrara nada y seguido con “-c bash” le dire que abra una nueva shell en este caso como hemos confirmado en la de Selena.

<img width="912" height="318" alt="image" src="https://github.com/user-attachments/assets/cbf2ee3d-74c0-428f-b69e-75fc1ab44825" />

> Captura indagando en los directorios de /home/selena

Una vez hemos entrado podemos ver que en el “/home” de Selena se puede ver nuevos archivos uno con su diario personal, donde se confirma la clave ftp que vimos anteriormente era culpa de esta. I un archivo más “user2.txt” donde hay la misma flag anterior que estaba con la de Ariana y ahora sep uede ver que dice que estas más cerca de encontrar al supuesto hacker.

<img width="906" height="220" alt="image" src="https://github.com/user-attachments/assets/144e52e1-a46b-46fb-9c3b-e8a7d8ec388d" />

> Captura que encuentro el grupo docker y ejecuto el script para encontrar que es root

Luego de estar buscando un rato una posible pista porque ninguno de los 2 archivos ayudo realmente a seguir probé a buscar en los grupos que estaba selena y fue interesante porqué estaba en el “docker group” de esta forma buscando encontre que con el siguente comando podemos entrar como root del sistema, aprovechando esta vulnerabilidad de docker. 

``` docker run -v /:/mnt --rm -it alpine chroot /mnt sh ```

<img width="971" height="506" alt="image" src="https://github.com/user-attachments/assets/4c4478da-2961-4451-82bc-29643ef66514" />

> Captura extraida de https://gtfobins.github.io/gtfobins/docker/ en el apartado de shell con ./docker

# 4. Referencias (formato APA)

1. GTFOBins. (s. f.). Docker — shell. GTFOBins. (https://gtfobins.github.io/gtfobins/docker/)
2. HackMyVM. (s. f.). Plataforma de máquinas virtuales para hacking ético. HackMyVM. (https://hackmyvm.eu/)
3. Wfuzz. (s. f.). Wfuzz — Documentation. Read the Docs. (https://wfuzz.readthedocs.io/en/latest/)







