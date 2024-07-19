# partielsKUBERNETES

AUGER LOICK 4INFO R
Question 1 et 2 :
docker login // je me login sur dockerhub.
docker build -t kl8n/redispartiel:latest //construction de l'image.
docker push kl8n/redispartiel:latest // push sur le dockerhub pour utilisation.

Question 3:
Une fois mes manifeste yaml construit j'exécute : 
kubectl apply -f fastapi-deployment.yaml
kubectl apply -f redis-deployment.yaml
pour déployer mes pods.

a. Choix du Workload et du Service
Redis
Workload : Deployment
Choix : Un seul réplicat est utilisé pour Redis, suffisant pour une configuration simple.

Service : ClusterIP
Choix : Utilisé pour Redis, permettant l'accès uniquement à l'intérieur du cluster.
FastAPI

Workload : Deployment
Choix : Deux réplicas sont déployés pour assurer la haute disponibilité et répartir la charge.

Service : LoadBalancer
Choix : Utilisé pour exposer FastAPI à l'extérieur du cluster via une IP publique, avec le port 80 mappé au port 8000 du conteneur.

b. Choix faits pour la configuration de l’application
Redis
Image Docker : Utilisation de l'image officielle de Redis version 5.
Service : ClusterIP pour limiter l'accès de Redis au sein du cluster uniquement.

FastAPI
Image Docker : L'application est déployée à partir de l'image kl8n/redispartiel:latest.
Exposition extérieure : Utilisation d'un LoadBalancer pour rendre l'application accessible depuis l'extérieur du cluster.
Variable d'environnement : REDIS_HOST est configurée pour pointer vers le service Redis.


c. Dépendances éventuelles
Base de données : Redis est utilisé pour le stockage et la récupération des données.
Cache : Redis sert aussi de cache pour améliorer les performances.

d. Requests et Probes
Requests et Limits
Redis :

Requests : 256 MiB de mémoire et 250 milli-Cores de CPU.
Limits : 512 MiB de mémoire et 500 milli-Cores de CPU.
FastAPI :

Requests : 512 MiB de mémoire et 500 milli-Cores de CPU.
Limits : 1 GiB de mémoire et 1 CPU.
Probes
Liveness Probe :

Configuration : Vérifie la santé du conteneur FastAPI à /health sur le port 8000 après 30 secondes, et le port 6379 pour Redis après 30 secondes.
Readiness Probe :

Configuration : Vérifie la disponibilité du conteneur FastAPI à /health sur le port 8000 après 5 secondes, et le port 6379 pour Redis après 5 secondes.
