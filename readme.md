# WordPress on Kubernetes with Monitoring

Helm chart **WordPress + MySQL**  Kubernetes (Minikube EC2), 
Ingress, PVCs, StatefulSet Deployment. 
 **Prometheus + Grafana** **WP Blog – Health**.

---

- Kubernetes cluster (Minikube)
- kubectl 
- helm
- Docker + ECR
- GitHub/GitLab 

---


### 1. Ingress-NGINX
```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm install ingress-nginx ingress-nginx/ingress-nginx --set controller.service.type=NodePort -n ingress-nginx --create-namespace
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install kps prometheus-community/kube-prometheus-stack -n monitoring --create-namespace

helm install blog ./wordpress-stack -n blog --create-namespace

kubectl get pods -n blog
echo "127.0.0.1 blog.local" | sudo tee -a /etc/hosts

#port porward 
kubectl port-forward svc/ingress-nginx-controller -n ingress-nginx 30080:80

#noe in your leptop 
http://blog.local:30080

#grafana 
kubectl port-forward svc/kps-grafana -n monitoring 3000:80

in your leptop: http://localhost:3000

user: admin

pasward: prom-operator

#cleanup
helm uninstall blog -n blog
helm uninstall kps -n monitoring
helm uninstall ingress-nginx -n ingress-nginx

