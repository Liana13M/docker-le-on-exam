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
