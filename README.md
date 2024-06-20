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

`ssh-copy-id utilisateur@ip`

# Partie 2 : Premier test réseau (4 points)

`sudo hostnamectl set-hostname etudiant`
`sudo hostnamectl set-hostname mentor`    

`sudo vi /etc/sysconfig/network-scripts/ifcfg-enp0s3`

# pour etudiant 

`BOOTPROTO=static`
`IPADDR=192.168.56.11`
`NETMASK=255.255.255.0`

# pour mentor
`BOOTPROTO=static`
`IPADDR=192.168.56.12`
`NETMASK=255.255.255.0`


`sudo systemctl restart network`

`ping 192.168.56.12  etudiant a mentor`

`ping 192.168.56.11 mentor a etudiant`


## Partie 3 : Un petit peu de routage (6 points)

`sudo vi /etc/sysconfig/network-scripts/ifcfg-enp0s3`

`BOOTPROTO=static`
`IPADDR=192.168.56.1`
`NETMASK=255.255.255.0`

`sudo vi /etc/sysconfig/network-scripts/ifcfg-enp0s8`

`BOOTPROTO=static`
`IPADDR=192.168.57.1`
`NETMASK=255.255.255.0`

`sudo systemctl restart network`

`Configurer le routeur pour qu'il puisse faire le lien entre les deux réseaux`

`sudo vi /etc/sysctl.conf`

`net.ipv4.ip_forward = 1`

`sudo sysctl -p`

`Tester la connectivité entre les machines`

`ping 192.168.56.1  "etudiant" vers "routeur" `
`ping 192.168.57.1  "mentor" vers "routeur"`
`ping 192.168.57.12  "etudiant" vers "mentor"`








