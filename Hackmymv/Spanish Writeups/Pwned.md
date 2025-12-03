1. Introducción

En esta práctica explico el proceso y los resultados de la práctica de ciberseguridad realizada sobre la máquina objetivo alojada en la plataforma HackMyVM (máquina: pwned).
El propósito del ejercicio fue aplicar técnicas de enumeración, análisis y explotación en un entorno controlado y autorizado para obtener las «flags» que confirman el compromiso de las cuentas objetivo. El trabajo se realizó con fines académicos y de aprendizaje, respetando el alcance y las normas de la plataforma.
2. Desarrollo del tema

Qué he aplicado mediante todo el desarollo he realizado una enumeración completa de la máquina objetivo para identificar servicios y posibles vectores de ataque.
He aplicado técnicas de fuzzing y escaneo para descubrir recursos ocultos o puntos de entrada.
También he tenido que acceder a servicios (FTP/SSH) y, con la información obtenida, escalar privilegios hasta obtener las flags previstas por la práctica.

2.1 Metodología

Para tener un orden de como enfocar las fases ha sido de la siguiente manera: reconocimiento → enumeración → explotación inicial → post-explotación/escalado → limpieza y documentación.
Herramientas utilizadas 
    • Herramientas de escaneo y enumeración de red y servicios (nmap)
    • Herramientas de fuzzing/web discovery para encontrar rutas/directorios(wFuzz)
    • Clientes FTP/SSH para interactuar con los servicios identificados. (FTP,id_rsa)





