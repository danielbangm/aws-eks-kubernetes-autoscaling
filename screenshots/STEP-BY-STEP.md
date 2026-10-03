Project 2: Deploying Microservices on Amazon EKS 

Step1: Install required tools
→Install AWS CLI(aws –version)


→Install eksctl:
I installed eksctl for ubuntu


Step2: Set Up AWS CLI Credentials
Create an IAM user in aws console and assign it a “AdministratorAccess”


Step3: Create an EKS CLUSTER



Step4: Verify the Cluster Creation
I ran into so many errors here trying to update kubectl configuration. That's because I'm running wsl in vs code on a windows machine. had to clear bash command cache(bash -r). Anyways I eventually figured it out


Step5: Install helm 


Step6: Create a Helm Chart for your Microservices


Step7: Deploy the Microservices Using Helm


Step8: Verify the Microservices Deployment



Step9: Implement Horizontal Pod Autoscaler (HPA)
created a hpa.yml file first(touch hpa.yml)

Then I applied


Step10: Test Autoscaling Using Siege
TERMINAL 1:  Watch HPA
(kubectl get hpa -w)
Watch CPU and number of replicas change.

TERMINAL 2: Watch Pods
(kubectl get pods -w)
Watch Kubernetes create additional WordPress Pods.

TERMINAL 3 — Generate Traffic
kubectl run siege \
  --image=jstarcher/siege \
  --restart=Never \
  -- /bin/sh -c "siege -c 100 -t 5m http://10.100.8.111”


During the load test, Siege generated heavy traffic against the WordPress application, causing CPU utilization to rise from about 3% to 121%, well above the HPA target of 50%. The Horizontal Pod Autoscaler detected the increased CPU usage and automatically instructed Kubernetes to create additional WordPress pods to handle the load, demonstrating how Kubernetes can automatically scale an application when demand increases. 

