# QA Postman Tests - Exercice 1

## Description
Projet qui contient une collection Postman "Exo1" (qu'on as fait dans le cour) pour tester l'API.

## Fichiers
- Exo1.postman_collection.json : collection Postman
- PP.postman_environment.json : environnement Postman

## Prérequis
- Node.js
- Newman
- Jenkins

## Exécution manuelle
```bash
newman run Exo1.postman_collection.json -e PP.postman_environment.json
