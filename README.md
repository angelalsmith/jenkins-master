# Dockerized Jenkins master

### How to run
```
docker run -p 8083:8083 -p 50000:50000 \
          -v ~/jenkins_home:/var/jenkins_home -v ~/.m2:/root/.m2 -v ~/.ssh:/var/jenkins_home/.ssh 
          -e JENKINS_OPTS="--httpPort=-1 --httpsPort=8083 --httpsCertificate=/var/jenkins_home/ssl.crt --httpsPrivateKey=/var/jenkins_home/ssl.key" \
          -d --rm --name jenkins qaprosoft/jenkins-master:latest
```

### Read plugins from remote Jenkins
```
#!/bin/bash

JENKINS_HOST=<username>:<password>@localhost:8080
curl -sSL "http://$JENKINS_HOST/pluginManager/api/xml?depth=1&xpath=/*/*/shortName|/*/*/version&wrapper=plugins" | perl -pe 's/.*?<shortName>([\w-]+).*?<version>([^<]+)()(<\/\w+>)+/\1 \2\n/g'|sed 's/ /:/'
```

### Manual deployment steps for 3rd party Jenkins Setup
Follow detailed [guide](https://github.com/qaprosoft/jenkins-master/blob/master/manual_deployment/README.md)

### F.A.Q
Q: Unable to start any job due to the:
```
General error during conversion: Error grabbing Grapes -- [download failed: org.beanshell#bsh;2.0b4!bsh.jar]
```
A: remove completely $HOME/.m2/repository and QPS_HONE/jenkins/.groovy/grapes content to allow jenkins to redownload everything from scratch
```
rm -rf ~/.m2/repository
cd ~/tools/qps-infra
rm -rf ./jenkins/.groovy/grapes
```

