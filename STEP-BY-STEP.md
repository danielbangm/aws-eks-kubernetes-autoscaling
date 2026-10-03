## Step1: Install required tools
→Install AWS CLI(aws –version)
<img width="783" height="94" alt="p1" src="https://github.com/user-attachments/assets/d6e3e0b0-dc5b-475a-ba1a-13f9ca5973e6" />

→Install eksctl:
I installed eksctl for ubuntu
<img width="791" height="280" alt="p2" src="https://github.com/user-attachments/assets/2bc07bec-cc2a-40ee-acce-0411e1c1e920" />

## Step2: Set Up AWS CLI Credentials
Create an IAM user in aws console and assign it a “AdministratorAccess”
<img width="789" height="161" alt="p3" src="https://github.com/user-attachments/assets/1a28b7e0-5568-4bcc-97c8-f1c097b85783" />

## Step3: Create an EKS CLUSTER
<img width="770" height="311" alt="p4" src="https://github.com/user-attachments/assets/a258458d-02f9-45b3-94e1-35a4aa5f404e" />

## Step4: Verify the Cluster Creation
I ran into so many errors here trying to update kubectl configuration. That's because I'm running wsl in vs code on a windows machine. had to clear bash command cache(bash -r). Anyways I eventually figured it out
<img width="786" height="324" alt="p5" src="https://github.com/user-attachments/assets/dbcf0524-3666-4977-a08a-efa3541b3004" />
<img width="793" height="135" alt="p6" src="https://github.com/user-attachments/assets/bce9da9f-311a-4484-90ee-05503ee9386c" />

## Step5: Install helm 
<img width="788" height="259" alt="p7" src="https://github.com/user-attachments/assets/10070451-8932-436e-b373-bee2cad5c7d1" />

## Step6: Create a Helm Chart for your Microservices
<img width="787" height="384" alt="p8" src="https://github.com/user-attachments/assets/177233d5-5460-4776-b532-c8d290e16b63" />
<img width="777" height="392" alt="p9" src="https://github.com/user-attachments/assets/69892f6e-00af-410f-86a4-ca6fd3997f2e" />
<img width="797" height="378" alt="p10" src="https://github.com/user-attachments/assets/ca67530d-93ac-4c6a-a384-7e79dcc7e635" />
<img width="788" height="320" alt="p11" src="https://github.com/user-attachments/assets/28ba3f37-616d-4f90-9d25-2f9457c7c5ec" />

## Step7: Deploy the Microservices Using Helm
<img width="830" height="187" alt="step7" src="https://github.com/user-attachments/assets/3ebb8562-cfc1-4a24-9b76-9d0ee492c302" />

## Step8: Verify the Microservices Deployment
<img width="791" height="209" alt="step8" src="https://github.com/user-attachments/assets/a19f30ea-1a75-4d1f-a014-c107bffc39c3" />

## Step9: Implement Horizontal Pod Autoscaler (HPA)
created a hpa.yml file first **touch hpa.yml**

<img width="519" height="316" alt="step9" src="https://github.com/user-attachments/assets/d7315dc1-ec0a-438d-af8c-5daf5d69157c" />

**Then I applied**

<img width="786" height="114" alt="step9a" src="https://github.com/user-attachments/assets/2dcdab5f-ff7c-465e-9932-ccac0c5ca247" />

## Step10: Test Autoscaling Using Siege
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

<img width="790" height="241" alt="step10" src="https://github.com/user-attachments/assets/f47e6111-5024-4b98-8cba-97557f78501e" />

## During the load test, Siege generated heavy traffic against the WordPress application, causing CPU utilization to rise from about 3% to 121%, well above the HPA target of 50%. The Horizontal Pod Autoscaler detected the increased CPU usage and automatically instructed Kubernetes to create additional WordPress pods to handle the load, demonstrating how Kubernetes can automatically scale an application when demand increases. 

