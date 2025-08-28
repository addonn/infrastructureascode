# the below commands creates the required infrastructure.

cd /terraform

# initiate
terraform init -reconfigure

# store the plan - required to destory.
terraform plan -out plan

# Run the below command once the above command is successful without any warnings or errors.
# this command creates the infrastructure required on the configured cloud. 
terraform apply -auto-approve

aws ec2 describe-instances  --filters "Name=tag:eks:cluster-name,Values=aiassistant-cluster" --query "Reservations[].Instances[].InstanceId" --output table


# 1. kubectl logs (Primary method)
# View logs for a specific pod
kubectl logs <pod-name> -n <namespace>

# Follow logs in real-time
kubectl logs -f <pod-name> -n <namespace>

# View logs for all pods with a specific label
kubectl logs -l app=mcp-server -n <namespace>

# View previous container logs if pod restarted
kubectl logs <pod-name> --previous -n <namespace>

# 2. Find your MCP server pods first
# List all pods to find your MCP servers
kubectl get pods -n <namespace>

# Or filter by labels if you used them
kubectl get pods -l app=mcp-server -n <namespace>

# Get detailed pod information including events
kubectl describe pod <pod-name> -n <namespace>

# 3. Check pod events and status
# View cluster events (useful for deployment issues)
kubectl get events -n <namespace> --sort-by='.lastTimestamp'

# Check deployment status
kubectl get deployments -n <namespace>
kubectl describe deployment <deployment-name> -n <namespace>

# 4. Container-level debugging
# Execute into a running container
kubectl exec -it <pod-name> -n <namespace> -- /bin/bash

# Check container resource usage
kubectl top pods -n <namespace>