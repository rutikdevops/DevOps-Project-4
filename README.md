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
# Install and Configure the Jenkins :-
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
















