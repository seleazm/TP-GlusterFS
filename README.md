---
- name: Configurar servidores GlusterFS
  hosts: gluster
  become: yes

  vars:
    gluster_nodos:
      - { ip: "192.168.1.115", name: "gluster1" }
      - { ip: "192.168.1.116", name: "gluster2" }
      - { ip: "192.168.1.117", name: "gluster3" }
 tasks:

    - name: Actualizar repositorios
      apt:
      update_cache: yes

   # instalación de gluster    
- name: Instalar GlusterFS 
      apt:
        name: glusterfs-server
        state: present 
        
   - name: Iniciar y habilitar el servicio GlusterFS
      service:
        name: glusterd
        state: started
        enabled: yes

    - name: Agregar nodos al archivo /etc/hosts
      lineinfile:                                            #módulo que permite agregar o modificar líneas en un archivo.
        path: /etc/hosts
        line: "{{ item.ip }} {{ item.name }}"                #Agrega una línea con la dirección IP y el nombre del nodo
        state: present
      loop: "{{ gluster_nodos }}"

    - name: crear el clúster GlusterFS
      hosts: master
      become: yes

      tasks:

    - name: Esperar disponiblidad del servicio
      wait_for:                                             #el playbook espera hasta que el servicio responda en el puerto 24007 antes de continuar
        port: 24007
        timeout: 20                                         #Espera un máximo de 20 segundos; si el puerto no está disponible, la tarea falla
    
     - name: Conectar gluster2 al clúster
      command: gluster peer probe gluster2
      register: peer gluster2
      failed_when: false                                              #Evita que el playbook falle si el comando devuelve un error
      changed_when: "'success' in peergluster2.stdout.lower()"

    - name: Conectar gluster3 al clúster
      command: gluster peer probe gluster3
      register: peer gluster3
      failed_when: false
      changed_when: "'success' in peergluster3.stdout.lower()"

    - name: Mostrar estado del clúster
      command: gluster peer status
      register: status
    

