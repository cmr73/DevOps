# 📊 Grafana Setup on Google Kubernetes Engine (GKE)

## ✅ (Optional) Step: Create the Kubernetes Cluster

> If you've already created a cluster and haven't deleted it, skip this step.

### Open the Cloud Shell.

### Check existing clusters:
```bash
gcloud container clusters list
````

> If no clusters are listed, create one:

### Create a new cluster:

```bash
gcloud container clusters create my-cluster \
    --zone us-central1-a \
    --disk-type=pd-standard
```

⚠️ *Cluster creation may take 5 to 10 minutes.*

### Once the cluster is ready:

* Go to **Kubernetes Engine → Clusters** in the Google Cloud Console.
* Confirm that `my-cluster` is in **Running** state.

---

## ✅ Step: Access Grafana

### 1. Get Grafana Admin Username:

```bash
kubectl get secret prometheus-grafana -n monitoring -o jsonpath="{.data.admin-user}" | base64 --decode ; echo
```

> This will output the username (usually `admin`).

### 2. Get Grafana Admin Password:

```bash
kubectl get secret prometheus-grafana -n monitoring -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```

> This will output the default password (usually `prom-operator`).

---

## ✅ Step: Port Forward to Access Grafana UI

### Run the following command:

```bash
kubectl port-forward svc/prometheus-grafana 3000:80 -n monitoring
```

---

## ✅ Step: View Grafana in Browser

1. In Google Cloud Shell, click on **Web Preview**.
2. Change the port to `3000`.
3. Click **Change and Preview**.

### ✅ Login Credentials:

* **Username:** `admin`
* **Password:** `prom-operator` (or as retrieved from the command above)

🎉 You should now be able to access the **Grafana dashboard** in your browser.

---
### ✅Done
