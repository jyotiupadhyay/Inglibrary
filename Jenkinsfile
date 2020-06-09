pipeline {
   agent any
	stages {
      stage('Git Checkout SCM') {
         steps {            
            git 'https://github.com/jyotiupadhyay/Inglibrary.git'
		}
	}
	stage('Build Analysis') {
		steps {
			withSonarQubeEnv('sonar') {
				sh '/opt/maven/bin/mvn clean verify sonar:sonar -Dsonar.password=admin -Dsonar.login=admin  -Dmaven.test.skip=true'
			}
		}
	}
	stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
              }
            }
          }
	stage ('Install') {
		steps {
			sh '/opt/maven/bin/mvn clean install  -Dmaven.test.skip=true'
		}
	}
	stage ('Release') {
		steps {
		       sh 'export JENKINS_NODE_COOKIE=dontKillMe ;nohup java -Dspring.profiles.active=dev -jar $WORKSPACE/target/*.jar &'
		}
	}
}
}
