## AWS Fargate Web App Tutorial

This is the source code for the AWS Fargate tutorial on https://www.learnaws.org/.

eksctl create cluster \
--name fargate-tutorial-cluster \
--version 1.14 \
--region us-east-2 \
--fargate \
--alb-ingress-access
eksctl utils update-cluster-logging --region=us-east-2 --cluster=fargate-tutorial-cluster
eksctl utils describe-stacks --region=us-east-2 --cluster=fargate-tutorial-cluster
kubectl get nodes
eksctl get cluster --region us-east-2 --name fargate-tutorial-cluster -o yaml
wget https://raw.githubusercontent.com/kubernetes-sigs/aws-alb-ingress-controller/v1.1.4/docs/examples/rbac-role.yaml
wget https://raw.githubusercontent.com/kubernetes-sigs/aws-alb-ingress-controller/v1.1.4/docs/examples/alb-ingress-controller.yaml
kubectl apply -f rbac-role.yaml
kubectl apply -f alb-ingress-controller.yaml
kubectl logs -n kube-system $(kubectl get po -n kube-system | egrep -o "alb-ingress[a-zA-Z0-9-]+") 

aws ecr create-repository --repository-name fargate-tutorial
123133550781

eksctl create fargateprofile --namespace python-web --cluster fargate-tutorial-cluster --region us-east-2

kubectl apply -f namespace.yaml
kubectl get ns

kubectl apply -f service.yaml
kubectl get svc -n python-web

kubectl apply -f deployment.yaml
kubectl get pods -n python-web

kubectl apply -f ingress.yaml
kubectl describe ing -n python-web python-web

curl http://e577236e-pythonweb-pythonw-0d03-1301295728.us-east-2.elb.amazonaws.com

eksctl delete cluster --region=us-east-2 --name fargate-tutorial-cluster
