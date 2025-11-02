## Lab 18: Persistent Storage Setup for Application Logging

### Objective

The goal of this lab is to configure **persistent storage** for application logs using Kubernetes **Persistent Volumes (PV)** and **Persistent Volume Claims (PVC)**.
This ensures that log data remains available even if pods are restarted or deleted.

---

###  Step 1: Create the Host Directory on Node

Before defining the PV, create the log directory on the node that will host the data.

```bash
sudo mkdir -p /mnt/app-logs
sudo chmod 777 /mnt/app-logs
```

> `/mnt/app-logs` will be the physical location where log files are stored on the host system.

---

### Step 2: Define the Persistent Volume (PV)

Create a file named **`pv.yaml`**:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: app-logs-pv
spec:
  capacity:
    storage: 1Gi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteMany
  persistentVolumeReclaimPolicy: Retain
  storageClassName: manual
  hostPath:
    path: /mnt/app-logs
```

**Details:**

* **Size:** 1Gi
* **Storage type:** hostPath
* **Path:** `/mnt/app-logs`
* **Access mode:** ReadWriteMany
* **Reclaim policy:** Retain

---

### Step 3: Define the Persistent Volume Claim (PVC)

Create a file named **`pvc.yaml`**:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: app-logs-pvc
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: manual
  resources:
    requests:
      storage: 1Gi
```

**Details:**

* Requests **1Gi** storage
* Uses the same access mode (**ReadWriteMany**) and storage class (**manual**) as the PV

---

### Step 4: Apply the YAML Files

```bash
kubectl apply -f pv.yaml
kubectl apply -f pvc.yaml
```

---

### 🔍 Step 5: Verify the Setup

Check that the PV and PVC are correctly bound:

```bash
kubectl get pv
kubectl get pvc
```

Expected output example:

```
NAME           CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                  STORAGECLASS   AGE
app-logs-pv    1Gi        RWX            Retain           Bound    default/app-logs-pvc   manual         1m
```

---

### ✅ Step 6 (Optional): Test with a Pod

You can test the storage by creating a Pod that mounts the PVC:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: log-writer
spec:
  containers:
    - name: logger
      image: busybox
      command: ["/bin/sh", "-c"]
      args: ["while true; do echo $(date) >> /var/log/app/test.log; sleep 5; done"]
      volumeMounts:
        - name: logs
          mountPath: /var/log/app
  volumes:
    - name: logs
      persistentVolumeClaim:
        claimName: app-logs-pvc
```

Apply it:

```bash
kubectl apply -f pod.yaml
```

Then verify logs are being written:

```bash
kubectl exec -it log-writer -- cat /var/log/app/test.log
```

---



---

### 📝 Notes

* Ensure the directory `/mnt/app-logs` exists **on the node**, not inside the pod.
* The `Retain` policy means the data will remain on the node even if the PVC is deleted.
