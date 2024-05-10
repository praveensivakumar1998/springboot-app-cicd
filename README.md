# Jenkins Pipeline for Java based application using Maven, SonarQube, operators, Argo CD and Kubernetes
![image](https://github.com/praveensivakumar1998/springboot-app-cicd/assets/108512714/8e16f73e-1c8b-4c3f-b7e0-87e8c09d208a)


CI/CD, or continuous integration and continuous delivery, is a software development practice that's a key part of DevOps methodology. CI/CD automates the process of code integration and deployment, which helps speed up the process of getting new features, bug fixes, and enhancements from development to production

- clone this repository https://github.com/praveensivakumar1998/springboot-app-cicd.git

## Prerequistes:
* Java Application in your Code repository
* Jenkins
* Sonarqube
* Docker
* Kubernetes cluster
* ArgoCD operator
* ArgoCD server

## Contionous Integration Steps:
* Create EC2 instance for Jenkins
  ![image](https://github.com/praveensivakumar1998/springboot-app-cicd/assets/108512714/c054578d-a751-46e8-a5a2-c6c3b40f69ca)
* Initiate and install neccessary cmds
```
sudo apt update -y
sudo apt install unzip -y
sudo apt install net-tools -y
```
* Run the below commands to install Java and Jenkins
```
sudo apt install openjdk-11-jre -y

curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```
* Validate the Installations
```
java --version
```
```
sudo systemctl enable Jenkins
sudo systemctl status Jenkins
```
![image](https://github.com/praveensivakumar1998/springboot-app-cicd/assets/108512714/cf5c1694-5023-47dd-8c14-b7bba08e7d08)

* Configure Jenkins in http://public-ip:8080/
* Install neccesary plugins in Jenkins console.
  * *Docker pipeline* plugin
  * *SonarQube Scanner* plugin
* Install Sonarqube in Jenkins server
```
sudo su 
adduser sonarqube
sudo su - sonarqube
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.4.0.54424.zip
chmod -R 755 /home/sonarqube/sonarqube-9.4.0.54424
chown -R sonarqube:sonarqube /home/sonarqube/sonarqube-9.4.0.54424
cd sonarqube-9.4.0.54424/bin/linux-x86-64/
./sonar.sh start
```
### Output:
![image](https://github.com/praveensivakumar1998/springboot-app-cicd/assets/108512714/54472d8c-022e-4beb-b6d8-9f80a15e71d4)

* Configure Sonarqube server http://publicip:9000/ - username as *admin* and password as *admin*

![image](https://github.com/praveensivakumar1998/springboot-app-cicd/assets/108512714/a7b0b23c-6f8c-41fc-955d-44d7477d8fd2)
* Generate token for Jenkins Code Analysis
    * Administrator > My Account > Security > Generate Tokens
    ![sonarqube cred](https://github.com/praveensivakumar1998/springboot-app-cicd/assets/108512714/55a6b409-c4ab-4c5d-8d16-b95ed4c688bb)
* Update the sonarqube server domain in your Github repo JenkinsFile [spring-boot-app/JenkinsFile ]
![image](https://github.com/praveensivakumar1998/springboot-app-cicd/assets/108512714/af116414-b73e-4514-8cfc-16a1569c3c9f)


* Install Docker for Build and test in Jenkins server
```
sudo apt install docker.io -y

sudo usermod -aG docker Jenkins
sudo usermod -aG docker ubuntu
```
* Update necessary Credentials in Jenkins Console [ Login to Jenkins console > Manage Jenkins > Credentials > System > Global credentials (unrestricted) > Add credentials]
  * Github (update the commit)
  * Sonarqube token (for Code analysis)
  * Dockehub credentials (for push Docker image to your DockerHub)
  ![jenkins credentilas](https://github.com/praveensivakumar1998/springboot-app-cicd/assets/108512714/9faded5e-d1ac-42d5-bc02-216600f2d48f)

* Create Pipeline job in a Jenkins Dashboard and update your JenkinsFile in a Pipeline
  ![image](https://github.com/praveensivakumar1998/springboot-app-cicd/assets/108512714/61bf7e7d-6ffd-48d9-addb-2c9809c7237c)
  
* Build your Pipeline project
  ![image](https://github.com/praveensivakumar1998/springboot-app-cicd/assets/108512714/e59df827-da13-43a0-83e8-78183bce808c)
# Now you Continous Integration CI process is build successfully 
   -  Sonarqube Code Vulnerability Check - Passed :tada:
   -  DockerHub Image Push - Passed :tada:
   -  Github commit for updated image in Deployment.yaml manifest - Passed :tada:

  ![image](https://github.com/praveensivakumar1998/springboot-app-cicd/assets/108512714/1fd89ed7-0377-4d61-a42b-41fd393c81d4)

## Continous Delivery steps:
* Create a EC2 machine for Kubernetes cluster as kops server.
* Create Kubernetes cluster using Kops - refer this document [ https://github.com/praveensivakumar1998/Creating-Kubernetes-cluster-using-kops.git ]
* Install ArgoCD operator in Kubernetes cluster [ https://operatorhub.io/operator/argocd-operator ]
```
curl -sL https://github.com/operator-framework/operator-lifecycle-manager/releases/download/v0.27.0/install.sh | bash -s v0.27.0
kubectl create -f https://operatorhub.io/install/argocd-operator.yaml
kubectl get pods -n operators
```
![image](https://github.com/praveensivakumar1998/springboot-app-cicd/assets/108512714/9cfa6f3a-6063-4b46-ae75-ccb35f19133b)

* Install ArgoCD Server in a kubernetes cluster [ https://argocd-operator.readthedocs.io/en/latest/usage/basics/ ]
* 

