# Node.js GKE CI/CD with GitHub Actions

Deploy a containerized Node.js application to Google Kubernetes Engine (GKE).

## Architecture

GitHub -> GitHub Actions -> Docker Build -> Artifact Registry -> GKE -> Kubernetes Deployment -> LoadBalancer

## Required GitHub Secrets

Create these repository secrets:

- `GCP_PROJECT_ID` = your Google Cloud project ID
- `GCP_SA_KEY` = JSON content of the GitHub Actions service account key

## GCP resources

The workflow expects:

- GKE cluster: `nodejs-gke-cluster`
- Zone: `asia-south1-a`
- Artifact Registry: `nodejs-repo`
- Artifact Registry region: `asia-south1`

## Create Artifact Registry

```bash
gcloud services enable container.googleapis.com artifactregistry.googleapis.com

gcloud artifacts repositories create nodejs-repo \
  --repository-format=docker \
  --location=asia-south1
```

## Create GKE cluster

```bash
gcloud container clusters create nodejs-gke-cluster \
  --zone asia-south1-a \
  --machine-type e2-medium \
  --num-nodes 2
```

## GitHub

Upload this project to a GitHub repository and push to the `main` branch.

The GitHub Actions workflow will build, push, and deploy automatically.

## Verify

```bash
kubectl get pods
kubectl get service nodejs-service
```

Open the external IP from the LoadBalancer service in a browser.
