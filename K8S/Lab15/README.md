# Lab 15: Node Isolation Using Taints in Kubernetes

## 🎯 Objective
The goal of this lab is to isolate a specific node in a Kubernetes cluster using **Taints**, ensuring that only pods with the appropriate **Tolerations** can be scheduled on that node.

---

## Prerequisites
- A running Kubernetes cluster with **at least 2 nodes** (Minikube, Kubeadm, EKS, Rancher, etc.)
- `kubectl` CLI configured and connected to the cluster.

---

## 🪜 Steps

### 1️⃣ List all nodes in the cluster
```bash
kubectl get nodes
```
Example output:
```
NAME           STATUS   ROLES           AGE   VERSION
node1          Ready    control-plane   10m   v1.30.0
node2          Ready    <none>          10m   v1.30.0
```

---

### 2️⃣ Apply a taint to the second node
Taint the second node with a key-value pair `workload=worker` and effect `NoSchedule`.

```bash
kubectl taint nodes node2 workload=worker:NoSchedule
```

This command prevents any pods **without the matching toleration** from being scheduled on this node.

---

### 3️⃣ Verify the taint
Check that the taint has been successfully added:

```bash
kubectl describe node node2 | grep -A5 Taints
```

Expected output:
```
Taints: workload=worker:NoSchedule
```

---

### ✅ Verification
List all nodes again to confirm the taint was applied:

```bash
kubectl get nodes -o wide
```

---

## 📘 Notes
- **Taint** = mark a node to repel certain pods.  
- **Toleration** = allows specific pods to tolerate a node’s taint and get scheduled there.  
- Common taint effects:
  - `NoSchedule` → Prevent scheduling of new pods.
  - `PreferNoSchedule` → Prefer not to schedule pods.
  - `NoExecute` → Evict existing pods and prevent new ones.

---

## Next Step (Optional)
Create a pod and test scheduling behavior:
- Without toleration → pod won’t be scheduled on the tainted node.
- With toleration → pod can run successfully on that node.

---

