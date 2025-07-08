# Examen Docker
__FINIAVANA RANARIMANANA  Liana MIharisoa__  
*L1A/135/24-25*            
<br>
<br>

## Généralité sur Docker
__Docker__ est un est outil qui a perimis de populariser la conteneurtisation qui est finalement un mode de mutualisation ou de virtualisation des serveurs sur des machines physiques ou machines virtuelles.
1. Objectifs
* Simplifier les deploiements
* Changer le mode de livrable
* Faciliter la gestion des dépenedances
<br>

2. Concepts ou principes  
**Image** (enveloppe de stockage) : un élément inactif, une sorte de méta package qui contient à la fois du code et des éléments statiques :
- Code du/des programme(s)
- Toutes les dépendances

**Conteneur** : c'est l'activation d'une image de manière à faire tourner le processus :
- Un ou plusieurs process
- Isolation : cgroups / namespaces

![Conteneur](concept.png)
<br>
<br>

## Les commandes basiques sur Docker
---
sudo usermod -aG docker $USER
---
Cette commande permet d'ajouter l'utilisateur au groupe docker. Cela t’autorise à exécuter Docker sans devoir mettre sudo à chaque fois.
<br>

---
docker ps
---
Cette commande permet d’afficher la liste des conteneurs Docker en cours d'exécution.
<br>

---
docker ps -a
---
Sert à voir tous les conteneurs, qu'ils soient actifs ou non.
<br>

---
docker ps -q
---
Liste seulement les IDs des conteneurs Docker actifs.
<br>

---
docker ps -qa
---
Cette commande affiche uniquement les IDs de tous les conteneurs Docker, qu’ils soient actifs ou arrêtés.
<br>

---
docker run nginx:latest
---
Sert à télécharger (si nécessaire) et démarrer un conteneur qui exécute le serveur web Nginx.
L'image nginx:latest utilise la dernière version de Nginx.
Cette commande lance Nginx dans un conteneur, prêt à fonctionner.
<br>

---
docker run -d nginx:latest
---
Cette commande sert à démarrer un conteneur avec l’image Nginx en mode détaché (en arrière-plan).
L’option -d permet de laisser le conteneur tourner en arrière-plan sans bloquer le terminal.
<br>

---
docker run -d --name c1 nginx:latest
---
Cette commande lance Nginx dans un conteneur nommé c1, qui tourne en arrière-plan.
<br>

---
docker rm -f c1
---
Cette commande sert à supprimer immédiatement le conteneur c1, même s'il est encore en cours d'exécution.
<br>

---
docker run -ti --name c1 debian:latest
---
Elle démarre un conteneur Debian avec accès au terminal pour exécuter des commandes, dans un conteneur nommé c1.
<br>

---
docker run -ti --rm --name c1 debian:latest
---
Cette commande lance un conteneur Debian interactif, nommé c1, qui sera supprimé dès qu'on quittera le terminal.
<br>
<br>

## Docker Volume
Un *Docker Volume* est un espace de stockage persistant et partagé que Docker utilise pour conserver les données des conteneurs, même si le conteneur est supprimé ou recréé.
<br>

__Intérêts:__  
* Facile pour persister de la donnée
* Pratique pour faire les backups
* Partager entre de multiple conteneur
* Multiconteneurs & permissions
* Locals ou distants

![Volume](volume_dck.png)






