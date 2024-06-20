# reseau-mentor

## Partie 1 : Setup du patron (4 points) 
`VBoxManage createvm --name "Patron" --register`

`VBoxManage modifyvm "Patron" --nic1 nat`
`VBoxManage modifyvm "Patron" --nic2 hostonly`

`VBoxManage storageattach "Patron" --storagectl "IDE" --port 0 --device 0 --type dvddrive --medium D:\Rocky-9.4-x86_64-minimal `

`sudo vi /etc/sysconfig/network-scripts/ifcfg-enp0s3`

`BOOTPROTO=static`
`IPADDR=10.0.2.15`
`NETMASK=255.255.255.0`

`sudo systemctl restart network`

`sudo hostnamectl set-hostname patron`

`ssh-keygen -t rsa`

`ssh-copy-id Patron@10.0.2.15`

# Partie 2 : Premier test réseau (4 points)

`sudo hostnamectl set-hostname etudiant`
`sudo hostnamectl set-hostname mentor`    
`sudo cat /etc/sysconfig/network-scripts/ifcfg-enp0s3`

`sudo vi /etc/sysconfig/network-scripts/ifcfg-enp0s3`

# pour etudiant 

`TYPE=Ethernet`
`BOOTPROTO=static`
`IPADDR=10.1.1.10`
`NETMASK=255.255.255.0`
`ONBOOT=yes`


# pour mentor

`TYPE=Ethernet`
`BOOTPROTO=static`
`IPADDR=10.1.1.11`
`NETMASK=255.255.255.0`
`ONBOOT=yes`

`ping 10.1.1.11  etudiant a mentor`

`ping 10.1.1.10 mentor a etudiant`


## Partie 3 : Un petit peu de routage (6 points)
Pour routeur :
Supprimez la carte réseau Host-Only des VM mentor et etudiant.
Configurez les interfaces réseau pour routeur :
LAN 1:
`sudo vi /etc/sysconfig/network-scripts/ifcfg-enp0s3`

`TYPE=Ethernet`
`BOOTPROTO=static`
`IPADDR=10.1.1.254`
`NETMASK=255.255.255.0`
`ONBOOT=yes`

LAN 2: 

`sudo vi /etc/sysconfig/network-scripts/ifcfg-enp0s8`

`TYPE=Ethernet`
`BOOTPROTO=static`
`IPADDR=10.1.2.254`
`NETMASK=255.255.255.0`
`ONBOOT=yes`

`sudo systemctl restart network`

Configurer le routeur pour qu'il puisse faire le lien entre les deux réseaux

`sudo vi /etc/sysctl.conf`

`net.ipv4.ip_forward = 1`

`sudo sysctl -p`

Testez la connectivité entre les machine

`ping 10.1.2.254`
`ping 10.1.1.254`
`ping 10.1.1.11`

Tester la connectivité entre les machines

`ping 10.1.1.254  "etudiant" vers "routeur" `
`ping 10.1.1.254  "mentor" vers "routeur"`
`ping 192.168.57.12  "etudiant" vers "mentor"`









