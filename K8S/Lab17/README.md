# Lab 17: Managing Configuration and Sensitive Data with ConfigMaps and Secrets

## Objective
Define and manage application configuration and sensitive data in Kubernetes using **ConfigMaps** and **Secrets**.

---

## Prerequisites
- A running Kubernetes cluster (Minikube, Kubeadm, EKS, etc.)
- `kubectl` CLI configured and connected to the cluster.

---

## Steps

### 1️⃣ Create a ConfigMap
Define a ConfigMap to store non-sensitive MySQL configuration variables.

File: `mysql-configmap.yaml`
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: mysql-config
  namespace: ivolve
data:
  DB_HOST: mysql-service
  DB_USER: ivolve_user
Apply the file:

bash
Copy code
kubectl apply -f mysql-configmap.yaml
Verify:

bash
Copy code
kubectl get configmap -n ivolve
kubectl describe configmap mysql-config -n ivolve
2️⃣ Create a Secret
Define a Secret to store sensitive MySQL credentials securely.

Before creating the Secret, encode the values in base64 format:

bash
Copy code
echo -n "password123" | base64
# cGFzc3dvcmQxMjM=

echo -n "rootpass" | base64
# cm9vdHBhc3M=
File: mysql-secret.yaml

yaml
Copy code
apiVersion: v1
kind: Secret
metadata:
  name: mysql-secret
  namespace: ivolve
type: Opaque
data:
  DB_PASSWORD: cGFzc3dvcmQxMjM=
  MYSQL_ROOT_PASSWORD: cm9vdHBhc3M=
Apply the file:

bash
Copy code
kubectl apply -f mysql-secret.yaml
Verify the secret creation:

bash
Copy code
kubectl get secret -n ivolve
kubectl describe secret mysql-secret -n ivolve
📘 Notes
ConfigMap is used to store non-sensitive configuration data (like environment variables or file paths).

Secret is used to store sensitive data (like passwords, tokens, or keys) in base64 encoded form.

When mounted into pods, Kubernetes automatically decodes the data for use by containers.

Secrets are stored in etcd, so access should be restricted for better security.

🧩 Next Step (Optional)
Use these ConfigMap and Secret objects in a pod specification as environment variables:

yaml
Copy code
envFrom:
  - configMapRef:
      name: mysql-config
  - secretRef:
      name: mysql-secret