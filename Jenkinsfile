pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
	git 'https://github.com/reeshu13489/devops_pipeline_demo.git'

        sh '''#!/bin/sh
echo "********** Starting CI/CD Pipeline *********"
pwd

#-Build
echo ""
echo "********** Starting Build Phase *********"
cd java_web_code
mvn install

#-Build (Test)
echo ""
echo "********** Test Phase started :: Testing via Automated scripts:: *********"
cd ../integration-testing
mvn clean verify -P integration-test'''
      }
    }

    stage('PROVISIONING DEPLOYMENT') {
      steps {
        sh '''#!/bin/sh
#-POSTBUILD(PROVISIONING DEPLOYMENT)
echo ""
echo "..... Integration Phase Started :: Copying Artifacts :: ....."
cd java_web_code
/bin/cp target/wildfly-spring-boot-sample.war ../docker
echo $?
pwd
echo ""
echo "..... Provisioning Phase started :: Building Docker container :: ......"
cd ../docker
docker build -t devops_pipeline_demo .'''
      }
    }

    stage('App Deployed') {
      steps {
        sh '''#!/bin/sh 

CONTAINER=$(docker ps) 
RUNNING=$(sudo docker inspect --format="{{ .State.Running }}" $CONTAINER 2> /dev/null)

if [ $? -eq 1 ];then 
	echo "\'$CONTAINER\' does not exist"
else
	sudo docker rm -f $CONTAINER
fi

#run your container
echo ""
echo "........ Deployment Phase started :: Building Docker Container :: ........"
sudo docker run -d -p 8180:8080 --name devops_pipeline_demo devops_pipeline_demo

#-Completion
echo "--------------------------------------------------------"
echo "View App Deployed here: http://server-ip:8180/sample.txt"
echo "--------------------------------------------------------"

'''
      }
    }

  }
}
