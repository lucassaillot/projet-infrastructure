# Projet infrastructure

Bienvenue sur le **guide d'installation** du projet infrastructure du groupe de **Lucas Saillot** et **Louise Ducrocq**.

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
  <img src="img/etape5.png" alt="Sélectionner la langue" width="45%" />
  <img src="img/etape6.png" alt="Sélectionner le pays" width="45%" />
</p>

### 4. Mot de passe root et nouvel utilisateur

Saisissez un mot de passe complexe pour l'utilisateur root

<p align="left">
  <img src="img/etape7.png" alt="" width="50%" />
</p>

Nous devons à présent définir un nouvel utilisateur, que nous allons appeler cesi dans notre cas.   
En premier lieu le nom puis l'identifiant qui servira de nom d'utilisateur pour se connecter.

<p align="left">
  <img src="img/etape8.png" alt="Sélectionner la langue" width="45%" />
  <img src="img/etape9.png" alt="Sélectionner le pays" width="45%" />
</p>


