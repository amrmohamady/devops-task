1- Installing and configured AWS CLI
https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-mac.html

2- Installing aws-iam-authenticator
https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html

3-Installing kubectl
https://kubernetes.io/docs/tasks/tools/install-kubectl/
4- In your own console, create a ~/.aws/credentials file and put your credentials in it:
[default]
 aws_access_key_id=***********
 aws_secret_access_key=****************************

5- Set up and initialize your Terraform workspace
In your terminal, clone the following repository. It contains the example configuration used in this tutorial.
git clone https://github.com/hashicorp/learn-terraform-provision-eks-cluster

6- You can explore this repository by changing directories or navigating in your UI.
cd learn-terraform-provision-eks-cluster
Then run the following command
terraform init
terraform apply

7- Configure kubectl
Now that you've provisioned your EKS cluster, you need to configure kubectl.
Run the following command to retrieve the access credentials for your cluster and automatically configure kubectl.
aws eks --region $(terraform output -raw region) update-kubeconfig --name $(terraform output -raw cluster_name)

8- Deploy and access Kubernetes Dashboard
To verify that your cluster is configured correctly and running, you will deploy the Kubernetes dashboard and navigate to it in your local browser.
While you can deploy the Kubernetes metrics server and dashboard using Terraform, kubectl is used in this tutorial so you don't need to configure your Terraform Kubernetes Provider.

9-Deploy Kubernetes Metrics Server running the following command
wget -O v0.3.6.tar.gz https://codeload.github.com/kubernetes-sigs/metrics-server/tar.gz/v0.3.6 && tar -xzf v0.3.6.tar.gz
kubectl apply -f metrics-server-0.3.6/deploy/1.8+/

10- Deploy Kubernetes Dashboard
RUN kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta8/aio/deploy/recommended.yaml
RUN kubectl proxy
You should be able to access the Kubernetes dashboard here (http://127.0.0.1:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/).

11- Authenticate the dashboard
kubectl apply -f https://raw.githubusercontent.com/hashicorp/learn-terraform-provision-eks-cluster/master/kubernetes-dashboard-admin.rbac.yaml
Select "Token" on the Dashboard UI then copy and paste the entire token you receive into the dashboard authentication screen to sign in. You are now signed in to the dashboard for your Kubernetes cluster.

Navigate to the "Cluster" page by clicking on "Cluster" in the left navigation bar. You should see a list of nodes in your cluster.
12 - RUN kubectl get nodes -o wide
13- install Helm
https://helm.sh/docs/intro/install/
Then RUN
kubectl apply -f tiller-user.yaml
helm init --service-account tiller

