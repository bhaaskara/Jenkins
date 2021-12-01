# Install Jenkins
## Install Jenkins using war file
1. Download the war file  
   https://get.jenkins.io/war-stable/2.303.3/jenkins.war
2. Run the Jenkins war file  
   `java -jar jenkins.war`
   > Note the unlock password and unlock the Jenkins using https://localhost:8080
3. Add the jenkins to system startup
   ```sh
   vi ~/.bashrc
   #add the below line at the end
   nohup java -jar /home/master/jenkins/jenkins.war > /dev/null 2>&1 &
   ```
