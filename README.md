# Introducción
   Crear un sistema de almacenamiento distribuido, altamente disponible y escalable para que se puedan manejar grande volúmenes de datos de manera 
   confiable y eficiente.Esta arquitectura de referencia contiene los componentes de infraestructura
necesarios para un sistema de archivos de red distribuido. Contiene tres instancias con hardware dedicado, que es el mínimo necesario para configurar una alta disponibilidad para GlusterFS.
 En una configuración de tres servidores, al menos dos servidores deben estar en línea para permitir operaciones de escritura en el cluster. Los datos se replican en todos los nodos.
   
   En este informe se puede ver como con Glusterfs se puede mantener la disponibilidad de los datos ante la caída de un nodo por medio de la replicación; y cuando este vuelve a estar disponible el sistema realiza la recuperación y la sincronización de la información. 




# Objetivo Principales 

Alta disponibilidad:
  * Mantener los datos accesibles incluso si el  nodo falla.
  * Reducir los puntos únicos de fallo.

Tolerancia a fallo:
  * Protege los datos mediante la replica o dispersión entre varios nodos.

Centralización de almacenamiento:
  * Unifica el espacio de almacenamiento de múltiples servidoresen un únicosistema de archivos.

Arquitectura Cliente-Servidor:
  * Los servidoresexportanboquesde almacenamiento de múltiples servidoresen un único sistema de archivos.

Flexibilidad en volúmenes:
  * volumen distribuido: Maximiza la capacidad de almacenamiento disponible, no ofrece redundancia.
  * volumen replicados: Proporciona alta disponibilidad. Si un nodo falla, los datos siguen dispo-
                        niblesen los demás nodos.
  * volumen disperso: Diivide la información en fragmentos, añade bloques de paridad para recuperar
                      datos. 


 
#  Requisitos del Sistema para configurar GlusterFS
   * Nodos: se necesita minimo dos (2) hosts para tolerancia a fallo, pero se recomienda tres (3) para 
         configuraciones de alta disponibilidad. 
   * Almacenamiento en (brick): es el dispositivo de almacenamiento que se utiliza para GlusterFS debe contar 
         con una capacidad de al menos 25 GB.
   * Memoria (RAM): 8 GB por servidor.
   * Formato de disco: el dispositivo de almacenamiento que utiliza Glusterfs debe ser un disco si formato.
     No debe ser formateado, ni particionados.





 # Despliegue de GlusterFS en tres (3) máquinas virtuales, simulación de caída y recuperación del nodo. 
  
   Los tres nodos

  
<img width="649" height="142" alt="image" src="https://github.com/user-attachments/assets/990f13b1-3d26-4f86-b1fd-4e1fa66ab5ff" />




       
  Iniciar y habiliatar el servicio GlusterFS


            
<img width="645" height="368" alt="image" src="https://github.com/user-attachments/assets/ae1f2802-fdbb-4065-bc4a-431ec1396106" />





 #  Configurar resolución de nombres        

    Para la configuración de los nodos de GlusterFS se debe configurar el archivo /etc/hosts de cada máquina virtual.
   También, se puede automatiza completa del nodo maestro. Si el nodo maestro tiene acceso SSH a los otros nodos mediante la clave pública 
   puede automatizar todo el proceso.

   


   Se configura la conectividad entre nodos

   <img width="594" height="112" alt="image" src="https://github.com/user-attachments/assets/c584a123-f0f1-4c29-8bc9-7b8db19e361a" />

   <img width="648" height="129" alt="image" src="https://github.com/user-attachments/assets/294bff39-d7c1-47a1-a127-75babe14d9c6" />




  Añadir el peer desde el nodo maestro

   <img width="515" height="97" alt="image" src="https://github.com/user-attachments/assets/b0257e80-f344-4d55-8410-70a5d3171119" />




  Verificar el estado del Clúster

<img width="484" height="215" alt="image" src="https://github.com/user-attachments/assets/0ce9777c-e2d4-442e-b5f4-c1a8246e97c8" />




  
  Crear directorio brick

   <img width="511" height="70" alt="image" src="https://github.com/user-attachments/assets/6beba92b-e93e-48f1-8733-85fe970ef708" />




   Crear un volumen replicado 

   <img width="650" height="100" alt="image" src="https://github.com/user-attachments/assets/04cf77c8-08ad-4601-97e4-e79d48c8d64e" />




    Iniciar el volumen 


<img width="528" height="65" alt="image" src="https://github.com/user-attachments/assets/e0171ec2-e6c5-4a10-8b5e-5a3fda941946" />

    



    Verificación del volumen 

   
   <img width="522" height="363" alt="image" src="https://github.com/user-attachments/assets/0230f9ae-8a46-4e39-9ca4-f006efac0d58" />





 
     Montar el volumen cliente

   

    
<img width="649" height="61" alt="image" src="https://github.com/user-attachments/assets/8524e1ae-3aca-4de4-887a-301c2bcc347d" />





    
     Comprobación de que el montaje se creo 


    
<img width="652" height="258" alt="image" src="https://github.com/user-attachments/assets/d6c5c59d-23d4-467f-96b6-147b795267ab" />





     Simulación de la caída de un nodo
 
       Apago el nodo 3  (gluster3)  

       
   

<img width="488" height="64" alt="image" src="https://github.com/user-attachments/assets/fc554273-b0b1-454f-a28d-498665272dca" />


   

 
     Verificación del estado del nodo 3 desde el nodo 1


   
<img width="465" height="230" alt="image" src="https://github.com/user-attachments/assets/8cca9b5c-235e-44b8-89b2-ce38436d7163" />




 
     Recuperación del nodo 

      Con sudo systemctl start glusterd se enciende el nodo

      Con sudo systemctl status glusterd  se comprueba que el nodo esta encendido.

     
  <img width="651" height="381" alt="image" src="https://github.com/user-attachments/assets/2d5fcb8a-a03a-447c-8fd8-35cd05111b5c" />




     
     Verificación de reintegración del nodo3 

      Se realiza desde el nodo 1 

 <img width="485" height="235" alt="image" src="https://github.com/user-attachments/assets/4adfcaa5-5299-4e2f-86f8-8af23942767c" />




    
     Verificación auto-heal 

       Consulta el estado del nodo


<img width="583" height="259" alt="image" src="https://github.com/user-attachments/assets/41cf90b3-a43d-4706-8c5c-8ffc81d9b00c" />

    

