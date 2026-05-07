<div id="top"></div>

<h1 align="center">Projet DevOps - Node.js Express REST API MySQL</h1>

<div align="center">
  <p align="center">
    Projet complet de déploiement DevOps utilisant Docker, Kubernetes (K3s) et une pipeline CI/CD avec GitHub Actions.
  </p>
</div>

---

# 📚 Table des matières

<details>
  <summary>Afficher la table des matières</summary>

  <ol>
    <li>
      <a href="#a-propos-du-projet">À propos du projet</a>
      <ul>
        <li><a href="#technologies-utilisees">Technologies utilisées</a></li>
      </ul>
    </li>

   <li><a href="#containerisation-docker">Containerisation Docker</a></li>

   <li><a href="#deploiement-kubernetes">Déploiement Kubernetes</a></li>

   <li><a href="#persistance-des-donnees">Persistance des données</a></li>

   <li><a href="#autoscaling-hpa">Autoscaling - HPA</a></li>

   <li><a href="#pipeline-cicd">Pipeline CI/CD</a></li>

   <li><a href="#runner-self-hosted">Runner Self-Hosted</a></li>

   <li><a href="#recuperation-automatique-en-cas-de-crash">Récupération automatique en cas de crash</a></li>

   <li><a href="#structure-du-projet">Structure du projet</a></li>

   <li><a href="#commandes-utiles">Commandes utiles</a></li>

   <li><a href="#objectifs-realises">Objectifs réalisés</a></li>

  </ol>
</details>

---

# 📌 À propos du projet

Ce projet consiste à déployer une API REST Node.js connectée à une base de données MySQL en utilisant les outils DevOps modernes.

L’objectif principal était de mettre en pratique :

- la containerisation avec Docker ;
- l’orchestration avec Kubernetes ;
- la gestion de la persistance ;
- l’autoscaling ;
- l’automatisation du déploiement avec CI/CD ;
- l’utilisation de GitHub Actions ;
- la configuration d’un runner self-hosted.

L’application permet d’effectuer des opérations CRUD (Create, Read, Update, Delete) sur des utilisateurs stockés dans une base de données MySQL.

---

# 🛠 Technologies utilisées

- Node.js
- Express.js
- MySQL
- Docker
- Docker Hub
- Kubernetes
- K3s
- GitHub Actions
- YAML
- Linux (Ubuntu / Debian)

<p align="right">(<a href="#top">retour en haut</a>)</p>

---

# 🐳 Containerisation Docker

L’application a été containerisée avec Docker.

Un fichier `Dockerfile` a été créé afin de :

- définir l’environnement Node.js ;
- installer les dépendances ;
- copier le code source ;
- exposer le port de l’API ;
- lancer automatiquement le serveur Node.js.

Exemple de commandes utilisées :

```bash
docker build -t nom-image .
docker push nom-image
```

L’image Docker générée est ensuite utilisée par Kubernetes pour créer les conteneurs.

<p align="right">(<a href="#top">retour en haut</a>)</p>

---

# ☸ Déploiement Kubernetes

L’application a été déployée sur un cluster Kubernetes K3s.

Plusieurs fichiers YAML (manifests Kubernetes) ont été créés dans le dossier `k8s/`.

---

## 🔹 Déploiement de l’API

Le Deployment de l’API permet de :

- créer les pods de l’API ;
- gérer automatiquement les replicas ;
- recréer les pods en cas de crash ;
- connecter l’API à MySQL ;
- lancer les conteneurs à partir de l’image Docker.

Fichier :

```text
k8s/api-deployment.yaml
```

---

## 🔹 Déploiement MySQL

Le Deployment MySQL permet de :

- lancer le conteneur MySQL ;
- stocker les données ;
- permettre la communication avec l’API via un Service Kubernetes.

Fichier :

```text
k8s/mysql-deployment.yaml
```

---

## 🔹 Services Kubernetes

Les Services Kubernetes ont été utilisés afin de :

- fournir une adresse réseau stable ;
- permettre la communication entre les pods ;
- exposer l’API à l’intérieur du cluster.

Les Services résolvent le problème des IP dynamiques des pods.

