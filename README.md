# Déploiement d’une application Gradio Dockerisée sur Cloud Run

## Prérequis

Être authentifié à Google Cloud et avoir configuré le bon projet GCP.

Personnellement, j’utilise **Cloud Shell**, qui est un shell directement fourni par Google Cloud Platform. Dans ce cas, l’authentification est déjà faite. Il suffit simplement de définir le bon **Project ID** avec la commande suivante :

```bash
gcloud config set project MON_PROJECT_ID
```

---

## Déploiement de l’application

Une **unique commande** est nécessaire pour déployer l’application Gradio, à exécuter depuis le dossier contenant le `Dockerfile` :

```bash
gcloud run deploy gradio-app \
  --source . \
  --region europe-west1 \
  --platform managed \
  --allow-unauthenticated
```

---

## Détail de la commande

* **`gradio-app`** : nom du service Cloud Run
* **`--source .`** : construit l’image Docker à partir du `Dockerfile` présent dans le dossier courant, puis la pousse automatiquement dans **Artifact Registry**
* **`--region europe-west1`** : région de déploiement (Europe de l’Ouest – proche Paris / Belgique)
* **`--platform managed`** : déploiement sur Cloud Run *fully managed*
* **`--allow-unauthenticated`** : rend le service accessible publiquement (sinon accès refusé – 403)

---

## Résultat

À la fin du déploiement, Cloud Run fournit une **URL publique** permettant d’accéder directement à l’application Gradio.

---

## Notes spécifiques à Gradio

* L’application Gradio doit écouter sur `0.0.0.0` et sur le port fourni par la variable d’environnement `PORT`
* Cloud Run injecte automatiquement `PORT=8080`
* Le binding du port se fait **dans le code Python** via `demo.launch(server_name=..., server_port=...)`

---

## Notes générales

* Cloud Run détecte automatiquement le `Dockerfile`
* L’image est buildée et stockée sans configuration manuelle d’Artifact Registry
* Le service est prêt à être utilisé immédiatement après le déploiement
