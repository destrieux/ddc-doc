#Comment chiffrer une partition sur un système existant ?
(d'après adrien.restoin@univ-tours.fr et vincent.riffonneau@univ-tours.fr, DSI Université de Tours)<br>

* Le plus simple est de [chiffrer le disque au moment de l'installation du système](https://www.debian.org/releases/stable/amd64/ch06s03.fr.html#di-partition) en utilisant l'option *LVM avec chiffrement* lors du partitiionement assisté.
* Cette procédure explique comment chiffer la partition /var d'un système existant. Cette partition contient le site web, les documents liés et les bases de données wordpress et civicrm.

## Configuration matérielle et prérequis

### Installation de crypsetup
Pour les distributions récentes, l'outil LUKS cryptsetup est inclus dans les paquets pré-installés. Si ce n'est pas le cas :

```bash
sudo apt update
sudo apt install cryptsetup
```
### Identification du disque à chiffrer
* Vous devez disposer d'un disque additionnel à ajouter au système et vérifier qu'il est bien visible et actif dans l'UEFI ou le BIOS. 
* La commande lsblk repère le disque ajouté et le disque système contenant l apartition */var*. 

```bash
lsblk -f
---

NAME   FSTYPE FSVER LABEL UUID            FSAVAIL FSUSE% MOUNTPOINTS  
sda                                                                              
sdb                                                                              
├─sdb1 ext4   1.0         c92bcf96-9914-46ee-b6dd-db386b6c1a49    5,4G    22% /  
├─sdb2                                                                           
├─sdb5 swap   1           0d5fd09e-2e45-4fe1-bea0-9f941a473edc                \[SWAP\]  
└─sdb6 ext4   1.0         ecd4e1cd-532f-4ce2-8795-d162f376a544   11,1G     0% /home  
sr0

```

Ici :

* le disque sdb contient l'ensemble du système, dont */var*,
* le disque sda ne contient aucune partition. C'est lui qui devra être partitionné, chiffré et monter

* Si le disque ajouté, sda, contenait des partitions:
    * il faudraitt d'abord les démonter 
    * retirer les points de l'ensemble du système
    * vider entièrement le disque, ==incluant la table de partition du disque sda==.

```bash
    sudo dd bs=1M if=/dev/zero of=/dev/sda status=progress
```

### Chiffrement du disque

* Lors du chiffrement, il faut créer une phrase secrète, l'enregistrer et la communiquer à l'administrateur (et l'utilisateur, s'il n'existe pas de déverrouillage TPM2):

```bash
sudo cryptsetup luksFormat \
  --type luks2 \
  --cipher aes-xts-plain64 \
  --key-size 512 \
  --hash sha512 \
  --pbkdf argon2id \
  /dev/sda2

# ATTENTION !

Cette action écrasera définitivement les données sur /dev/sda.

Êtes-vous sûr ? (Typez « yes » en majuscules) : YES  
Saisissez la phrase secrète pour /dev/sda :
Vérifiez la phrase secrète :
```
> Cette configuration utilise AES-256 XTS, SHA-512, Argon2id, LUKS2 (en mode XTS, deux clés AES sont utilisées, donc AES-256 correspond à une clé de 512 bits)

* La clé ajoutée doit maintenant être contenue dans un fichier de clés récupéré au montage des disques chiffrés : 

    * créer un bloc de déchiffrement sur le disque système:

    ```bash
    sudo dd bs=512 count=4 if=/dev/random iflag=fullblock | sudo install -m 0600 /dev/stdin /boot/cleluks
    ```

    * Attribuer ce disque et ce bloc au disque chiffré 

    ```bash
    sudo cryptsetup luksAddKey /dev/sda /boot/cleluks
    ```

### Partitionnement du disque chiffré

* Déchiffrer le disque sda 
```bash
sudo cryptsetup luksOpen /dev/sda secure
```
Si vous regardez dans `lsblk`, une partition *secure* est désormais présente sur le disque sda.

* Formater cette partition */dev/mapper/secure*
```bash
sudo mkfs.ext4 -L homeluks /dev/mapper/secure #a modifier selon les besoins de systèmes de fichiers
```
* Monter la partition *secure* sur le point de montage */montage*
```bash
sudo mkdir secure
sudo mount /dev/mapper/secure /secure
```

### Intégration à l'environnement

Deux options de fonctionnement sont possibles :

#### Montage "en dur"

Il faudra copier l'ensemble des fichiers de /home vers le point de montage provisoire, puis au prochain démarrage, le disque chiffré "écrase" l'ancien /home par un montage direct sur ce répertoire.

```bash
sudo rsync -avh /var/ /secure/
```

#### Redirection par liens symboliques
* Vous pouvez également générer un ou plusieurs liens symboliques redirigeant vers le point de montage qui deviendra définitif (vous n'aurez donc pas à remonter directement vers le /home).

* Cette solution est très utile si vous souhaitez "panacher" la destination des répertoires au sein du même répertoire source. <br>
Ici par exemple, nous allons rediriger simplement le répertoire *www* vers le nouveau disque:

```bash
sudo rsync -avh /var/www secure/www #privilégié car il copie également les droits
sudo rm -rf /var/www #vous pouvez également déplacer vers une backup
sudo ln -s /secure/www /var/www
```

#### Montage en lien

Sur les versions plus récentes de Debian ou Ubuntu, il est également possible de faire un montage mixte de type --bind qui est privilégié pour ce type d'usage à la place du lien symbolique:

```bash
sudo mount --bind /secure/www /var/www
```

Il faudra ajouter une ligne par montage dans le fstab:

```bash
sudo nano /etc/fstab
```

```ini
/secure/www /var/www none bind,uid=158444,gid=100,nofail 0 0
```
Les propriétés du montage dépendent évidemment des fonctionnalités et accès du montage.

### Déverrouillage et montage au démarrage du poste
Il faut :

* repérer l'identifiant du disque chiffré,
* l'ajouter à l'amorçage sécurisé
* modifier fstab
* modifier GRUB

#### Repérer l'identifiant du disque chiffré
```bash
lsblk -f
NAME     FSTYPE      FSVER LABEL    UUID            FSAVAIL FSUSE% MOUNTPOINTS  
sda      crypto_LUKS 2              361b0143-1880-4ac3-bf4b-19b63a16a2a8                   
└─secure ext4        1.0   homeluks 25529cfc-75aa-4a61-9102-b2d353efd74e    8,1G     0% /secure  
sdb                                                                                        
├─sdb1   ext4        1.0            c92bcf96-9914-46ee-b6dd-db386b6c1a49    5,4G    22% /  
├─sdb2                                                                                     
├─sdb5   swap        1              0d5fd09e-2e45-4fe1-bea0-9f941a473edc                \[SWAP\]  
└─sdb6   ext4        1.0            ecd4e1cd-532f-4ce2-8795-d162f376a544   11,1G     0% /home  
sr0
```

Nous voyons ici que l'identifiant du disque chiffré est bien **361b0143-1880-4ac3-bf4b-19b63a16a2a8**. <br>
C'est cet identifiant qui sera ajouté à /etc/crypttab:

#### Ajouter ce disque à l'amorçage sécurisé
```bash
sudo nano /etc/crypttab
```

```ini
securedisk  UUID=361b0143-1880-4ac3-bf4b-19b63a16a2a8 /boot/cleluks.key nofail
```

#### Modifier le fstab
```bash
sudo nano /etc/fstab
```

```ini
UUID=25529cfc-75aa-4a61-9102-b2d353efd74e  /secure ext4 rw,nofail,noatime 0 0
```

#### Modifications du GRUB
* Il s'agit d'ajouter la ligne permettant l'utilisation de disques chiffrés dans le champ de paramètres, si elle n'est pas déjà présente:
```bash
sudo nano /etc/default/grub
```
```ini
GRUB_ENABLE_CRYPTODISK=y
```

* Mettre à jour le grub
```bash
sudo update-grub
```

* Installer la partition concernée dans le grub:
```bash
sudo grub-install /dev/mapper/secure
```

* Régénérer le grub pour intégrer ces modifications à l'amorçage:
```bash
sudo systemctl daemon-reload
```

Evidemment, les identifiants et points de montage seront différents selon les systèmes et usages ; ces dernières commandes dépendent de l'environnement matériel et logiciel et ne sont ==pas à copier-coller== sans réflexion.

### Déverrouillage automatique par TPM2

* Vous pouvez également inscrire la clé de déverrouillage dans la puce de sécurité TPM2 si votre poste dispose de cette technologie. 
* Cette méthode permet la suppression du fichier de clé dans /boot/cleluks et de la ligne de direction vers la clé dans crypttab