<p align="right">(<a href="#top">retour en haut</a>)</p>

---

# 💾 Persistance des données

La persistance a été configurée avec un PersistentVolumeClaim (PVC).

Sans persistance :

- toutes les données MySQL seraient perdues lors d’un crash du pod.

Avec le PVC :

- les données restent disponibles même après la recréation du pod.

Cela garantit la conservation des données de la base MySQL.

<p align="right">(<a href="#top">retour en haut</a>)</p>

---

# 📈 Autoscaling - HPA

Un Horizontal Pod Autoscaler (HPA) a été configuré pour l’API.

Le HPA permet :

- d’ajouter automatiquement des pods lorsque la charge CPU augmente ;
- de réduire automatiquement le nombre de pods lorsque la charge diminue.

Cela améliore :

- la scalabilité ;
- les performances ;
- la disponibilité de l’application.

Fichier :

```text
k8s/api-hpa.yaml
```

<p align="right">(<a href="#top">retour en haut</a>)</p>

---

# 🔄 Pipeline CI/CD

Une pipeline CI/CD a été mise en place avec GitHub Actions.

Le workflow se lance automatiquement lorsqu’une modification est envoyée sur la branche `main`.

La pipeline permet :

- de builder automatiquement l’image Docker ;
- de déployer automatiquement les manifests Kubernetes ;
- d’automatiser le processus de déploiement ;
- de simplifier les mises à jour de l’infrastructure.

Fichier du workflow :

```text
.github/workflows/ci-cd.yml
```

<p align="right">(<a href="#top">retour en haut</a>)</p>

---

# 🖥 Runner Self-Hosted

Un runner GitHub Actions self-hosted a été configuré localement sur la machine virtuelle Linux.

Le runner permet à GitHub Actions d’exécuter les tâches directement sur l’infrastructure locale.

Étapes réalisées :

- téléchargement du runner ;
- configuration du runner ;
- connexion du runner au dépôt GitHub ;
- lancement du service runner.

Le runner écoute ensuite automatiquement les jobs GitHub Actions.

<p align="right">(<a href="#top">retour en haut</a>)</p>

---

# 🔥 Récupération automatique en cas de crash

Les Deployments Kubernetes assurent la récupération automatique des pods.

En cas de crash :

- Kubernetes détecte l’arrêt du pod ;
- le ReplicaSet recrée automatiquement un nouveau pod.

Cela garantit la haute disponibilité de l’application.

<p align="right">(<a href="#top">retour en haut</a>)</p>

---

# 📂 Structure du projet

```text
.
├── Dockerfile
├── README.md
├── .github
│   └── workflows
│       └── ci-cd.yml
├── k8s
│   ├── api-deployment.yaml
│   ├── mysql-deployment.yaml
│   └── api-hpa.yaml
├── sql
├── src
└── postman
```

<p align="right">(<a href="#top">retour en haut</a>)</p>

---

# 💻 Commandes utiles

## Déployer les manifests Kubernetes

```bash
sudo k3s kubectl apply -f k8s/
```

---

## Voir les pods

```bash
sudo k3s kubectl get pods
```

---

## Voir les services

```bash
sudo k3s kubectl get svc
```

---

## Voir le HPA

```bash
sudo k3s kubectl get hpa
```

---

## Construire l’image Docker

```bash
docker build -t nom-image .
```

---

## Push de l’image Docker

```bash
docker push nom-image
```

<p align="right">(<a href="#top">retour en haut</a>)</p>

---

# ✅ Objectifs réalisés

✅ Containerisation Docker

✅ Déploiement Kubernetes

✅ Gestion de la persistance

✅ Autoscaling avec HPA

✅ Pipeline CI/CD GitHub Actions

✅ Configuration d’un runner self-hosted

✅ Automatisation de l’infrastructure

✅ Recréation automatique des pods en cas de crash

<p align="right">(<a href="#top">retour en haut</a>)</p>

---

# 👨‍💻 Auteur

Projet DevOps réalisé dans le cadre de l’apprentissage de l’automatisation d’infrastructure, de la containerisation et de l’orchestration Kubernetes.
