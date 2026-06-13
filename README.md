#!/bin/bash

NODOS=("gluster1" "gluster2" "gluster3")

for nodo in "${NODOS[@]}"
do
    ssh $nodo '
        sudo apt update -y
        sudo apt install -y glusterfs-server

        sudo bash -c "cat >> /etc/hosts << EOF
192.168.1.115 gluster1
192.168.1.116 gluster2
192.168.1.117 gluster3
EOF"

        sudo systemctl enable glusterd
        sudo systemctl start glusterd
    '
done

sleep 10

gluster peer probe gluster2
gluster peer probe gluster3

gluster peer status
