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
Elle montre des informations comme :
* l'ID du conteneur
* l'image utilisée
* l'état (en cours d'exécution ou stoppé)
* la commande lancée
* les ports
* la durée d'exécution
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

---
docker volume ls
---
Cette commande affiche la liste de tous les volumes Docker présents sur la machine.  
Elle nous montre les noms des volumes créés, ce qui permet de savoir quels espaces de stockage persistent sont disponibles.
<br>

---
docker volume create mynginx
---
Cette commande crée un volume Docker nommé mynginx.  
Ce volume servira à stocker des données de façon persistante, par exemple pour un conteneur Nginx, afin que les données restent même si le conteneur est supprimé.
<br>

---
docker run -d --name c1 -v mynginx:/usr/share/ngninx/html/ ngninx:latest 
---
Elle monte un volume Docker nommé mynginx dans le dossier /usr/share/nginx/html/ du conteneur, qui est l’emplacement où Nginx sert les fichiers web. Grâce à ce montage, les fichiers stockés dans le volume persistent indépendamment du conteneur, ce qui permet de conserver les données même si le conteneur est arrêté ou supprimé.
<br>

---
docker exec -ti c1 bash
---
Cette commande permet d’ouvrir un terminal interactif (bash) dans le conteneur c1.  
Concrètement, elle nous donne un accès direct à l’intérieur du conteneur pour exécuter des commandes Linux comme si on était connecté sur une machine normale.
<br>

---
docker volume inspect ngninx
---
Cette commande permet d’afficher les détails du volume Docker ngninx.  
Elle te montre des informations précises, comme :
* le chemin exact où le volume est stocké sur ta machine
* le nom du volume
* le type de volume
* d'autres métadonnées
<br>

### Types de Docker Volumes
![Types of docker volume](vol_types.png)
On a déjà vu précédemment le *Docker Volume*, parlons tout de suite le *Bind Mount*.
<br>
<br>
#### Bind Mount
Le Bind Mount est un type de volume Docker qui permet de monter un dossier directement depuis la machine (l'hôte) vers un conteneur Docker.  
__Comment ça marche?__
* On indique un chemin absolu sur la machine
* Ce dossier devient accessible à l'intérieur du conteneur dans un dossier qu'on choisis
* Toute modification dans ce dossier (dans le conteneur ou sur la machine) sera immédiatement visible des deux côtés
<br>

__*Voici un exemple*__
---
sudo mkdir /data  
sudo mkdir /data2  
sudo touch /data/Hello  
sudo mount --bind /data/ /data2  
sudo findmnt 
---  
On s'intéresse sur la 4ème ligne de commande;  
* Cela fait pointer /data2 vers /data.
* Les deux répertoires deviennent identiques : ce qu'on met dans /data apparaît dans /data2, et inversement
* C'est comme un raccourci monté ou un "miroir" au niveau du système de fichiers
* Très utilisé pour :
    * Accéder à un dossier à partir de plusieurs emplacements
    * Sécuriser l’accès à un sous-dossier sans déplacer les fichiers

Pour la 5ème ligne, elle affiche tous les systèmes de fichiers montés et on y verra une ligne montrant que /data est monté aussi sur /data2.  

<br>
<br>
<br>
Lorsqu'on réalise un montage d'un volume, on va avoir 2 comportements différents:  
* Pour le cas de *Bind Mount*, on va surcharger les datas à partir des données sources du volume sur notre image donc on va écraser potontiellemnt les données qui sont présentes dans l'image au niveau de notre montage
* En revanche, pour le *Docker Volume*, c'est l'inverse, c'est à dire qu'on va prendre ce qui est dans l'image et ça va venir alimenter le volume qu'on va créer à l'extérieur varlip docker volume...   
<br>

__Bind Mount__
---
docker run -d --name c1 --mount type=bind,source=/data/,destination=/usr/share/nginx/html/ nginx:latest
---
Cette commande lance un conteneur Nginx en arrière-plan et relie le dossier /data/ de la machine au dossier web /usr/share/nginx/html/ du conteneur. Ainsi, les fichiers dans /data/ sont directement servis par Nginx. C’est un Bind Mount qui facilite le partage de fichiers entre l’hôte et le conteneur.  
Et si on lance la commande __*docker inspect --format "{{.Mounts}}" c1*__ après, elle inspecte le conteneur c1 pour afficher uniquement les informations sur les montages (volumes et Bind Mounts). Et elle affichera:  
* Le type de montage (bind ou volume)
* Le chemin sur l’hôte (Source)
* Le chemin dans le conteneur (Destination)
* Les options associées  
<br>

__Docker Volume__
---
docker run -d --name c2 --mount type=volume,source=mynginx,destination=/usr/share/nginx/html/ nginx:latest
---
Lors de cette étape, nous avons constaté que les résultats obtenus sont similaires à ceux que nous avions déjà rencontrés précédemment. Cela confirme que le fonctionnement reste identique et que les concepts abordés auparavant s'appliquent toujours ici.















