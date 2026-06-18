
Crear un sistema de almacenamiento distribuido, altamente disponible y escalable para que se puedan manejar grandes volúmenes de datos de manera confiable y eficiente. Esta arquitectura de referencia contiene los componentes de infraestructura necesarios para un sistema de archivos de red distribuido. Contiene tres instancias con hardware dedicado, que es el mínimo necesario para configurar una alta disponibilidad para GlusterFS. En una configuración de tres servidores, al menos dos servidores deben estar en línea para permitir operaciones de escritura en el cluster. Los datos se replican en todos los nodos.

En este informe se puede ver como con Glusterfs se puede mantener la disponibilidad de los datos ante la caída de un nodo por medio de la replicación; y cuando este vuelve a estar disponible el sistema realiza la recuperación y la sincronización de la información.

Objetivo Principales

Alta disponibilidad:

Mantener los datos accesibles incluso si el nodo falla.
Reduzca los puntos únicos de fallo.
Tolerancia a fallo:

Protege los datos mediante la réplica o dispersión entre varios nodos.
Centralización de almacenamiento:

Unifica el espacio de almacenamiento de múltiples servidores en un único sistema de archivos.
Arquitectura Cliente-Servidor:

Los servidores exportan bancos de almacenamiento de múltiples servidores en un único sistema de archivos.
Flexibilidad en volúmenes:

Volumen distribuido: Maximiza la capacidad de almacenamiento disponible, no ofrece redundancia.
volumen replicados: Proporciona alta disponibilidad. Si un nodo falla, los datos siguen disponibles en los demás nodos.
Volumen disperso: Divide la información en fragmentos, añade bloques de paridad para recuperar datos.
Requisitos del sistema para configurar GlusterFS

Nodos: se necesita un mínimo de dos (2) hosts para tolerancia a fallo, pero se recomiendan tres (3) para configuraciones de alta disponibilidad.
Almacenamiento en (ladrillo): es el dispositivo de almacenamiento que se utiliza para GlusterFS debe contar con una capacidad de al menos 25 GB.
Memoria (RAM): 8 GB por servidor.
Formato de disco: el dispositivo de almacenamiento que utiliza Glusterfs debe ser un disco si formato. No debe ser formateado, ni particionado.
Despliegue de GlusterFS en tres (3) máquinas virtuales, simulación de caída y recuperación del nodo.

Los tres nodos

imagen
