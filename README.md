# DevOps-Project-4
Deploy Website on Docker with code analysis tool SonarQube using  ec2 instance & Jenkins
![Docker Host (1)](https://github.com/rutikdevops/DevOps-Project-4/assets/109506158/d0b11f69-5f8c-4075-83a7-a7274a8a2fcb)
# Project Blog link :-
# Project Overview :-
# Project Steps :-
- Create 3 ec2 instance :
1. Jenkins-Server    : AWS Linux-2, t2 micro
2. Docker-Server     : AWS Linux-2, t2 micro
3. SonarQube-Server  : AWS Linux-2, t2 medium
<img width="960" alt="image" src="https://github.com/rutikdevops/DevOps-Project-4/assets/109506158/71f18501-3a09-45aa-a8a8-7e5c3d3bab2c">
# 1. Install and Configure the Jenkins :-
```bash
ec2-user
sudo su
yum update -y
hostnamectl set-hostname jenkins
bash
```
- Java installation on Jenkins server:-
```bash
yum install java* -y
java --version
alternatives --config java              ## Using this command you can choose any version of Java
                                        // select java 11 version
```

- Jenkins installation on Jenkins server:-
```bash
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
yum install jenkins -y
systemctl enable jenkins.service
systemctl start jenkins.service
systemctl status jenkins.service
```
- Copy public ip of jenkins server and paste it in new tab with port no.8080
<img width="576" alt="image" src="https://github.com/rutikdevops/DevOps-Project-2/assets/109506158/75d4392c-c663-441e-baa8-39c11af26ead">

- copy this path and paste in terminal with "cat" command
```bash
cat /var/lib/jenkins/secrets/initialAdminPassword
```
- Now copy & paste this passwd in jenkins. so, jenkins is ready.
<br></br>


# 2. Run the code in jenkins: 
- Jenkins-> New Item-> Freestyle Project-> Source Code Management-> Git
![image](https://github.com/rutikdevops/DevOps-Project-4/assets/109506158/02f20d88-f4b8-4a1c-9d29-7c1162a6b826)

- Connect github to jenkins through webhook
<img width="960" alt="image" src="https://github.com/rutikdevops/DevOps-Project-4/assets/109506158/2f366ed5-fae2-4056-8836-3c3e989f3008">

- Build Now in Jenkins
![image](https://github.com/rutikdevops/DevOps-Project-4/assets/109506158/9a4812b9-7f49-4503-b820-031ff998895c)



# 3. Run sonarqube
```bash
ec2-user
sudo su
yum update -y
hostnamectl set-hostname sonarqube
bash
```
- Java installation on Jenkins server:-
```bash
yum install java* -y
java --version
alternatives --config java              ## Using this command you can choose any version of Java
                                        // select java 11 version
```


```bash
cd /opt
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-8.9.10.61524.zip  #(chrome-downloadsonarqube-latestversion-rightclick-copypaste)
unzip sonarqube-8.9.10.61524.zip
cd sonarqube-8.9.10.61524
cd bin
cd linux-x86-64
./sonar.sh start
./sonar.sh status    (in that sonarqube is not running)
cd ../..
cd logs
useradd sonaradmin
chown -R sonaradmin:sonaradmin /opt/sonarqube-8.9.10.61524
pwd
cd ..
su - sonaradmin
cd /opt/sonarqube-8.9.10.61524/
cd bin
cd linux-x86-64
./sonar.sh start
./sonar.sh status
netstat -tulpn        #(command for check port)
                      #(paste public ip with :9000)
                      #(securitygroup-inboundrule-all traffic, anywhere)
                      #(username passwd bydefault is admin)
```



# 4. Connect to terminal and install docker :-
```bash
sudo su - 
yum install docker -y
systemctl start docker
systemctl enable docker
systemctl status docker
useradd dockeradmin
passwd dockeradmin
usermod -aG docker dockeradmin
vim /etc/ssh/sshd_config
 ( PasswordAuthentication yes )
systemctl restart sshd
```

- Lets Integrate Docker with Jenkins
- Jenkins -> Manage Jenkins -> Plugins -> Available -> Search for Publish Over SSH -> Install
- Lets configure Docker in Jenkins
- Jenkins -> Manage Jenkins -> System -> Publish Over SSH -> SSH Servers -> Add -> Give Name -> Hostname : <Docker host private ip> -> Username : dockeradmin -> advanced -> enable password based authentication -> password : <password of dockeradmin -> test configuration -> Apply -> Save

- Successfully instegrated docker with jenkins.
- Now goto docker server
```bash
cd /opt
mkdir docker
cd docker
chown -R dockeradmin:dockeradmin /opt/docker
vim Dockerfile
FROM tomcat:latest
RUN cp -R /usr/local/tomcat/webapps.dist/* /usr/local/tomcat/webapps
COPY ./*.war /usr/local/tomcat/webapps
 
chmod 777 /var/run/docker.sock
```


- Now Create a Jenkins job to pull the code from Github , build with help of Maven and copy the Artifacts to Dockerhost.
- Jenkins -> Dashboard ->New project-> DevOps-Project-4-> maven project-> Source Code Management->  Post-build Actions -> select Send build artifacts over SSH -> SSH Server -> Name : give name of Added SSH Server -> Source file: webapp/target/*.war -> Remove prefix : webapp/target -> Remote directory : //opt//docker -> Exec command : enter the below commands
```bash
cd /opt/docker;
docker build -t regapp:v1 .;
docker stop registerapp;
docker rm registerapp;
docker run -d --name registerapp -p 8081:8080 regapp:v1
```

![image](https://github.com/rutikdevops/DevOps-Project-4/assets/109506158/c8641958-0a3c-428b-8df1-67046160f153)
<img width="667" alt="image" src="https://github.com/rutikdevops/DevOps-Project-4/assets/109506158/5c8e3f4b-8120-402c-8b01-0b33ca5271fc">
<img width="680" alt="image" src="https://github.com/rutikdevops/DevOps-Project-4/assets/109506158/9f0b104a-5739-4179-8488-791791a65ad7">
![image](https://github.com/rutikdevops/DevOps-Project-4/assets/109506158/3c97a996-3b88-4bcb-a595-0c01dc464aef)
- Apply -> Save

# Output :-

<img width="946" alt="image" src="https://github.com/rutikdevops/DevOps-Project-4/assets/109506158/5b40ea11-bae4-49c7-8301-f79f953813f6">
<img width="949" alt="image" src="https://github.com/rutikdevops/DevOps-Project-4/assets/109506158/5abf45d1-3905-4ca9-a549-307dd96720ce">
<img width="719" alt="image" src="https://github.com/rutikdevops/DevOps-Project-4/assets/109506158/7ac056bf-3a91-4c38-b2db-51532e09ee50">



- In this section, we have successfully deployed the application to the Docker server and enabled CI/CD. If any changes occur in the source code, they will be automatically built and deployed to the server.











