## Lab 19: StatefulSet Deployment with Headless Service

###  **Objective**

Deploy a MySQL database using a **StatefulSet** with a **Headless Service** in Kubernetes.
The StatefulSet should:

* Run **1 replica** of MySQL.
* Consume the root password from a **Secret**.
* Include a **toleration** for taint `workload=worker:NoSchedule`.
* Use a **PersistentVolumeClaim** mounted to `/var/lib/mysql`.
* Be accessible through a **headless service** (`clusterIP: None`).

---

### **Step 1 — Create Secret**

File: `mysql-secret.yaml`

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mysql-secret
type: Opaque
data:
  # echo -n 'mypassword' | base64
  mysql-root-password: bXlwYXNzd29yZA==
```

---

###  **Step 2 — Create Persistent Volume**

File: `mysql-pv.yaml`

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mysql-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /mnt/mysql-data
```

> **Note:**
> On the worker node, run:
>
> ```bash
> sudo mkdir -p /mnt/mysql-data
> sudo chmod 777 /mnt/mysql-data
> ```

---

###  **Step 3 — Create StatefulSet**

File: `mysql-statefulset.yaml`

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
spec:
  serviceName: "mysql-headless"
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      tolerations:
      - key: "workload"
        operator: "Equal"
        value: "worker"
        effect: "NoSchedule"
      containers:
      - name: mysql
        image: mysql:8.0
        ports:
        - containerPort: 3306
          name: mysql
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-secret
              key: mysql-root-password
        volumeMounts:
        - name: mysql-storage
          mountPath: /var/lib/mysql
  volumeClaimTemplates:
  - metadata:
      name: mysql-storage
    spec:
      accessModes: ["ReadWriteOnce"]
      resources:
        requests:
          storage: 1Gi
```

---

###  **Step 4 — Create Headless Service**

File: `mysql-headless-svc.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mysql-headless
spec:
  clusterIP: None
  selector:
    app: mysql
  ports:
  - port: 3306
    name: mysql
```

---

### 🚀 **Step 5 — Apply All Files**

```bash

kubectl apply -f mysql-secret.yaml

kubectl apply -f mysql-pv.yaml

kubectl apply -f mysql-statefulset.yaml

kubectl apply -f mysql-headless-svc.yaml
```

---

### 🔍 **Step 6 — Verify Deployment**

```bash
kubectl get statefulsets

kubectl get pods -l app=mysql

kubectl get pvc

kubectl get pv
```

---

### 🧠 **Step 7 — Connect to MySQL**

Access the pod shell:

```bash
kubectl exec -it mysql-0 -- sh
```

Then connect to MySQL:

```bash
mysql -u root -p
# Enter: mypassword
```

Check databases:

```sql
SHOW DATABASES;
```

---

### **Expected Result**

* A running MySQL StatefulSet with 1 replica.
* PVC created and bound to PV.
* Headless service configured (`clusterIP: None`).
* MySQL accessible from within the cluster.
