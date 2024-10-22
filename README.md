# Comparación de Máquinas Virtuales y Contenedores: Vagrant y Docker

Este repositorio contiene los archivos necesarios para ejecutar una prueba comparativa entre servidores basados en máquinas virtuales y servidores basados en contenedores utilizando Vagrant y Docker
#Descripción del Proyecto
El objetivo de este proyecto es comparar el rendimiento de un servidor web configurado en una máquina virtual utilizando Vagrant con un servidor web en un contenedor Docker, ambos ejecutando el servidor NGINX. 
El cliente realiza una prueba de carga utilizando la herramienta wrk para generar solicitudes al servidor y medir el rendimiento en términos de CPU y memoria.
Componentes Principales:
Vagrant para el Servidor Web (VM): Se configura una máquina virtual con Ubuntu 18.04 y NGINX utilizando Vagrant.
Docker para el Servidor Web: Se levanta un contenedor con NGINX utilizando Docker, comparando su rendimiento con la máquina virtual.
Vagrant para el Cliente: El cliente también se levanta utilizando Vagrant, en una máquina que instala y ejecuta wrk, la herramienta de prueba de carga
Configuración del Servidor con Vagrant
El servidor basado en VM utiliza NGINX para servir una página simple. Aquí puedes encontrar el Vagrantfile del servidor VM.

Configuración del Servidor con Docker
Se configuró un contenedor Docker para servir la misma página web a través de NGINX, permitiendo realizar la comparación de rendimiento. 

Cliente para Pruebas de Carga
El cliente utiliza wrk, una herramienta de pruebas de carga, para enviar múltiples solicitudes al servidor y medir su rendimiento en términos de CPU y uso de memoria. El cliente está configurado en su propio Vagrantfile. 

Análisis de Rendimiento del Servidor en VM (Vagrant)
A continuación se detalla un análisis de los datos obtenidos durante la ejecución de la prueba de carga utilizando el servidor NGINX en una máquina virtual configurada con Vagrant. Los resultados nos permiten evaluar tanto el rendimiento en términos de solicitudes por segundo como la utilización de recursos (CPU y memoria).

1. Datos de la Prueba de Carga
3228022 requests in 3.00m, 855.66MB read
Requests/sec:  17924.05
Transfer/sec:      4.75MB
Solicitudes Totales: El servidor procesó 3,228,022 solicitudes en un lapso de 3 minutos.
Solicitudes por Segundo: La tasa de solicitudes por segundo fue de 17,924.05 requests/sec, lo cual es un rendimiento alto y significativo, mostrando que el servidor NGINX en una máquina virtual puede manejar cargas elevadas de manera eficiente.
Transferencia de Datos: Se transfirieron 855.66 MB en total durante la prueba, con una tasa de transferencia promedio de 4.75 MB/s, lo que también es un buen indicador de la capacidad del servidor para servir contenido a gran velocidad.
2. Datos de Utilización de Recursos (VM)
%Cpu(s):  0.2 us, 44.5 sy,  0.0 ni,  4.9 id,  0.0 wa,  0.0 hi, 50.4 si,  0.0 st
KiB Mem :   492420 total,     6584 free,    93972 used,   391864 buff/cache
Utilización de CPU:

El servidor mostró una alta utilización de CPU en modo "system" (sy) con un 44.5%. Esto indica que una gran parte del procesamiento de las solicitudes HTTP fue manejado a nivel del sistema, ya que las solicitudes requieren una gestión intensiva de recursos del sistema operativo.
El 50.4% de la CPU fue utilizado en "soft interrupts" (si), lo que indica que gran parte del procesamiento se realizó para manejar interrupciones suaves relacionadas con la red y otras tareas.
Un bajo 4.9% del CPU estuvo idle (sin utilizarse), lo que sugiere que casi toda la capacidad de procesamiento estuvo en uso durante la prueba.
Memoria:

De los 492,420 KiB de memoria total, sólo 6,584 KiB permanecieron libres, lo que indica que el servidor utilizó casi toda la memoria disponible, con 93,972 KiB en uso activo y el resto en caché de buffers.
No se observó uso de swap, lo cual es positivo, ya que indica que el sistema no tuvo que recurrir a la memoria secundaria para manejar la carga.
3. Rendimiento del Servidor
El servidor NGINX en una VM de Vagrant mostró un rendimiento sólido, capaz de procesar cerca de 18,000 solicitudes por segundo. Esto es indicativo de una alta eficiencia en la gestión de múltiples conexiones concurrentes. La alta utilización de CPU en system y soft interrupts señala que el sistema estuvo bajo una carga significativa, pero aún así logró responder de manera rápida y eficiente.

