# Projet infrastructure

Bienvenue sur le **guide d'installation** du projet infrastructure du groupe de **Lucas Saillot** et **Louise Ducrocq**.

---

## Sommaire

- [Étape 1 : Installer l'OS](#Étape-1--installer-los)
- [Étape 2 : Environnement](#étape-2--environnement)

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
- Avoir un logiciel de connexion SSH comme ([Bitsive](https://bitvise.com/ssh-client-download)).

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
- PHP
- Apache
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
