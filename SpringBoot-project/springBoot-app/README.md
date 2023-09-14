### Configure a Sonar Server locally

```
apt install unzip
adduser sonarqube
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.9.2.77730.zip
unzip *
chmod -R 755 /home/sonarqube/sonarqube-9.9.2.77730
chown -R sonarqube:sonarqube /home/sonarqube/sonarqube-9.9.2.77730
cd sonarqube-9.9.2.77730/bin/linux-x86-64/
./sonar.sh start
```

Now you can access the `SonarQube Server` on `http://<ip-address>:9000` 

### Docker Installation

Run below commands to install docker

'''
sudo apt update
sudo apt install docker.io
'''

Grant jenkins user and ubuntu user permission to docker

'''
sudo usermod -aG docker jenkins
sudo usermod -aG docker ubuntu
sudo systemctl restart docker
'''
### Amazon EKS installation

To install eksctl
'''
https://eksctl.io/introduction/#installation
'''

## To create an Amazon EKS cluster

Run a following command to create an IAM trust policy

'''
cat >eks-cluster-role-trust-policy.json <<EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "eks.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
EOF
'''

kubectl and eksctl installation

'''
https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html
https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html
'''

Create an Amazon EKS cluster role using above json

'''
aws iam create-role --role-name myAmazonEKSClusterRole --assume-role-policy-document file://"eks-cluster-role-trust-policy.json"
'''

Attach amazon EKS managed policy named AmazonEKSClusterPolicy to the above role

'''
aws iam attach-role-policy --policy-arn arn:aws:iam::aws:policy/AmazonEKSClusterPolicy --role-name myAmazonEKSClusterRole
'''

Now run the below first command to create eks cluster with name "argoCDcluster" with 2 worker nodes named worker

'''
eksctl create cluster --name argoCD --region ap-south-1 --version 1.25 --with-oidc --nodegroup-name worker --node-type t2.micro --nodes 2 --managed --ssh-access --ssh-public-key myKey
aws eks update-kubeconfig --region region-code --name my-cluster
'''

ArgoCD Installation

'''
https://operatorhub.io/operator/argocd-operator
'''
