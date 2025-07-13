## 📘 CKA Exam Kubectl Cheat Sheet

---

### 📚 Official Resources

- [CKA KodeKloud GitHub Course](https://github.com/kodekloudhub/certified-kubernetes-administrator-course)
- [Kubernetes Docs](https://kubernetes.io/docs/)
- [CKA Kubeadm Upgrade](https://v1-25.docs.kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/)

---

## 🔧 Aliases

Add to your `~/.bashrc`:
```bash
alias k=kubectl
alias kcr='kubectl create'
alias ka='kubectl apply -f'
alias kg='kubectl get'
alias ke='kubectl edit'
alias kd='kubectl describe'
alias kdd='kubectl delete'
alias kgp='kubectl get pods'
alias kgd='kubectl get deployments'
alias kgpvc='kubectl get pvc'
alias kgpv='kubectl get pv'
alias fg='--force --grace-period=0'
alias do='--dry-run=client -o yaml'
alias oy='-o yaml'

# Enable autocomplete
echo 'complete -o default -F __start_kubectl k' >>~/.bashrc
```

---

## 📦 Pods

```bash
kubectl run nginx --image=nginx
kubectl run redis --image=redis:alpine -l='tier=db'
kubectl run custom-nginx --image=nginx --port=8080
kubectl run webapp-color --image=kodekloud/webapp-color -l=name=webapp-color --env="APP_COLOR=green"
kubectl scale rs my-replica-set --replicas=5
```

**[CKA] Pod YAML:**
```bash
kubectl run nginx --image=nginx --dry-run=client -o yaml > pod.yaml
```

---

## 📄 Deployments

```bash
kubectl create deployment nginx --image=nginx
kubectl create deployment nginx --image=nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml
kubectl scale deployment nginx --replicas=5
kubectl set image deployment nginx nginx=nginx:1.19
kubectl rollout status deployment nginx
kubectl rollout undo deployment nginx
```

**Custom column output:**
```bash
kubectl get deployments -o custom-columns=DEPLOYMENT:.metadata.name,IMAGE:.spec.template.spec.containers[].image
```

---

## 🌐 Services

```bash
kubectl expose deployment nginx --port=80 --type=NodePort
kubectl create service clusterip redis --tcp=6379:6378 --dry-run=client -o yaml
kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml
```

---

## 🧭 Labels & Selectors

```bash
kubectl get pods --selector env=dev
kubectl get all --selector env=prod,bu=finance
```

---

## 🚫 Taints & Tolerations

```bash
kubectl taint nodes node1 spray=mortein:NoSchedule
kubectl taint nodes node1 spray=mortein:NoSchedule-
```

---

## 📌 NodeSelector & Affinity

```bash
kubectl label node node1 size=large
```

---

## 🧱 Static Pods

```bash
kubectl run static-busybox --image=busybox --restart=Never --dry-run=client -o yaml --command -- sleep 1000 > /etc/kubernetes/manifests/static-busybox.yaml
```

---

## 📋 ConfigMaps

```bash
kubectl create configmap webapp-config --from-literal=APP_COLOR=blue
kubectl describe configmap webapp-config
```

---

## 🔐 Secrets

```bash
kubectl create secret generic db-secret --from-literal=username=admin
```

---

## ⚖️ RBAC

```bash
kubectl create role developer --verb=get,list,watch --resource=pods --namespace=default
kubectl create rolebinding dev-binding --role=developer --user=dev-user --namespace=default
kubectl auth can-i create deployments --as=dev-user
```

---

## 🔐 ClusterRoles

```bash
kubectl create clusterrole pod-reader --verb=get,list,watch --resource=pods
kubectl create clusterrolebinding read-pods-global --clusterrole=pod-reader --user=admin
```

---

## 🧪 Troubleshooting

```bash
kubectl get nodes
kubectl describe pod <pod-name>
kubectl logs <pod-name>
sudo journalctl -u kubelet
kubectl exec -it <pod-name> -- /bin/sh
```

---

## 🧩 ETCD Backup

```bash
ETCDCTL_API=3 etcdctl \
--endpoints=https://127.0.0.1:2379 \
--cacert=/etc/kubernetes/pki/etcd/ca.crt \
--cert=/etc/kubernetes/pki/etcd/server.crt \
--key=/etc/kubernetes/pki/etcd/server.key \
snapshot save /opt/snapshot.db
```

---

## 🔐 TLS Certificates

```bash
kubectl get csr
kubectl certificate approve <csr-name>
openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout
```

---

## 🔄 Kubeconfig & Context

```bash
kubectl config get-contexts
kubectl config use-context <context-name>
kubectl config view
```

---

## 🔍 JSONPath

```bash
kubectl get pods -o jsonpath='{.items[*].metadata.name}'
```

---

## 🛠️ Debug Pods

```bash
kubectl run --rm -ti netshoot --image=nicolaka/netshoot -- bash
```

---

## 📦 Storage

```bash
kubectl get pv
kubectl get pvc
kubectl describe pvc <pvc-name>
```

---

## 🌐 DNS

```bash
kubectl exec -it <pod> -- nslookup <service-name>
```

---

## ⚙️ System Services

```bash
sudo systemctl status kubelet
sudo journalctl -u kubelet
```

---

## 🎓 Final Tips

- Practice with `--dry-run=client -o yaml` + save to file.
- Use `kubectl explain` to understand object fields.
- Stick to official documentation during the exam.
- Use aliases and YAML generation wherever possible.

---

Good luck with your CKA preparation! 🧠💪
