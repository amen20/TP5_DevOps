# TP5: Déploiement avec Helm et ArgoCD  

## Objectifs  
- Utiliser **Helm** pour gérer les déploiements Kubernetes.  
- Configurer **ArgoCD** pour le déploiement continu d’une app MERN.  

## Prérequis  
- Un cluster Kubernetes actif (Docker Desktop/Minikube).  
- `kubectl`, `helm` et `docker` installés.  
- Un dépôt GitHub pour stocker les **Helm Charts**.  

## 1. Installation de Helm  
Vérifiez l’installation :  
```sh
helm version
```
Si nécessaire, installez-le via [Helm Docs](https://helm.sh/docs/intro/install/).  

## 2. Création des Helm Charts  
Créez un répertoire et des charts pour MongoDB, le serveur et le client :  
```sh
mkdir mern-charts && cd mern-charts  
helm create mongodb  
helm create server  
helm create client  
```
Modifiez les fichiers `values.yaml` pour chaque composant (ports, images Docker, variables d’environnement).  

## 3. Hébergement des Charts Helm  
Ajoutez les charts à un dépôt Git :  
```sh
git init  
git add mern-charts/  
git commit -m "Ajout des charts Helm"  
git remote add origin [URL_DU_DEPOT]  
git push -u origin main  
```

## 4. Installation d’ArgoCD  
```sh
kubectl create namespace argocd  
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml  
kubectl get pods -n argocd  
```
Accédez à l’interface via **port forwarding** :  
```sh
kubectl port-forward svc/argocd-server -n argocd 8085:443  
```
Puis ouvrez [https://localhost:8080](https://localhost:8085).  

## 5. Récupération du Mot de Passe ArgoCD  
```sh
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo  
```
Utilisez **admin** comme identifiant pour vous connecter.  

## 6. Déploiement avec ArgoCD  
Dans l’interface **ArgoCD** :  
1. **Ajoutez le dépôt Git** (`Settings > Repositories`).  
2. **Créez une nouvelle application** avec :  
   - **Name** : `mern-app`  
   - **Repo URL** : `[URL_DU_DEPOT]`  
   - **Path** : `mern-charts/`  
   - **Namespace** : `default`  
   - **Sync Policy** : `Automatic`  
3. **Lancez la synchronisation** et surveillez l’état (`Healthy`, `Synced`).  

## 7. Mise à Jour Continue  
Apportez des modifications aux **Helm Charts** et poussez-les sur Git :  
```sh
git add .  
git commit -m "Mise à jour des valeurs"  
git push origin main  
```
ArgoCD mettra automatiquement à jour le déploiement.  

## 8. Accès à l’Application  
Ajoutez cette ligne à `/etc/hosts` :  
```
127.0.0.1 mern-app.local  
```
Puis ouvrez [http://mern-app.local](http://mern-app.local).  

## 9. Nettoyage  
```sh
argocd app delete mern-app  
kubectl delete namespace argocd  
```

## Capture d’écran ArgoCD  
![Screenshot from 2025-01-05 17-38-37](https://github.com/user-attachments/assets/72f3f2bb-8c30-4cb0-854e-1d6fea5ece8f)
