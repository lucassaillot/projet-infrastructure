# Projet infrastructure

Bienvenue sur le **guide d'installation** du projet infrastructure du groupe de **Lucas Saillot** et **Louise Ducrocq**.

---

## Sommaire

- [Étape 1 : Installer l'OS](#Étape-1--installer-los)
- [Étape 2 : Environnement](#étape-2--environnement)
- [Étape 3 : Premier site](#étape-3--premier-site)
- [Étape 4 : Connexion SSH par Clé / SFTP](#étape-4--connexion-ssh-par-clé--sftp)
- [Étape 5 : Configuration de la base de données](#étape-5---configuration-de-la-base-de-données)

---

## Objectifs

- Configurer et administrer un serveur pour héberger une solution web.  
- Installer, paramétrer et utiliser des outils fiables et reconnus pour l’hébergement de solutions web.  
- Déployer un site web, comme WordPress, sur le serveur.  
- Automatiser les sauvegardes des fichiers du site et de sa base de données.  
- Rendre la solution web accessible sur un réseau local, à la fois via son adresse IP et un nom de domaine personnalisé.  

---

## Prérequis

Avant de commencer, assurez-vous d'avoir les éléments suivants :

- Avoir un logiciel de virtualisation ou un serveur pour pouvoir installer Debian 12 comme OS.
- Avoir téléchargé l'ISO de Debian 12 ([Télécharger ici](https://www.debian.org/download)).
- Avoir un logiciel de connexion SSH comme [Bitsive](https://bitvise.com/ssh-client-download).
- Avoir le logiciel [WinSCP](https://winscp.net/eng/downloads.php)
---

## Étape 1 : Installer l'OS

Avant de commencer toute manipulation il nous faut une machine sous Linux, dans notre cas nous avons choisi Debian 12.

### 1. Booter l'ISO

Booter votre serveur ou machine virtuelle sur l'iso debian, vous devrez arriver sur le même écran affiché ci-dessous.  
Appuyez sur **Graphical install** pour suivre les étapes d'installation dans une interface graphique. Cela ne signifie pas que le serveur sera configuré avec une interface graphique permanente, car celle-ci sera désactivée lors des étapes suivantes.

<p align="left">
  <img src="img/etape1.png" alt="" width="50%" />
</p>

### 2. Sélection de la langue

Sélectionnez votre **Langue**, **Pays**, et **Disposition du clavier** :  

<p align="left">
  <img src="img/etape2.png" alt="Sélectionner la langue" width="50%" />
  <img src="img/etape3.png" alt="Sélectionner le pays" width="50%" />
  <img src="img/etape4.png" alt="Sélectionner la disposition du clavier" width="50%" />
</p>

### 3. Nom de la machine

Sélectionnez le nom de la machine que vous voulez et laisser vide la case domaine.

<p align="left">
  <img src="img/etape5.png" alt="Nom de la machine" width="45%" />
  <img src="img/etape6.png" alt="Nom de domaine" width="45%" />
</p>

### 4. Mot de passe root et nouvel utilisateur

Saisissez un mot de passe complexe pour l'utilisateur **root**.

<p align="left">
  <img src="img/etape7.png" alt="Mot de passe root" width="50%" />
</p>

Nous devons à présent définir un **nouvel utilisateur**, que nous allons appeler cesi dans notre cas.   
En premier lieu le nom puis l'identifiant qui servira de nom d'utilisateur pour se connecter.

<p align="left">
  <img src="img/etape8.png" alt="Nom de l'utilisateur" width="45%" />
  <img src="img/etape9.png" alt="Identifiant" width="45%" />
</p>

Et enfin le **mot de passe** de ce nouvel utilisateur :

<p align="left">
  <img src="img/etape10.png" alt="Mot de passe utilisateur" width="50%" />
</p>

### 5. Configurer les disques

Sélectionner **"Assisté - utiliser un disque entier"** puis vous pouvez cliquer sur **continuer** jusqu'au moment où vous tomberez sur la même page que la deuxième capture d'écran, à ce moment vous mettrez **"Oui"**.

<p align="left">
  <img src="img/etape11.png" alt="Configuration des disques" width="45%" />
  <img src="img/etape12.png" alt="Confirmation des disques" width="45%" />
</p>

### 6. Configurer les paquets

Sélectionner **"France"**.

<p align="left">
  <img src="img/etape13.png" alt="Configuration des paquets" width="50%" />
</p>

Sélectionner non si vous souhaitez désactiver les statistiques sur l'utilisation des paquets.

<p align="left">
  <img src="img/etape14.png" alt="Statistiques des paquets" width="50%" />
</p>

### 7. Choix des outils installés par défaut

⚠️ Important, décocher **"environnement de bureau Debian"** et **"GNOME"** (C'est l'interface graphique de Debian, dans notre cas nous voulons qu'une console comme il s'agit d'un serveur).   
Ainsi que le **Serveur SSH**, nous allons l'installer manuellement.   
Laissez uniquement les utilitaires usuels du système.

<p align="left">
  <img src="img/etape15.png" alt="Choix des outils" width="50%" />
</p>

### 8. Installation de GRUB

Il s’agit du chargeur de démarrage qui permet de booter sur Linux.   
Mettez **oui** puis sélectionnez votre disque.

<p align="left">
  <img src="img/etape16.png" alt="Installation de GRUB" width="45%" />
  <img src="img/etape17.png" alt="Choix du disque" width="45%" />
</p>

### 9. Redémarrer

Voilà vous avez terminé la première étape de ce guide, vous pouvez maintenant redémarrer votre machine quand cet écran s'affiche :

<p align="left">
  <img src="img/etape18.png" alt="Redémarrer" width="50%" />
</p>

## Étape 2 : Environnement

Nous allons dans ces étapes installer :
- SSH
- Apache
- PHP
- Base de donnée

### 1. Installer et configurer SSH

Dans un premier temps nous allons se connecter à la Machine :
*Vos identifiants sont ceux défini pour l'installation, utiliser l'utilisateur root pour plus de faciliter pour commencer*
<p align="left">
  <img src="img/connexion.png" alt="Connexion" width="75%" />
</p>

Pour commencer on autorisé votre utilisateur à utiliser "sudo" pour plus de facilité, notre utilisateur s'appelle **cesi** dans notre cas.

```
su #Pour se connecter en root pour régler les paramètres
echo "cesi ALL=(ALL:ALL) ALL" | sudo tee -a /etc/sudoers
sudo usermod -aG sudo cesi
exit #Pour retourner à l'utilisateur cesi
```
Rentrer les commandes les unes après les autres

```bash
apt install sudo -y
sudo apt update
sudo apt upgrade -y
sudo apt install openssh-server -y
```

Une fois effectué, rentrer la commande suivante :
```bash
sudo systemctl status sshd
```
Vous devrez obtenir un statut "active"
<p align="left">
  <img src="img/install_ssh.png" alt="SSH" width="75%" />
</p>

On va récupérer l'ip local de notre machine pour pouvoir s'y connecter en SSH via notre PC.
```bash
ip a
```
<p align="left">
  <img src="img/ip_a.png" alt="IP A" width="75%" />
</p>

Noté bien cette IP et son /, dans notre cas **192.168.87.128/24** <br>

Lancer bitsive et configurer le avec l'ip récupéré, le port 22 et les identifiants choisis. Connecter vous et ensuite cliquer sur "New terminal console"

<p align="left">
  <img src="img/bitsive.png" alt="Bitsive" width="50%" />
</p>

<p align="left">
  <img src="img/terminal.png" alt="Bitsive" width="50%" />
</p>

### 2. Définir une IP fixe

Nous allons récupérer la passerelle de notre réseau
```bash
ip route show
```
Nous obtenons ce résultat :
default via **192.168.87.2** dev ens33

Nous allons maintenant définir l'ip fixe
```bash
sudo nano /etc/network/interfaces
```
<p align="left">
  <img src="img/dhcp.png" alt="dhcp" width="50%" />
</p>

Par défaut l'interface est réglé en DHCP, qui fait que l'ip est dynamique.
Nous avons au préalable récupéré l'ip fixe à mettre et la passerelle.
Voilà ce qu'il faut remplir à la place de DHCP, address correspond à l'ip récupéré avec son masque et le gateway correspond à la passerelle.
```
iface ens33 inet static
        address 192.168.87.128/24
        gateway 192.168.87.2
```
<p align="left">
  <img src="img/static.png" alt="static" width="50%" />
</p>

Pour enregistrer et quitter (CTRL + S et CTRL + X) <br>

Nous allons maintenant redémarrer le service réseau :
```
sudo systemctl restart networking.service
```
Si tout c'est bien passé vous pouvez maintenant effectuer la commande suivante :
```
ping google.com
```
Si tout fonctionne vous recevrez des paquets ce qu'il signifie que votre machine à internet et une adresse IP fixe.

### 3. Installation d'Apache

Commençons par installer Apache. <br>
Tapez les commandes les unes après les autres.
```
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2
```
Accéder maintenant à l'ip de votre machine sur votre ordinateur et vous verrez cette page qui signifie que l'installation d'apache s'est bien déroulé.

<p align="left">
  <img src="img/apache.png" alt="apache" width="75%" />
</p>

### 4. Installation PHP

Nous allons installer PHP avec la commande suivante :
```
sudo apt install -y php libapache2-mod-php
sudo apt-get update
```

Nous allons à présent vérifier si php est correctement installé.
Commençons par créer et éditer un fichier info.php dans le dossier apache :
```
sudo nano /var/www/html/info.php
```
Nous allons utiliser une fonction php qui permet de vérifier si tout est bien installé :
```
<?php
phpinfo();
?>
```
Pour enregistrer et quitter (CTRL + S et CTRL + X) <br>
Rendez-vous sur votre adresse IP /info.php dans notre cas :
http://192.168.87.129/info.php
<p align="left">
  <img src="img/php.png" alt="apache" width="75%" />
</p>

Voilà PHP est installé et fonctionnel !

### 5. Installation de MariaDB
Pour installer MariaDB :
```
sudo apt update && sudo apt upgrade -y
sudo apt install mariadb-server -y
sudo systemctl start mariadb
sudo systemctl enable mariadb
```
Voilà MariaDB est installé mais il va falloir à présent sécurisé l'installation :

```
sudo mysql_secure_installation
```
- Définir le mot de passe root (si demandé).
- Supprimer les utilisateurs anonymes : Répondez Oui.
- Désactiver l'accès root à distance : Répondez Oui.
- Supprimer la base de données de test : Répondez Oui.
- Recharger les tables de privilèges : Répondez Oui.


## Étape 3 : Premier site
Placez vous dans le dossier apache :
```
cd /var/www/html
```
Créer un dossier monsite :
```
sudo mkdir monsite
```
Placer vous dans ce dossier, créer et modifier un fichier index.php :
```
cd ./monsite
sudo nano index.php
```
Nous placer dans ce fichier le code suivant (vous êtes libre de placer le code que vous voulez) :
```
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bienvenue sur mon site</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #f0f8ff;
        }
        .container {
            text-align: center;
            background: white;
            padding: 2em;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        h1 {
            color: #333;
            font-size: 2.5em;
            margin-bottom: 0.5em;
        }
        p {
            color: #666;
            font-size: 1.2em;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Bienvenue sur notre site</h1>
        <p><?php echo "Projet infrastructure"; ?></p>
    </div>
</body>
</html>
```
Pour enregistrer et quitter (CTRL + S et CTRL + X) <br>
Rendez-vous sur l'ip /monsite, dans notre cas : http://192.168.87.128/monsite/
<p align="left">
  <img src="img/monsite.png" alt="apache" width="75%" />
</p>

## Étape 4 : Connexion SSH par Clé / SFTP
Ouvrez bitvise puis cliquer sur "Client key manager"
<p align="left">
  <img src="img/clientmanager.png" alt="apache" width="50%" />
</p>

Cliquez sur "Generate New" et sélectionner RSA, 4096 et votre passphrase
<p align="left">
  <img src="img/generate_new_ssh.png" alt="apache" width="25%" />
</p>

Une fois généré cliquer sur export puis ouvrer le fichier vous debrez avoir votre Clé SSH :
<p align="left">
  <img src="img/export_ssh.png" alt="apache" width="50%" />
</p>
Copier tout ça, nous allons en avoir besoin.

**Retour sur la VM**
```
mkdir ~/.ssh/
sudo nano ~/.ssh/authorized_keys
```
Copier dans ce fichier votre clé, puis pour enregistrer et quitter (CTRL + S et CTRL + X)

Vous pouvez maintenant vous reconnecter avec votre clé Public SSH en choisissant :
- Initial method : publickey
- Client key : celui que vous avez créer
- Passphrase : votre passphrase
<p align="left">
  <img src="img/ssh_public_key.png" alt="apache" width="50%" />
</p>

### Connexion SFTP
Nous allons créer deux utilisateurs dev1 et dev2 qui vont avoir accès en SFTP au dossier /var/www/html/monsite et seulement ce dossier :
Commençons par créer un groupe sftpusers :
```
sudo groupadd sftpusers
```

Nous allons maintenant créer les utilisateurs :
```
# Création l'utilisateur Dev1
sudo useradd -g sftpusers -d /var/www/html/monsite -s /usr/sbin/nologin dev1
sudo passwd dev1

# Création l'utilisateur Dev2
sudo useradd -g sftpusers -d /var/www/html/monsite -s /usr/sbin/nologin dev2
sudo passwd dev2
```
Possédé par root
Non modifiable par l’utilisateur (permissions classiques : 755)
```
sudo chown root:root /var/www/html/monsite
sudo chmod 755 /var/www/html/monsite
```

Autoriser les sftpusers à modifier les fichiers de monsite
```
sudo chown -R root:sftpusers /var/www/html/monsite/*
sudo chmod -R g+rw /var/www/html/monsite/*
```

On va maintenant configurer le SFTP :
```
sudo nano /etc/ssh/sshd_config
```
Trouver la ligne suivante :
```
Subsystem       sftp    /usr/lib/openssh/sftp-server
```

Puis ajouter ceci à la suite :
```
Match Group sftpusers
  ChrootDirectory /var/www/html/monsite
  ForceCommand internal-sftp
  AllowTCPForwarding no
  X11Forwarding no
```
Pour enregistrer et quitter (CTRL + S et CTRL + X) <br>
Redémarrer SSH :
```
sudo systemctl restart ssh
```
Ouvrez WinSCP et ajouter un nouvelle connexion SFTP :
- Nom d'hôte : IP de votre VM
- Port : 22
- User : dev1 ou dev2
- MDP : celui choisi par vous
<p align="left">
  <img src="img/WinSCP_Connexion.png" alt="apache" width="50%" />
</p>
<p align="left">
  <img src="img/WinSCP_Connecte.png" alt="apache" width="50%" />
</p>

## Étape 5 - Configuration de la base de données

Connectez vous à la base de donnée :
```
mysql -u root -p
```
Création de la base de donnée "cesibdd" :
```
CREATE DATABASE cesibdd;
```
Créer l'utilisateur dibdd (remplacer mot_de_passe par le votre) :
```
CREATE USER 'dibdd'@'localhost' IDENTIFIED BY 'mot_de_passe';
```
Lui accorder le maximum de privilège :
```
GRANT ALL PRIVILEGES ON cesibdd.* TO 'dibdd'@'localhost';
FLUSH PRIVILEGES;
```
Quitter :
```
EXIT;
```
