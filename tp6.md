# Rapport TP6 : Monitoring Kubernetes avec Prometheus et Grafana

## 1. Préparation de l'environnement

J'ai commencé par vérifier que mon cluster Kubernetes était opérationnel et que Helm était correctement installé :
- Le cluster était fonctionnel avec `kubectl cluster-info` et `kubectl get nodes`
- Helm était déjà installé, confirmé avec `helm version`

## 2. Installation de Prometheus

J'ai suivi ces étapes pour installer Prometheus :
- Création d'un namespace dédié "monitoring"
- Ajout des dépôts Helm nécessaires (prometheus-community et kube-state-metrics)
- Installation du chart Prometheus v26.0.0 avec NodePort
- Vérification des pods en exécution dans le namespace monitoring

## 3. Exploration de kube-state-metrics

Kube-state-metrics a été installé automatiquement avec le chart Prometheus. J'ai pu observer les métriques de base du cluster via l'interface Lens, notamment :
- État des nodes
- Statut des pods
- Utilisation des ressources

## 4. Déploiement de l'application goprom

Pour déployer l'application d'exemple :
- Clone du dépôt k8s-deployment-strategies
- Navigation vers le répertoire de la stratégie "recreate"
- Application du fichier app-v1.yaml

## 5. Observation de l'application

L'application expose plusieurs endpoints :
- Port 8080 : Application web principale
- Port 8086 : Endpoints de santé (/live et /ready)
- Port 9101 : Métriques Prometheus (/metrics)

J'ai exposé les services goprom et goprom-metrics pour accéder à l'application et ses métriques.

## 6. Configuration de Prometheus

Configuration de Prometheus pour collecter les métriques :
- Modification du service en NodePort
- Exposition du service Prometheus
- Vérification de la collecte des métriques avec la requête PromQL

![Screenshot from 2025-01-05 18-42-23](https://github.com/user-attachments/assets/7d20e86f-0953-45f2-b008-85f1f4d6fd94)


## 7. Installation de Grafana

Installation et configuration de Grafana :
- Création du secret pour les credentials admin
- Ajout du dépôt Helm Grafana
- Installation de Grafana v8.6.4 avec NodePort
- Configuration de la source de données Prometheus

## 8. Création du Dashboard Grafana

Création d'un dashboard personnalisé :
- Ajout d'un panel de type Graph
- Configuration de la requête PromQL pour visualiser les requêtes HTTP
- Personnalisation de la légende pour afficher la version
  
![Screenshot from 2025-01-05 18-53-30](https://github.com/user-attachments/assets/6c68efd3-f939-49ba-841c-5100c1cde209)



## 9. Dashboard Kubernetes

Import et configuration du dashboard Kubernetes (ID 18283) :
- Téléchargement du fichier JSON
- Import dans Grafana
- Configuration avec la source de données Prometheus
  
![Screenshot from 2025-01-05 19-00-53](https://github.com/user-attachments/assets/d963a61f-480f-4c3a-a3cb-2b47aa467988)


## 10. Tests des stratégies de déploiement

Test des différentes stratégies :
- Déploiement de app-v2.yaml
- Observation des changements en temps réel
- Analyse des impacts sur les métriques

![Screenshot from 2025-01-05 19-03-01](https://github.com/user-attachments/assets/7e07aa53-640b-4e73-b954-ee3dce73f178)


## Conclusion

Ce TP m'a permis de :
- Mettre en place une stack de monitoring complète
- Comprendre l'interaction entre Prometheus et Grafana
- Observer en temps réel le comportement d'une application lors de ses déploiements
- Maîtriser les outils de visualisation des métriques Kubernetes
