# Install Jenkins
## Install Jenkins on Ubuntu 20.04
### Prereqs
1.  **Disable firewall**  
2. **Install Java**

1. **Disable firewall**  
   Check the firewall status
   ```sh
   sudo ufw status
   ```
   Disable firewall
   ```sh
   sudo ufw disable
   ```
2. **Install Java**
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
1. **Disable SELINUX**  
2. **Disable firewall**  
3. **Install Java**

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

### Uninstall Jenkins
`yum remove jenkins`
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
# Master node isolation / Disable jobs on master node
Out of the box, Jenkins is set up to run builds on the built-in node. This is to make it easier to get started with Jenkins, but is inadvisable longer term: Any builds running on the built-in node have the same level of access to the controller file system as the Jenkins process.

It is therefore highly advisable to not run any builds on the built-in node, instead using agents (statically configured or provided by clouds) to run builds.

To prevent builds from running on the built-in node directly, 
```navigate to 
Manage Jenkins » Manage Nodes and Clouds
Select master in the list
select Configure in the menu 
Set the number of executors to 0 and save
```
> Make sure to also set up clouds or build agents to run builds on, otherwise builds won’t be able to start.  

# Install Jenkins plugins manually
**1:** Download plugin from Jenkins plugin directory.  
`https://updates.jenkins-ci.org/download/plugins/`

**2:** Here you find your desired plugin and click on plugin name, now .hpi file will be downloaded.

**3:** Now open Jenkins and go to 
     `Manage Jenkins > Manage Plugins > Advance configuration`(tab)

**4:** Upload `your-plugin.hpi` file and save.

**5:** Restart Jenkins.

# view all jenkins labels
https://stackoverflow.com/questions/27384481/view-all-labels-in-jenkins

# Helloworld job
1.  Login to Jenkins server: `<serverip>:8080`
2. NewItem -> `<enter name>` -> Freestyle job -> ok
3. Build environment -> Execute shell (for linux server) -> enter Linux commands -> save
    ![](Pasted%20image%2020220403220931.png)
4. To run the Job: click on Build now
    ![](Pasted%20image%2020220403221308.png)
    Job status can be seen in Build history.
5. For the script/commands o/p check the console o/p in build history.

# Integrate Git with Jenkins
1. Install Git on Jenkins instance
2. Install Github plugin on Jenkins
3. Configure Git on Jenkins 

## Install Git on Jenkins instance
**Centos 7**
`sudo yum install git -y`
`git --version`

## Install github plugin on jenkins
Jenkins -> Manage Jenkins -> Manage Plugins -> Available -> `search for github`

## Configure Git on Jenkins
Jenkins -> manage jenkins -> Global tool configuration 
![](Pasted%20image%2020220405231413.png)
Note: path can be found by `whereis git` command.

# Integrate maven with Jenkins
1. Install maven on jenkins server
2. setup environment variables
    JAVA_HOME, M2, M2_HOME
3. Install maven plugin on jenkins
4. configure maven and java on jenkins

## 1. Install maven on jenkins server
`sudo yum install maven -y` # centos 7
`mvn -version`

## 2. Setup environment variables
```/root/.bash_profile
# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# User specific environment and startup programs
JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.322.b06-1.el7_9.x86_64
M2=/usr/bin/mvn #which maven
M2_HOME=/usr/share/maven #mvn -version

PATH=$PATH:$HOME/bin:$JAVA_HOME:$M2:M2_HOME
```
`sudo su -`
`source .bash_profile`

## 3. Install maven plugin on Jenkins
Jenkins -> manage jenkins -> manage plgins -> available -> maven integration

## 4. configure maven and java on jenkins
Jenkins -> manage jenkins -> global tool configuration -> maven
![](Pasted%20image%2020220406172138.png)
Jenkins -> manage jenkins ->  global tool configuration -> JDK
![](Pasted%20image%2020220406172009.png)
Note: wait for a while for the error to go off

# Build a Java project using jenkins
>**Pre reqs**: mvn and git on jenkins server

**Create Jenkins job**
Jenkins -> new item -> maven project