Consideraciones:
Capacidad de Escalado: Dado que el servidor en VM mostró un uso de CPU cercano al límite (casi 95% de utilización), puede que una mayor carga o duración de la prueba implique una saturación de recursos, lo que podría llevar a un descenso en el rendimiento o a un aumento en la latencia de las respuestas.
Memoria: La memoria disponible fue suficiente para esta prueba, pero en cargas más pesadas, puede ser necesario aumentar la capacidad de RAM para evitar cuellos de botella relacionados con la memoria.
Interrupciones de Red: El porcentaje elevado de tiempo dedicado a soft interrupts sugiere que la gestión de las conexiones de red representó una parte significativa del trabajo, lo que podría indicar una posible limitación para manejar cargas aún mayores sin una mejora en la capacidad de red.






Análisis de Rendimiento del Servidor en Docker
A continuación, se presenta el análisis de los datos obtenidos durante la ejecución de la prueba de carga utilizando el servidor NGINX ejecutado dentro de un contenedor Docker. Al comparar los resultados con los obtenidos en la VM (Vagrant), podemos observar las diferencias en términos de rendimiento y uso de recursos.

1. Datos de la Prueba de Carga
126436 requests in 3.00m, 33.40MB read
Requests/sec:    702.33
Transfer/sec:    189.99KB
2. Datos de Utilización de Recursos (Docker)
%Cpu(s):  0.7 us,  6.0 sy,  0.0 ni,  1.3 id,  0.0 wa,  0.0 hi, 91.9 si,  0.0 st
KiB Mem :   492420 total,     6184 free,   146708 used,   339528 buff/cache
Utilización de CPU:

El servidor mostró una utilización del 91.9% en soft interrupts (si), lo que indica que gran parte de los recursos se dedicaron a la gestión de interrupciones de red.
Solo un 6% de la CPU fue utilizado en modo "system" (sy), lo cual es mucho menor que en la VM, donde este porcentaje era significativamente más alto.
A diferencia de la VM, en Docker la CPU mostró un 1.3% de inactividad (idle), lo que sugiere que hubo algo de capacidad de procesamiento libre.
Memoria:

En este caso, la memoria total disponible fue similar a la de la VM (492,420 KiB). Sin embargo, el contenedor Docker utilizó más memoria activa (146,708 KiB) en comparación con la VM.
339,528 KiB se utilizaron en caché de buffers, lo cual es similar a los resultados de Vagrant.
No hubo uso de swap, lo cual es positivo y sugiere que la memoria física disponible fue suficiente.
3. Rendimiento del Servidor
El servidor NGINX en Docker procesó alrededor de 702 solicitudes por segundo, lo que representa un rendimiento mucho menor en comparación con las 17,924 solicitudes por segundo en la VM de Vagrant. A pesar de que la CPU estuvo ocupada principalmente manejando interrupciones suaves, la baja utilización del modo "system" indica que el contenedor Docker es más eficiente en términos de consumo de recursos del sistema operativo.

Consideraciones:
Capacidad de Escalado: El contenedor Docker manejó significativamente menos solicitudes por segundo, lo que podría estar relacionado con la forma en que Docker gestiona las conexiones de red y el procesamiento de solicitudes. A pesar de utilizar menos recursos de CPU en el modo "system", la mayor parte del procesamiento se centró en las interrupciones de red, lo que podría ser un cuello de botella en escenarios de alta concurrencia.
Memoria: Docker utilizó más memoria activa en comparación con la VM, lo que puede ser indicativo de una mayor optimización de la memoria interna del contenedor. Sin embargo, a pesar de esto, el rendimiento en términos de solicitudes por segundo fue menor.
Interrupciones de Red: La mayor parte del tiempo de CPU se dedicó a gestionar interrupciones relacionadas con la red, lo que sugiere que Docker puede tener una mayor sobrecarga en la gestión de redes en comparación con las máquinas virtuales tradicionales.
Comparación General:
Rendimiento: La VM de Vagrant mostró un rendimiento mucho más alto en términos de solicitudes por segundo y tasa de transferencia de datos, lo que sugiere que es más adecuada para manejar grandes volúmenes de tráfico.
Eficiencia de Recursos: Docker fue más eficiente en el uso de los recursos del sistema operativo, con un menor porcentaje de uso de la CPU en modo "system". Sin embargo, este ahorro en recursos parece haber afectado su rendimiento en términos de procesamiento de solicitudes.
Conclusión: Para cargas de trabajo intensivas en solicitudes, las VM pueden ofrecer un mejor rendimiento general. Docker, aunque más ligero en términos de consumo de recursos, podría no manejar bien cargas pesadas en entornos de red intensivos como un servidor web bajo alta concurrencia.


LINK DE LA PRESENTACION
https://www.canva.com/design/DAGUTsnvItE/7js34Jzznd2eukKOtfn50Q/edit?utm_content=DAGUTsnvItE&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton

