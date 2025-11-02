# Lab 16: Namespace Management and Resource Quota Enforcement

## Objective
Create a dedicated namespace called **ivolve** and enforce a **ResourceQuota** policy that limits the total number of pods within the namespace to only **2**.

---

## Prerequisites
- A running Kubernetes cluster (Minikube, Kubeadm, EKS, etc.)
- `kubectl` CLI configured and connected to the cluster.

---

## Steps

### 1️⃣ Create a new namespace
```bash
kubectl create namespace ivolve
```

Verify that the namespace has been created:
```bash
kubectl get namespaces
```
Expected output:
```
NAME          STATUS   AGE
default       Active   5m
kube-system   Active   5m
kube-public   Active   5m
ivolve        Active   2s
```

---

### 2️⃣ Create a ResourceQuota definition file
Create a file named `resource-quota.yaml` with the following content:

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: pod-limit
  namespace: ivolve
spec:
  hard:
    pods: "2"
```

---

### 3️⃣ Apply the ResourceQuota
```bash
kubectl apply -f resource-quota.yaml
```
Expected output:
```
resourcequota/pod-limit created
```

---

### 4️⃣ Verify the ResourceQuota
List all resource quotas in the `ivolve` namespace:
```bash
kubectl get resourcequota -n ivolve
```
Example output:
```
NAME        AGE   REQUEST   LIMIT
pod-limit   1m    2         -
```

You can also describe the quota for more details:
```bash
kubectl describe resourcequota pod-limit -n ivolve
```

---

## 📘 Notes
- **Namespace** helps organize and isolate resources in a Kubernetes cluster.
- **ResourceQuota** limits how many or how much of certain resources (like pods, CPU, or memory) can be used in a namespace.
- If a user tries to create more than 2 pods in the `ivolve` namespace, Kubernetes will **reject** the new pod creation request.

---
