# 📊 Prometheus Setup on Google Kubernetes Engine (GKE)

## ✅ (Optional) Step: Create the Kubernetes Cluster

> If you've already created a cluster and haven't deleted it, skip this step.

### Open the Cloud Shell.

### To check existing clusters:
```bash
gcloud container clusters list
````

> If no clusters are listed, proceed to create one.

### To create a new cluster:

```bash
gcloud container clusters create my-cluster \
    --zone us-central1-a \
    --disk-type=pd-standard
```

⚠️ *Cluster creation may take 5 to 10 minutes.*

### Once the cluster is ready:

* Go to **Kubernetes Engine → Clusters** in the Cloud Console.
* Confirm that **`my-cluster`** is in a **Running** state.

---

## ✅ Step: Install Prometheus with Helm

### 1. Add Prometheus Helm repo:

```bash
helm repo add prometheus https://prometheus-community.github.io/helm-charts
```

### 2. Update Helm repo:

```bash
helm repo update
```

### 3. Install Prometheus stack:

```bash
helm install prometheus prometheus-community/kube-prometheus-stack \
    --namespace monitoring \
    --create-namespace
```

> 🎯 This installs:

* Prometheus
* Alertmanager
* Grafana

---

## ✅ Step: Check Deployment

### List pods in monitoring namespace:

```bash
kubectl get pods -n monitoring
```

### List services in monitoring namespace:

```bash
kubectl get svc -n monitoring
```

---

## ✅ Step: Access Prometheus UI via Port Forwarding

### Forward Prometheus service to local port 9090:

```bash
kubectl port-forward svc/prometheus-kube-prometheus-prometheus 9090:9090 -n monitoring
```

---

## ✅ Step: View Prometheus in Browser

1. Click on **Web Preview** in Google Cloud Shell.
2. Change the port to **9090**.
3. Click **Change and Preview**.

✅ You should now see the **Prometheus UI** in your browser.

---
## ✅ Done
