---
description: Installation du software client
label: installation-client
date: 2026-01-28
---

# Installation du client
## Installation de Python
## Création de l'environnement virtuel
## Installation des packages
### Problèmes potentiels
pyqt6 -> uninstall / reinstall
### Création d'un raccourci
```bash
```

## Réseau
Installation de Technitium DNS server: https://technitium.com/dns/
Download + Install
Ouverture de l'application web pour configuration : localhost:5380 et setup du nouveau mot de passe. Sur le laptop Aquineuro c'est `Aquineuro`.
Il faut ensuite paramétrer le DHCP: 
D'abord mettre une IP fixe au PC : `192.168.1.1` avec masque `255.255.255.0` dans les paramètres windows

Il faut ajouter un Scope DHCP donc dans l'onglet DHCP puis le sous onglet Scopes faire Add. 

:::{figure} images/DHCP_Win/DHCP_Edit.png
:label: dhcp_step_01

Création d'un scope DHCP
:::

Ensuite, remplir les options en suivant les captures d'écran suivantes :

:::{figure} images/DHCP_Win/DHCP_Param_1.png
:label: dhcp_step_02

Première série de paramètres
:::

:::{figure} images/DHCP_Win/DHCP_Param_2.png
:label: dhcp_step_03

Éventuellement on interdit certaines IP si on veut assigner des IPs fixes à certains matériels
:::

:::{figure} images/DHCP_Win/DHCP_Param_3.png
:label: dhcp_step_04

Si on veut des IPs fixes, c'est ici qu'elles sont attribuées en fonction des adresses MAC.

:::

:::{figure} images/DHCP_Win/RPi_MAC.png
:label: mac_address

Pour obtenir l'adresse MAC d'une RPi, on peut taper la ligne de commande `ip link` une fois connecté en SSH (voir la documentation concernant l'[installation du RPi](#aqtion-install)
:::
