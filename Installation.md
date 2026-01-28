---
description: Installation des softwares sur RPi
label: installation
date: 2026-01-05

---
# Installation de la carte base
(rpi-install)=
## Préparation de la carte SD pour la RPi
La première étape pour installer un système aqtion est de préparer une carte SD ([détails pour l'achat](https://aquineuro.synology.me/oo/r/13d7nrd8NygPOMaPGBaHmeTspGUbELm5#tid=1&range=7:7)) et d'y installer Raspbian Lite OS. La procédure est détaillée pas à pas dessous mais il faut commencer par insérer la carte SD dans l'ordinateur et s'assurer que [Raspberry Pi Imager](https://www.raspberrypi.com/software/) est installé. Ensuite, il faut suivre ces étapes : 

### Choix du système d'exploitation
:::{figure} images/RPi_install/01.PNG
:label: step_01

Nous utilisons des [Raspberry Pi 4](https://aquineuro.synology.me/oo/r/13d7nrd8NygPOMaPGBaHmeTspGUbELm5#tid=1&range=6:6). C'est donc ce modèle qu'il faut sélectionner
:::

:::{figure} images/RPi_install/02.PNG
:label: step_02

Pour installer la bonne version de l'OS il faut aller la chercher dans "Raspberry Pi OS (other)". On installe une version sans interface graphique !
:::

:::{figure} images/RPi_install/03.PNG
:label: step_03

On installe une version sans interface graphique : Le OS Lite (64 bit).
:::

:::{figure} images/RPi_install/04.PNG
:label: step_04

Il suffit de sélectionner la carte SD qui est sans doute le seul disque listé
:::

### Configuration
On renseigne quelques éléments pour personnaliser l'installation :

:::{figure} images/RPi_install/05.PNG
:label: step_05

Le nom du système. En général `aqtion????` avec 4 chiffres pour l'identification. Ces éléments sont à enregistrer dans le [fichier de suivi des systèmes aqtion](https://aquineuro.synology.me/oo/r/15zDkF3UgaFkCV93oI1MIz6crogwllNX) sur le NAS.
:::

:::{figure} images/RPi_install/06.PNG
:label: step_06

Le nom d'utilisateur est en général `aquineuro`. Le mot de passe doit être relativement simple à saisir, car nous serons amenés à l'utiliser plusieurs fois dans la suite de l'installation. Ces éléments sont à enregistrer dans le [fichier de suivi des systèmes aqtion](https://aquineuro.synology.me/oo/r/15zDkF3UgaFkCV93oI1MIz6crogwllNX) sur le NAS.
:::

:::{figure} images/RPi_install/07.PNG
:label: step_07

Inutile de configurer le WIFi.
:::

:::{figure} images/RPi_install/08.PNG
:label: step_08

Il est indispensable d'activer le SSH et de laisser l'authentification par mot de passe. C'est ce mécanisme que nous allons utiliser pour finaliser l'installation d'aqtion.
:::

:::{figure} images/RPi_install/09.PNG
:label: step_09

Inutile de l'activer
:::

:::{figure} images/RPi_install/10.PNG
:label: step_10

Nous sommes fin prêts à lancer l'installation en cliquant sur "Écrire". Pour s'assurer que nous sommes conscients de ce qu'il va se passer il y a un petit minuteur de quelques secondes au bout du quel le bouton pour lancer l'écriture est activé. Il faut donc patienter.
:::

:::{figure} images/RPi_install/11.PNG
:label: step_11

Nous sommes fin prêts à lancer l'installation en cliquant sur "Je comprends, effacer et écrire". Pour s'assurer que nous sommes conscients de ce qu'il va se passer il y a un petit minuteur de quelques secondes au bout du quel le bouton pour lancer l'écriture est activé. Il faut donc patienter.
:::

Une fois l'écriture sur la carte SD terminée, elle est automatiquement éjectée. On peut donc la récupérer et la placer dans une RPi.

:::{important}
Il est **très important** de copier les identifiants (nom d'utilisateur et mot de passe) dans le [fichier de suivi des systèmes aqtion](https://aquineuro.synology.me/oo/r/15zDkF3UgaFkCV93oI1MIz6crogwllNX) sur le NAS.
:::

(aqtion-install)=
## Déploiement du code aqtion
Une fois que la carte SD est dans une RPi il faut y mettre le code aqtion. Pour cela il faut pouvoir y accéder depuis le réseau, en [SSH](https://en.wikipedia.org/wiki/Secure_Shell). Pour cela, si l'installation se fait à l'INCIA il faut attribuer une IP fixe à cette RPi.

(ip-fixe)=
### Préliminaire à l'INCIA : IP fixe
:::{tip}
Ceci n'est à faire que si l'installation s'effectue à l'INCIA, qui nous oblige à avoir des IP fixes
:::

Pour se faire il faut brancher un clavier (USB) et un écran (*via* un câble miniHDMI - HDMI) à la RPi.  Une fois que clavier et écran sont branchés il faut allumer la RPi, et se logguer. Ensuite, saisir la ligne suivante : 
```bash
sudo nmtui
```
qui lance l'interface du *Network Manager* (Network Manager Terminal User Interface - nmtui). Une commande commençant par `sudo` permet d'exécuter ce qui suit en mode "administrateur" ce qui veut dire que le mot de passe peut être demandé. 
Il faut ensuite suivre les quelques étapes ci-dessous pour configurer la RPi pour qu'elle puisse être reconnue sur le réseau de l'INCIA. On se déplace dans l'interface en utilisant les flèches du clavier ainsi que les touches tabulation et entrée.

:::{figure} images/RPi_install/12.PNG
:label: nmtui_01

Choisir "Edit a connection"
:::

:::{figure} images/RPi_install/13.PNG
:label: nmtui_02

"Edit ..." sur la connexion Ethernet
:::

:::{figure} images/RPi_install/14.PNG
:label: nmtui_03

Il faut ensuite remplir les champs comme sur cette capture d'écran. Les éléments importants : 
* IPv4 configuration: Manual 
* Addresses: 172.22.0.20/24
* Gateway: 172.22.0.30
* DNS servers: 109.0.66.11
:::
Il suffit ensuite de quitter cette interface.

On peut ensuite redémarrer la RPi en tapant `sudo shutdown now`

:::{tip}
Une fois que l'IP a été configurée, tout peut se faire à distance, sans écran ni clavier, seule la connexion réseau est nécessaire. Depuis un ordinateur windows il faut lancer le powershell et taper `ssh aquineuro@aqtion0000`. Répondre `yes` à la question sur l'authenticité.
:::

### Activation de l'I2C

On peut en profiter pour s'assurer que l'I2C est bien activé, et l'activer le cas échéant, en utilisant `sudo raspi-config` et en suivant les quelques étapes ci-dessous :

:::{figure} images/RPi_install/15.PNG
:label: i2c_1

Sélectionner le menu Interfaces et appuyer sur Entrée.
:::

:::{figure} images/RPi_install/16.PNG
:label: i2c_2

Dans ce menu, sélectionner I2C et appuer sur Entrée
:::

:::{figure} images/RPi_install/17.PNG
:label: i2c_3

Sélectionner **Yes** appuyer sur Entrée.
:::


### Installation à distance
Le déploiement des programmes nécessaires se fait désormais à distance, depuis un PC (souvent le laptop Aquineuro) *via* un script qui s'appelle `deploy.py` auquel il faut fournir les identifiants de connexion.

```
python deploy.py --host aqtion0000 --user aquineuro --password mot_de_passe --do_all
```


### systemd - pigpiod
Sur les versions récentes de Raspbian (basées sur Debian Trixie 13) le service qui lance le démon pigpio (qui est est responsable de la gestion des GPIOs) n'est plus créé automatiquement lors de l'installation. Il faut donc le faire manuellement. Pour cela il faut créer, en mode super utilisateur, un fichier dans `/etc/systemd/system` en tapant : 
```bash
sudo nano /etc/systemd/system/pigpiod.service
```

ce qui ouvre un éditeur de texte rudimentaire dans lequel on peut copier le contenu suivant : 
```
Description=Pigpio GPIO daemon
After=network.target

[Service]
ExecStart=/usr/local/bin/pigpiod
ExecStop=/bin/systemctl kill pigpiod
Type=forking
Restart=always

[Install]
WantedBy=multi-user.target
```

Pour sauvegarder les changements on fait `Ctrl + O`  et `Entrée` ensuite pour quitter `Ctrl + X`. Une fois sortis de nano, il faut activer ce nouveau service et vérifier que tout fonctionne bien :
```bash
sudo systemctl daemon-reload
sudo systemctl enable --now pigpiod.service
sudo systemctl restart pigpiod
sudo systemctl status pigpiod
```


On peut, enfin, redémarrer la RPi en tapant : `sudo reboot now`

### Configuration
Il faut générer un fichier de configuration (`default_config.json`) qui fait le lien entre les pins de la RPi et les entrées / sorties effectivement branchées sur le système, et qui leur attribue des nom

(dhcp-rpi)=
### Passage en DHCP
Une fois toute l'installation terminée il faut repasser la configuration réseau en DHCP pour que l'ordinateur du setup puisse lui atttribuer une IP. Pour cela, une fois connecté en SSH, répéter les étapes [1](#nmtui_01) et [2](#nmtui_01) du paragraphe sur [l'IP fixe.](#ip-fixe)
Cette fois l'IPv4 configuration doit être placée sur Automatic.  Les champs `Adresses` et `Gateway` doivent être vides (utiliser `Remove`). Le `DNS server` doit contenir l'IP du PC sur lequel sera installé le client. Par exemple pour le portable Aquineuro il s'agit de `192.168.1.1`.	
