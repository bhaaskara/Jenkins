# Jenkins "unable to find valid certification path to requested target" error while installing plugins

https://stackoverflow.com/questions/24563694/jenkins-unable-to-find-valid-certification-path-to-requested-target-error-whil/55758378

However, you don't want to add it to the JRE cacert keystore because it will be overwritten/rewritten by the JRE, so it's best to duplicate this file for Jenkins.

#Import certificate
openssl s_client -showcerts -verify 5 -connect updates.jenkins.io:443

openssl s_client -showcerts -connect updates.jenkins.io:443 < /dev/null 2> /dev/null | openssl x509 -outform PEM > ~/root_ca.pem

###
I had to replace the https://your-target-server with updates.jenkins.io:443 and jenkins.io:443 though (without https://). Don't use the second part of the command after the pipe | because one url has two certificates (edit manually instead). Repeat the keytool part for each.
###
#openssl x509 -in full_chain.pem -out full_chain_sanitized.pem

#The default password of cacert is "changeit"

#Duplicate Java Keystore file and move into Jenkins...
mkdir $JENKINS_HOME/keystore/
cp $JAVA_HOME/jre/lib/security/cacerts $JENKINS_HOME/keystore/

#Add Certificate to Keystore
keytool -import -alias jenins-ca -keystore /var/lib/jenkins/keystore/cacerts -file ~/root_ca.pem

#Add -Djavax.net.ssl.trustStore=$JENKINS_HOME/keystore/cacerts to the
#Jenkins startup parameters. For Debian/Ubuntu, this is /etc/default/jenkins
echo 'JAVA_ARGS="$JAVA_ARGS -Djavax.net.ssl.trustStore=$JENKINS_HOME/keystore/cacerts"'\
>> /etc/default/jenkins

sudo service jenkins restart

