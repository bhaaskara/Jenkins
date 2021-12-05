# Install Jenkins
## Install Jenkins on Centos 7.7
### Prereqs
**Disable SELINUX**  
**Disable firewall**  
1. **Disable SELINUX**    
   Check the current SELinux status
   ```
   sudo sestatus
   ```
   Disable SELinux temporarily
   ```
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
   ```
   sudo firewall-cmd --state
   ```
   Stop the firewall
   ```
   sudo systemctl stop firewalld
   ```
   Disable it permanantly
   ```
   sudo systemctl disable firewalld
   ```
   Mask the FirewallD service which will prevent the firewall being started by other services
   ```
   sudo systemctl mask --now firewalld
   ```
### Install Jenkins
```
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
sudo yum upgrade
```
`IMPORTANT`
```sh
sudo vi /etc/yum.repos.d/epelfordaemonize.repo 
#Add the following:
[daemonize]
baseurl=https://download-ib01.fedoraproject.org/pub/epel/7/x86_64/
gpgcheck=no
enabled=yes
```
```
sudo yum install -y daemonize
```
> if this succeed, proceed and install Jenkins
```
sudo yum install jenkins -y
sudo systemctl daemon-reload
```
Start jenkins
```
sudo systemctl start jenkins
```
check status
```
sudo systemctl status jenkins
```
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
