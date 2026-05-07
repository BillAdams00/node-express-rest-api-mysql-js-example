<div id="top"></div>

<h1 align="center">Node.js Express REST API MySQL JS Example</h1>

<div align="center">
  <p align="center">
    This REST API example is a basic backend application to test basic API functions with MySQL database.
  </p>
  <a href="https://www.postman.com/workspace/node-js-express-mysql-rest-api-example/overview">View Postman Files</a>
</div>

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-application">About The Application</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li><a href="#how-to-install">How To Install</a></li>
    <li><a href="#available-scripts">Available Scripts</a></li>
    <li><a href="#postman">Postman</a></li>
  </ol>
</details>

<!-- ABOUT THE APPLICATION -->

## About The Application

This REST API example is a basic backend application to test basic API functions with MySQL database.

It is built with Node.js and Express Framework with Javascript. In addition, the applications database is MySQL, with the use of mysql2 library.

In the applicaiton we can manage user data, such as create/edit/delete a user. In addition, we can get all the users in the database.

The point of this backend application is to test CRUD operations with MySQL database.

<p align="right">(<a href="#top">back to top</a>)</p>

### Built With

-   [Node.js](https://nodejs.org/en/)
-   [Express](https://expressjs.com/)
-   [Cors](https://www.npmjs.com/package/cors)
-   [MySQL2](https://www.npmjs.com/package/mysql2)

<p align="right">(<a href="#top">back to top</a>)</p>

<!-- INSTALLATION INSTRUCTIONS -->

## How To Install

**Git clone**

```
git clone https://github.com/almoggutin/Node-Express-REST-API-MySQL-JS-Example
```

**Instructions**

-   After cloning the the repository run `npm i` in order to install all the dependencies.
-   Create an env file in the root of the project named .env and fill in the follwing variables: PORT, DB_HOST, DB_PORT, DB_USERNAME, DB_USERNAME_PASSWORD, DB_NAME.
-   In the sql directory, there are sql files that you will need to execute in order to initialize the database.

<p align="right">(<a href="#top">back to top</a>)</p>

<!--  AVAILABLE SCRIPTS -->

## Available Scripts

In the project directory, you can run:

### `npm start`

Runs the app in the production mode.\
However, this script is only meant to be run when deploying the application. The application is built, where you need to setup the env variables on the machine that you will be hosting it on or on a web hosting service, unlike in development mode.

### `npm run dev`

Runs the app in the development mode.\
Open localhost on the port you decided on in the env variables to view it in the browser.

The API will reload if you make edits with the use of nodemon.

<p align="right">(<a href="#top">back to top</a>)</p>

<!-- POSTMAN -->

## Postman

If you would like to run the files locally on your machine in the postman desktop application, included in the repository, in the `postman` directory all the files so you can import them. In addition you will have to configure env variables in postman so that you will be able to test properly everything.

<div align="center">
  <img src="./assets/postman/postman-global-env-variables.png" alt="Postman global env variables."/>
  <img src="./assets/postman/postman-jobs-env-variables.png" alt="Postman admin env variables."/>
</div>

<p align="right">(<a href="#top">back to top</a>)</p>


 TP DevOps - Déploiement complet d’une application Node.js avec Docker, Kubernetes et CI/CD

 1. Objectif du TP

Ce TP a pour objectif de mettre en place une chaîne DevOps complète autour d’une application Node.js utilisant une base de données MySQL.

L’objectif général est de partir d’un code source existant, de le conteneuriser avec Docker, de le déployer sur Kubernetes, puis de préparer une pipeline CI/CD avec GitHub Actions.

La chaîne mise en place est la suivante :

Code source → Docker → Docker Hub → Kubernetes → CI/CD

---

## 2. Environnement de travail

Le TP a été réalisé dans une machine virtuelle Linux.

Cette machine virtuelle sert d’environnement principal pour :
- installer Docker ;
- installer Kubernetes via k3s ;
- construire les images Docker ;
- déployer les manifests Kubernetes ;
- exécuter le runner self-hosted GitHub Actions.

L’utilisation d’une VM Linux permet de travailler dans un environnement proche d’un serveur réel.

---

## 3. Installation et vérification de Docker

Docker a été installé afin de permettre la création et l’exécution de conteneurs.

Docker permet d’emballer une application avec ses dépendances dans une image portable. Cette image peut ensuite être lancée sous forme de conteneur.

La vérification de Docker a été faite avec :
`bash
docker run hello-world

Cette commande permet de vérifier que Docker fonctionne correctement. Docker télécharge une image de test depuis Docker Hub, crée un conteneur, puis affiche un message de confirmation.

Une erreur de permission a été rencontrée au départ. Elle venait du fait que l’utilisateur courant n’avait pas les droits d’utiliser Docker. Le problème a été corrigé en ajoutant l’utilisateur au groupe Docker.

4. Récupération du code source

Le code source de l’application a été récupéré depuis GitHub.

L’application utilisée est une API Node.js / Express connectée à une base MySQL.

La commande utilisée est :

git clone <url-du-repo>

Git permet de récupérer le code source, mais aussi l’historique du projet. Il est utilisé car c’est l’outil standard de gestion de version dans les projets DevOps.

Après récupération du projet, la structure a été analysée afin de comprendre comment l’application fonctionne.

Les fichiers importants sont notamment :

package.json
src/
config/
sql/

Le fichier package.json est important car il indique les dépendances du projet ainsi que la commande de démarrage de l’application.

Le script de démarrage trouvé est :

"start": "cross-env NODE_ENV=production node src/index.js"

Cela signifie que l’application démarre avec :

npm start

Cette étape est essentielle avant de dockeriser une application, car Docker doit savoir quelle commande exécuter lorsque le conteneur démarre.

5. Création du Dockerfile

Un fichier Dockerfile a été créé afin de construire l’image Docker de l’application.

Le Dockerfile décrit les étapes nécessaires pour créer une image exécutable de l’application.

Exemple de Dockerfile utilisé :

FROM node:18-alpine

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
Explication du Dockerfile
FROM node:18-alpine

Cette ligne indique que l’image de base est une image Node.js.
La version alpine est utilisée car elle est plus légère, ce qui permet d’optimiser la taille finale de l’image.

WORKDIR /app

Cette ligne définit le dossier de travail dans le conteneur.

COPY package*.json ./

Cette ligne copie les fichiers package.json et package-lock.json dans l’image. Ces fichiers sont nécessaires pour installer les dépendances.

RUN npm install

Cette ligne installe les dépendances Node.js nécessaires au fonctionnement de l’application.

COPY . .

Cette ligne copie le reste du code source dans l’image.

EXPOSE 3000

Cette ligne indique que l’application utilise le port 3000.

CMD ["npm", "start"]

Cette ligne définit la commande exécutée au démarrage du conteneur.

6. Optimisation avec .dockerignore

Un fichier .dockerignore a été ajouté afin d’éviter de copier des fichiers inutiles dans l’image Docker.

Exemple :

node_modules
.git
.gitignore
README.md
.env

Ce fichier permet :

de réduire la taille de l’image ;
d’accélérer le build ;
d’éviter de copier des fichiers sensibles ;
de garder une image plus propre.

Sans .dockerignore, Docker pourrait copier des fichiers inutiles comme .git, node_modules ou des fichiers d’environnement.

7. Build de l’image Docker

L’image Docker a été construite avec la commande :

docker build -t mon-api .

Le point . à la fin indique à Docker d’utiliser le dossier courant comme contexte de build.

Cette commande lit le Dockerfile, installe les dépendances, copie le code source, puis crée une image Docker appelée mon-api.

Une fois le build terminé, l’image est visible avec :

docker images
8. Publication de l’image sur Docker Hub

Après avoir construit l’image, celle-ci a été poussée sur Docker Hub.

Docker Hub est un registre d’images Docker. Il permet de stocker et partager les images.

Les étapes réalisées sont :

docker login
docker tag mon-api <dockerhub-username>/mon-api:latest
docker push <dockerhub-username>/mon-api:latest

L’image publiée sur Docker Hub sera ensuite utilisée par Kubernetes pour déployer l’API.

9. Installation et utilisation de k3s

k3s a été utilisé comme distribution légère de Kubernetes.

Kubernetes sert à orchestrer les conteneurs. Il permet de déployer, redémarrer, scaler et gérer les applications conteneurisées.

Dans ce TP, la VM Linux joue le rôle de cluster Kubernetes local.

Un cluster Kubernetes est un ensemble de machines qui exécutent des applications conteneurisées. Dans ce cas, le cluster contient un seul nœud : la VM.

10. Déploiement de MySQL sur Kubernetes

La base de données MySQL a été déployée dans Kubernetes à l’aide d’un manifest YAML.

Un manifest YAML permet de décrire l’état souhaité dans Kubernetes.

Pour MySQL, plusieurs ressources ont été créées :

un Secret ;
un PersistentVolumeClaim ;
un Deployment ;
un Service.
Secret

Le Secret permet de stocker des informations sensibles comme le mot de passe de la base de données.

Cela évite d’écrire directement les mots de passe en clair dans les fichiers de configuration.

PersistentVolumeClaim

Le PersistentVolumeClaim permet de demander un stockage persistant à Kubernetes.

La persistance signifie que les données doivent rester disponibles même si le pod MySQL redémarre.

Sans persistance, les données de la base pourraient être perdues à chaque redémarrage du pod.

Deployment MySQL

Le Deployment MySQL permet de lancer un pod contenant le conteneur MySQL.

Un Deployment permet à Kubernetes de gérer le cycle de vie des pods :

création ;
redémarrage ;
remplacement en cas de crash.
Service MySQL

Le Service MySQL permet à l’API de contacter la base de données avec un nom stable.

Le service s’appelle :

mysql

L’API peut donc utiliser :

DB_HOST=mysql

Le type utilisé est :

type: ClusterIP

Cela signifie que MySQL est accessible uniquement à l’intérieur du cluster.

11. Déploiement de l’API sur Kubernetes

L’API a également été déployée sur Kubernetes à l’aide d’un manifest YAML.

Le fichier contient :

un Deployment pour lancer l’API ;
un Service pour rendre l’API joignable à l’intérieur du cluster.

Le Deployment utilise l’image Docker publiée sur Docker Hub.

Exemple :

image: <dockerhub-username>/mon-api:latest

Les variables d’environnement permettent à l’API de se connecter à MySQL :

- name: DB_HOST
  value: mysql
- name: DB_PORT
  value: "3306"
- name: DB_USERNAME
  value: root
- name: DB_USERNAME_PASSWORD
  value: root
- name: DB_NAME
  value: test

L’API ne contacte pas directement le pod MySQL.
Elle contacte le Service Kubernetes mysql, qui redirige ensuite vers le bon pod MySQL.

12. Communication entre l’API et MySQL

La communication entre l’API et MySQL se fait grâce au système DNS interne de Kubernetes.

Lorsqu’un Service est créé avec le nom mysql, Kubernetes crée automatiquement un nom DNS interne.

L’API peut donc joindre la base de données avec :

mysql:3306

Le fonctionnement est le suivant :

Pod API → Service mysql → Pod MySQL

Le Service permet de ne pas dépendre de l’adresse IP du pod MySQL, car cette adresse peut changer si le pod redémarre.

13. Mise en place du HPA

L’énoncé demandait d’avoir au moins 1 pod en permanence et jusqu’à 3 pods en cas de pic de charge.

Pour cela, un HorizontalPodAutoscaler a été créé.

Le HPA permet à Kubernetes d’augmenter ou de réduire automatiquement le nombre de pods en fonction de la charge.

Exemple :

charge normale → 1 pod
charge élevée → jusqu’à 3 pods

Le HPA cible le Deployment de l’API.

Il est configuré avec :

minimum : 1 pod ;
maximum : 3 pods ;
métrique : CPU.

Le HPA utilise les métriques fournies par Kubernetes pour décider s’il doit créer plus de pods.

14. Ressources CPU et mémoire

Pour que le HPA puisse fonctionner correctement, des ressources ont été définies dans le Deployment de l’API.

Exemple :

resources:
  requests:
    cpu: "100m"
    memory: "128Mi"
  limits:
    cpu: "500m"
    memory: "256Mi"

Les requests indiquent les ressources minimales demandées par le pod.

Les limits indiquent les ressources maximales que le pod peut utiliser.

Cela permet à Kubernetes de mieux gérer les ressources disponibles et de prendre des décisions de scaling.

15. Vérifications Kubernetes

Plusieurs commandes ont été utilisées pour vérifier l’état du déploiement :

sudo k3s kubectl get pods

Permet de voir les pods en cours d’exécution.

sudo k3s kubectl get svc

Permet de voir les services Kubernetes.

sudo k3s kubectl get pvc

Permet de vérifier que le volume persistant est bien créé.

sudo k3s kubectl get hpa

Permet de vérifier que l’autoscaling est bien configuré.

sudo k3s kubectl top pods

Permet de voir la consommation CPU et mémoire des pods.

16. Mise en place de GitHub

Le projet a été envoyé sur un dépôt GitHub personnel afin de pouvoir mettre en place la pipeline CI/CD.

Les commandes Git utilisées sont :

git add .
git commit -m "Initial commit - DevOps project"
git push

GitHub n’accepte plus l’authentification par mot de passe classique.
Un Personal Access Token GitHub a donc été utilisé pour pousser le code.

Le token doit avoir la permission repo afin d’autoriser l’écriture dans le dépôt.

17. Runner self-hosted GitHub Actions

Un runner self-hosted a été installé sur la VM.

Un runner est une machine qui exécute les jobs GitHub Actions.

Dans ce TP, le runner self-hosted est la VM Linux.
Il permet d’exécuter les commandes de build et de déploiement directement dans l’environnement où Docker et Kubernetes sont installés.

Le runner a été téléchargé depuis GitHub, extrait, configuré puis lancé avec :

./run.sh

Lorsque le runner affiche :

Listening for Jobs

cela signifie qu’il est connecté à GitHub et prêt à exécuter une pipeline.

18. Début de la pipeline CI/CD

Un fichier GitHub Actions doit être créé dans :

.github/workflows/ci-cd.yml

Ce fichier représente la pipeline CI/CD.

La pipeline a pour objectif de s’exécuter automatiquement à chaque modification de la branche main.

Elle devra :

récupérer le code ;
se connecter à Docker Hub ;
builder l’image Docker ;
pousser l’image sur Docker Hub ;
déployer les manifests Kubernetes.

Les secrets GitHub Actions nécessaires sont :

DOCKER_USERNAME ;
DOCKER_PASSWORD.

Ces secrets permettent à la pipeline de se connecter à Docker Hub sans exposer les identifiants dans le code.

19. Problèmes rencontrés

Plusieurs problèmes ont été rencontrés pendant la réalisation du TP.

Problème de permission Docker

Au départ, Docker refusait l’exécution des commandes sans sudo.
Le problème venait du fait que l’utilisateur n’était pas dans le groupe Docker.

Problèmes DNS

Lors du build Docker, Docker n’arrivait pas à résoudre l’adresse de Docker Hub.
Cela venait d’un problème DNS dans la VM.

Erreurs YAML

Plusieurs erreurs Kubernetes ont été rencontrées à cause de l’indentation YAML.

YAML est très strict : une mauvaise indentation ou une mauvaise casse peut empêcher Kubernetes de lire le fichier.

Exemple d’erreur corrigée :

containerport

a été corrigé en :

containerPort
Problèmes GitHub

GitHub refusait le push car l’authentification par mot de passe n’est plus supportée.
Un token GitHub a été utilisé à la place du mot de passe.

20. Conclusion

Ce TP a permis de mettre en pratique une chaîne DevOps complète.

Les principales compétences travaillées sont :

utilisation de Linux ;
conteneurisation avec Docker ;
publication d’image sur Docker Hub ;
déploiement Kubernetes avec k3s ;
gestion de services internes ;
persistance des données avec PVC ;
autoscaling avec HPA ;
gestion de version avec Git ;
préparation d’une pipeline CI/CD avec GitHub Actions ;
installation d’un runner self-hosted.

La chaîne DevOps obtenue est la suivante :

Code source
→ Dockerfile
→ Image Docker
→ Docker Hub
→ Kubernetes
→ MySQL persistant
→ HPA
→ GitHub Actions
→ Runner self-hosted

Ce TP montre comment automatiser progressivement le cycle de vie d’une application, depuis son code source jusqu’à son déploiement sur une infrastructure Kubernetes.
