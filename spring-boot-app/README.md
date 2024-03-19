# Spring Boot based Java web application
 
This is a simple Sprint Boot based Java application that can be built using Maven. Sprint Boot dependencies are handled using the pom.xml 
at the root directory of the repository.

This is a MVC architecture based application where controller returns a page with title and message attributes to the view.

## Execute the application locally and access it using your browser

Checkout the repo and move to the directory

```
git clone https://github.com/Sajiyah-Salat/Jenkins-e2e-k8s.git
cd Jenkins-e2e-k8s/spring-boot-app
```
[Install maven](https://www.digitalocean.com/community/tutorials/install-maven-linux-ubuntu) 


Execute the Maven targets to generate the artifacts

```
mvn clean package
```

The above maven target stroes the artifacts to the `target` directory. You can either execute the artifact on your local machine
(or) run it as a Docker container.

** Note: To avoid issues with local setup, Java versions and other dependencies, I would recommend the docker way. **



### The Docker way

Build the Docker Image

```
docker build -t ultimate-cicd-pipeline:v1 .
```

```
docker run -d -p 8010:8080 -t ultimate-cicd-pipeline:v1
```


Hurray !! Access the application on `http://<ip-address>:8010`
lauch instance with t2.large
Install Java

```
sudo apt update
sudo apt install openjdk-11-jre
```

Verify Java is Installed

```
java -version
```

Now, you can proceed with installing Jenkins

```
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

**Note: ** By default, Jenkins will not be accessible to the external world due to the inbound traffic restriction by AWS. Open port 8080 in the inbound traffic rules as show below.

- EC2 > Instances > Click on <Instance-ID>
- In the bottom tabs -> Click on Security
- Security groups
- Add inbound traffic rules as shown in the image (you can just allow TCP 8080 as well, in my case, I allowed `All traffic`).

<img width="1187" alt="Screenshot 2023-02-01 at 12 42 01 PM" src="https://user-images.githubusercontent.com/43399466/215975712-2fc569cb-9d76-49b4-9345-d8b62187aa22.png">

Create a jenkins pipeline and use git as a source of truth. 
apply changes and save file. 

install sonarqube scanner and docker pipeline in jenkins

## Next Steps

### Configure a Sonar Server locally

```
sudo su -
apt install unzip
adduser sonarqube
exit
sudo su - sonarqube
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.4.0.54424.zip
unzip *
chmod -R 755 /home/sonarqube/sonarqube-9.4.0.54424
chown -R sonarqube:sonarqube /home/sonarqube/sonarqube-9.4.0.54424
cd sonarqube-9.4.0.54424/bin/linux-x86-64/
./sonar.sh start
```

Hurray !! Now you can access the `SonarQube Server` on `http://<ip-address>:9000` 


generate a sonarqube token and add it to jenkins credentials with id 'sonarqube' ans it will be secret text

Run the below command to Install Docker

```
sudo su -
apt update
apt install docker.io
```
 
### Grant Jenkins user and Ubuntu user permission to docker deamon.

```
 
usermod -aG docker jenkins
usermod -aG docker ubuntu
systemctl restart docker
```

edit jenkins file, add github and docker creds in jenkins credentials. 
Run the pipeline. 


Congrats for running your pipeline succesfully. 


Now install argocd [operator](https://operatorhub.io/operator/argocd-operator)https://operatorhub.io/operator/argocd-operator) on killercoda

create argocd_basic.yaml
```
apiVersion: argoproj.io/v1alpha1
kind: ArgoCD
metadata:
  name: example-argocd
  labels:
    example: basic
spec: {}
```
kubectl apply -f argocd_basic.yaml

now run
kubectl get pods
kubectl get svc

edit, 
kubectl edit svc example-argocd-server

change type clusterip to nodeport





