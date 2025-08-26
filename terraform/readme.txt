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