# Fresh Setup

# Use the batch file to create the certificate for the add-onn.com

kubectl create namespace aiassistant-ui-dev
kubectl create namespace aiassistant-ui-test
kubectl create namespace aiassistant-ui-prod

kubectl create namespace aiassistant-mcp-dev

# install the ingress controller.
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install ingress-nginx ingress-nginx/ingress-nginx --namespace ingress-nginx --create-namespace

# run the command and check the Extenal-IP is displayed. if displayed the load balancer is ready.
kubectl get svc ingress-nginx-controller -n ingress-nginx


helm install aiassistant-ui ./assistant-ui -f assistant-ui/values-dev.yaml --namespace aiassistant-ui-dev
helm install aiassistant-ui ./assistant-ui -f assistant-ui/values-test.yaml --namespace aiassistant-ui-test
helm install aiassistant-ui ./assistant-ui -f assistant-ui/values-prod.yaml --namespace aiassistant-ui-prod

helm install aiassistant-mcp ./mcp-backend -f mcp-backend/values-dev.yaml --namespace aiassistant-mcp-dev




# Create secret for main domain (UI)
kubectl create secret tls aiassistant-add-onn-com-tls --cert=add-onn.com.crt --key=add-onn.com.key --namespace=aiassistant-ui-dev

# Create secret for API subdomain (MCP services)
kubectl create secret tls api-add-onn-com-tls --cert=add-onn.com.crt --key=add-onn.com.key --namespace=aiassistant-mcp-dev

# to upgrade the exisiting containers
helm upgrade aiassistant-ui ./assistant-ui -f assistant-ui/values-dev.yaml --namespace aiassistant-ui-dev
helm upgrade aiassistant-mcp ./mcp-backend -f mcp-backend/values-dev.yaml --namespace aiassistant-mcp-dev

 kubectl rollout restart deployment aiassistant-ui -n aiassistant-ui-dev 
 kubectl rollout status deployment aiassistant-ui -n aiassistant-ui-dev 
 
nslookup your-load-balancer-dns-name

kubectl exec -it aiassistant-ui-749958898d-46gmz -n aiassistant-ui-dev -- cat /etc/nginx/conf.d/default.conf

kubectl run tmp --rm -it --image=nginx -- bash