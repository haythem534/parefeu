# Pare-feu Linux avec Interface Web

Ce projet propose un pare-feu Linux complet basé sur nftables avec une interface web Flask. Il inclut des outils de supervision tels que Suricata, Rsyslog, et permet la configuration dynamique du pare-feu ainsi que la génération de configurations VPN avec WireGuard.

## Fonctionnalités

- Interface web Flask pour gérer les règles du pare-feu (via nftables)
- Analyse réseau avec Suricata
- Journalisation et affichage des logs Suricata et Flask a travers Rsyslog
- Génération de configuration VPN  WireGuard
- Déploiement rapide via Docker et Docker Compose


## Prérequis

- Système Linux avec Docker et Docker Compose installés (v1 ou v2 compatibles)
- Accès root (ou équivalent `sudo`)
- Connexion réseau fonctionnelle

## Installation

1. Cloner le dépôt GitHub

git clone https://github.com/haythem534/parefeu.git
cd parefeu

2. récuperer l’image docker

docker pull haythembj/parefeu:latest

3. Configurer l’interface réseau

Dans le fichier .env, indiquez l'interface réseau principale (par exemple enp0s3, eth0, ens33, etc.) :

INTERFACE=eth0

4. Démarrer les services

docker-compose up


## Utilisation

Une fois le conteneur lancé, accédez à l’interface via :

http://localhost:5000

Identifiants par défaut :

Nom d’utilisateur : admin

Mot de passe : Admin123


> Vous pouvez modifier les identifiants depuis l’interface web une fois connecté.

créez une configuration vpn puis copiez la configuration client vers le fichier /etc/wireguard/wg0.conf sur une autre machine client puis lancer l’interface wireguard "wg-quick up wg0" et un tunnel vpn vient d’etre crée.
vous pouvez aussi scanner le QR code pour wireguard mobile


## Arborescence du projet :

parefeu/
├── config/   # Fichiers de configuration (Suricata, rsyslog, etc.)
├── logs/     # Dossier de logs monté depuis le conteneur
├── .env      # Variables d’environnement (interface réseau, etc.)
├── docker-compose.yml   # Définition des services 
└── README.md  # Documentation du projet


## Dépannage

Erreur : INTERFACE variable is not set
Assurez-vous que la variable est définie avec le même nom partout :

Dans .env : INTERFACE=eth0

Dans docker-compose.yml :

environment:
  - INTERFACE=${INTERFACE}
command: ["-i", "${INTERFACE}"]


Interface non détectée : Vérifiez son nom avec ip a ou ifconfig.

Ports déjà utilisés : Assurez-vous que le port 5000 n’est pas utilisé par un autre service.


## Stack Technique

Backend : Python (Flask)

Sécurité réseau : nftables, Suricata

Logs : rsyslog, Suricata (eve.json)

Containerisation : Docker + Docker Compose