![](Pasted%20image%2020220406191336.png)
add Git repo and other options are default

# Integrating tomcat server in CI/CD pipeline
**_Deploy artifacts onto a tomcat server_**
![](Pasted%20image%2020220409190021.png)
### Setup a tomcat server
```
1. Install Java
2. Configure tomcat
3. start tomcat server
4. access webUI on port 8080
```
### Integrate Tomcat server with Jenkins
```
1. Install "deploy to container" jenkins plugin
2. configure tomcat server with credentials
```

**1. Install "deploy to container" jenkins plugin**
Jenkins -> manage jenkins -> manage plugins -> available -> search for "deploy to container"

**2. configure tomcat server with credentials**
**Add credential in Jenkins**
Jenkins -> manage jenkins -> manage credentials -> Jenkins -> Global credentials -> add credentials
![](Pasted%20image%2020220410220341.png)

## create a Job to deploy on to tomcat server
> Create a job with Git source code and maven as build trigger then add post build step to deply onto tomcat server.

![](Pasted%20image%2020220410220659.png)
![](Pasted%20image%2020220410221214.png)

# Integrating Kubernetes in CI/CD pipeline
![](Pasted%20image%2020220413200315.png)

## 1. Configure bootstrap server (EC2) to access AWS EKS.
1. Install AWS CLI
2. Install EKSCTL
3. Install kubectl
4. Assign AWS Role to the server (EC2 instance)

## 2. Integrate Kubernetes with Ansible
![](Pasted%20image%2020220413173141.png)

**On Bootstrap server**
1. Create ansadmin user
    ```sh
    useradd ansadmin
    passwd ansadmin # Set password
    ```

2. Add ansadmin to sudoers file
    ```sh
    visudo
    ...
    #add below line next to root
    ansadmin ALL=(ALL) NOPASSWD: ALL
```    

3. Enable password based login
    ```sh
    vi /etc/ssh/sshd_config
    ...
    # enable PasswordAuthentication yes and comment out PasswordAuthrntication no

    service sshd reload # Reload teh service
```

**On Ansible node**
1. Add bootstrap server to hosts file
    ```sh
    vi hosts
    localhost
    
    [kubernetes]
    172.31.28.76 #bootstarp server ip
    
    [ansible]
    172.31.30.109
```

2. Copy ssh keys on to bootstrap server
    `ssh-copy-id _bootstarp serveip_ `
    `ssh-copy-id root@_bootstapServerIP_  #copy ssh keys onto root user`

3. Test the connection
    `ansible -i hosts all -a uptimne`

## 3. Create ansible play book for deploy and service files
```yml
- hosts: kubernetes
  #become: true  #run as root user
  user: root
  
  tasks:
  - name: Deply regapp onto k8s
    command: kubectl create -f /root/deploy.yml
```

service.yml
```yml
- hosts: kubernetes
  #become: true  #run as root user
  user: root
  
  tasks:
  - name: Deply service onto k8s
    command: kubectl create -f /root/service.yml
```

Update deployment.yml
```yml
- hosts: kubernetes
  #become: true  #run as root user
  user: root
  
  tasks:
  - name: Deply regapp onto k8s
    command: kubectl create -f /root/deploy.yml

  - name: update deployment with new pods if image updated in the docker hub
    command: kubectl rollout restart deployment.apps/valaxy-app
```
## 4. Create Jenkins deployment job for Kubernetes
Jenkins -> new job -> free style job -> post build actions -> send build artifacts over ssh -> select ansible server


![](Pasted%20image%2020220413200744.png)

> Note: in this job no need to configure Git and others

## 5. CI job to create image for kubernetes
Create_docker_image.yml
```yml
- hosts: ansible

  tasks:
  - name: Create docker image
    command: docker build -t regapp:latest .
    args:
      chdir: /opt/docker
  
  - name: create tag to push to docker hub
    command: docker tag regapp:latest valaxy/regapp:latest

  - name: push docker image
    command: docker push valaxy/regapp:latest
```

Jenkins -> new job -> free style job -> post build actions -> send build artifacts over ssh -> select ansible server

![](Pasted%20image%2020220413211724.png)

