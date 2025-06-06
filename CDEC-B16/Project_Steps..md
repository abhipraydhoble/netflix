Project: Netflix App using CICD pipeline

Step1: Launch EC2 instance
       - AMI: Ubuntu
       - Instance_Type: t2.medium
       - Volume: 30 GB

Step2: Connect To Instance
      - Install Jenkins
      - Install Docker
      - Install SonarQube    # used for code quality testing
      - Install Trivy        # used to scan docker images
      
**Jenkins**
````
sudo apt update
sudo apt install fontconfig openjdk-21-jre  -y
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins -y
````
**Docker**
````

sudo apt-get update
sudo apt-get install docker.io -y
sudo systemctl start docker
sudo usermod -aG docker ubuntu
newgrp docker
sudo chmod 777 /var/run/docker.sock
````
**SonarQube**
````
docker run -d --name sonar -p 9000:9000 sonarqube:lts-community
````

**Trivy**
````
sudo apt-get install wget apt-transport-https gnupg lsb-release
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy -y
````

Step3: Connect to Jenkins 

Step4: Connect to SonarQube
      - Admin->my account->security->generate token

Step5: In Jenkins
     - Manage Jenkins: Credentials
       - Sonar-Token
       - Git-Cred
       - Docker-Cred

Step6: Install Required Plugins:
       - Eclipse Temurin Installer 
       - SonarQube Scanner
       - NodeJs Plugin
       - docker
       - stage view

Step7: Install  Tools: Manage Jenkins->Tools
       - add jdk: "jdk17" ->install from adoptium.net->version- 17
       - add SonarQube Scanner: "sonar-scanner"
       - add NodeJs: "node16" -> version 16
       - docker: "docker"

Step8: Configure Sonar Server: Manage Jenkins->System
      - name: "sonar-server"
      - url:
      - token:

Step9: Create Pipeline

Note: 
- ensure jenkins user has permission to create container
   # sudo usermod -aG docker jenkins
   # newgrp docker
   # sudo chmod 777 /var/run/docker.sock
   
- Verify all the names before running pipeline
