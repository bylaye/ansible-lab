# ansible-lab
mini Lab deploiement ansible

Ansible est un outil puissant qui permet d’automatiser l’administration des serveurs notamment:
* **le système d’exploitation (OS)**
* **les applications**
* **la configuration réseau**

Ansible permet de gérer **l’infrastructure avec du code (IaaC)**

## Environnement du mini lab

* virtualiseur : **VirtualBox v6.1**\
* OS Guest: **Ubuntu Server 22.04 LTS**\
* Name Host Ansible Engine: **central**\
* Host: **s1-db, s2, s3**
* Username ansible: **user**
* Default ssh user: **user**
* Default ssh port: **2223**
![](images/environnement.png)

* Ansible version: **ansible[core 2.16.3]**
```
ansible --version
```
> ![](images/ansible-version.png)

## LAB 1- Installation et configuration de l'environnement

En fonction de l’OS serveur installé pour les récentes versions (Ubuntu version 18.xx et + ), 
il y a déjà une version python v3.xx installée.

* Vérifier la version de python installée
```
python3 -V
```
> Python 3.10.12

* Installation de pip
```
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
```
```
python3 get-pip.py
```
* Installation de ansible via pip
```
python3 -m pip install ansible
```
> [!Tip]
> J’ai dû créer une machine virtuelle avec les dernières mise à jour sans y installer d'autres paquets particuliers. 
Ce VM sera cloner pour obtenir d’autres instances. Du coup on aura les mêmes instances basées sur le même système d’exploitation. 


Je vais juste changer le hostname générique s1 par le host pour facilement s’y retrouver
```
hostnamectl
```

Pour la machine ansible engine on va changer le host s1 par central
```
sudo hostnamectl set-hostname central
```

Pour les machines Hosts
Pour la machine s1-db
```
sudo hostnamectl set-hostname s1-db
```

Pour la machine s2
```
sudo hostnamectl set-hostname s2
```

Pour la machine s3
```
sudo hostnamectl set-hostname s3
```

* Configuration de l'environnement

Lors de l'installation du serveur on avait installé en meme temps le serveur openssh.
Il faut ensuite changer le port 22 par défaut pour écouter sur le port 2223. et autoriser les connexion ssh avec cle public

> [!NOTE]
> Ce changement de port n'est pas indispensable c'est juste une recommendation de ne pas utiliser le port par défaut ssh.

```
                            ____ s1-db
                           /
central __ssh port 2223___/_____ s2
                          \
                           \____ s3
```
* Autorisation connexion ssh sans password mais avec une cle public

Generer un ssh keygen
```
ssh-keygen -t rsa -b 4096
```
copier la cle publique vers le hote
cas hote s1-db
```
ssh-copy-id -p 2223 user@10.10.10.2
```

cas hote s2
```
ssh-copy-id -p 2223 user@10.10.10.4
```

cas hote s3
```
ssh-copy-id -p 2223 user@10.10.10.4
```

* Droit et permission du user ansible: acces sudo sans passer par mot de passe.
Sur les machines hotes s1-db, s2, s3
on édite le sudoers en mode root
```
visudo
```

On va ajouter cette ligne a la fin sudoers
```
user ALL=(ALL) NOPASSWD:ALL
```

