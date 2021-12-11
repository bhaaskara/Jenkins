# Install Jenkins
## Install Jenkins on Ubuntu 20.04
### Prereqs
**Disable firewall**  
**Install Java**

1. **Disable firewall**  
   Check the firewall status
   ```sh
   sudo ufw status
   ```
   Disable firewall
   ```sh
   sudo ufw disable
   ```
3. **Install Java**
   ```sh
   sudo apt-get install -y java-11-openjdk-devel
   ```
### Install Jenkins
```sh
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```
**Check the Jenkins service status**  
`system status jenkins`

## Install Jenkins on Centos 7.7
### Prereqs
**Disable SELINUX**  
**Disable firewall**  
**Install Java**
1. **Disable SELINUX**    
   Check the current SELinux status
   ```sh
   sudo sestatus
   ```
   Disable SELinux temporarily
   ```sh
   sudo setenforce 0
   ```
   Check it with `getenforce`  
   
   Disable SELINUX permanently
   ```sh
   vi /etc/selinux/config
   #set the
   SELINUX=disabled
   ```
   Reboot the server `init 6`  
2. **Disable firewall**  
   Check the firewall status
   ```sh
   sudo firewall-cmd --state
   ```
   Stop the firewall
   ```sh
   sudo systemctl stop firewalld
   ```
   Disable it permanantly
   ```sh
   sudo systemctl disable firewalld
   ```
   Mask the FirewallD service from being started by other services
   ```sh
   sudo systemctl mask --now firewalld
   ```
3. **Install Java**
   ```sh
   yum install java-11-openjdk-devel
   ```
### Install Jenkins
```sh
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
yum install epel-release # repository that provides 'daemonize'
sudo yum upgrade
sudo yum install jenkins -y
sudo systemctl daemon-reload
```
Start jenkins
```sh
sudo systemctl start jenkins
```
check status
```sh
sudo systemctl status jenkins
```

## Installation Issues
### Daemonize not found error
```sh
sudo vi /etc/yum.repos.d/epelfordaemonize.repo 
#Add the following:
[daemonize]
baseurl=https://download-ib01.fedoraproject.org/pub/epel/7/x86_64/
gpgcheck=no
enabled=yes
```
```sh
sudo yum install -y daemonize
```
> If daemonize installation fails with above method,  
> download the latest release (https://download-ib01.fedoraproject.org/pub/epel/7/x86_64/Packages/d/daemonize-1.7.7-1.el7.x86_64.rpm)  
> Install with `yum localinstall <rpm file>`  


### Jenkins.*.noarch.rpm file not found
Download the latest release from https://jenkins.io/redhat-stable/  
Install with `yum localinstall <rpm file>`  

## Install Jenkins using war file
1. Download the war file  
   https://get.jenkins.io/war-stable/2.303.3/jenkins.war
2. Run the Jenkins war file  
   `java -jar jenkins.war`
   > Note the unlock password and unlock the Jenkins using https://localhost:8080  
   > Initial unlock password may also be found at: /home/master/.jenkins/secrets/initialAdminPassword  
3. Add the jenkins to system startup
   ```sh
   vi ~/.bashrc
   #add the below line at the end
   nohup java -jar /home/master/jenkins/jenkins.war > /dev/null 2>&1 &
   ```
