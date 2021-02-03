pipeline {
  agent any 
  tools {
    maven 'Maven'
  }
  stages {
      
     stage ('Cloning Git') {
      steps {
        git 'https://github.com/abhi5658/AirQualityMonitoringSystem.git'
     }
    }
    
    stage ('Initialize') {
      steps {
        sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
            ''' 
      }
    }
    
   stage ('Check-Git-Secrets'){
        steps{
        sh 'rm trufflehog || true'
        sh 'docker run -t gesellix/trufflehog --json https://github.com/abhi5658/AirQualityMonitoringSystem.git > trufflehog'
        sh 'cat trufflehog'
       }
    }
    
/* stage ('Software Composition Analysis') {
      steps {
         sh 'rm -r dependency-check* || true' 
         sh 'wget https://github.com/jeremylong/DependencyCheck/releases/download/v6.0.3/dependency-check-6.0.3-release.zip'
         sh 'unzip dependency-check-6.0.3-release.zip'
         sh './dependency-check/bin/dependency-check.sh --scan ./* --enableRetired -f "ALL" '
      }
 }
       /*stage ('Software Composition Analysis') {
            steps {
                dependencyCheck additionalArguments: ''' 
                    -o "./" 
                    -s "./"
                    -f "ALL" 
                    --prettyPrint''', odcInstallation: 'dependencycheck'
                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
            }
        }*/
    
    
    
    
    stage ('Build') {
      steps {
      sh 'mvn clean package'
       }
    }
    
    /*stage ('SAST') {
      steps {
        withSonarQubeEnv('sonar') {
          sh 'mvn sonar:sonar'
          sh 'cat target/sonar/report-task.txt'
        }
      }
    }*/
   
    
   stage ('Deploy-To-Tomcat') {
            steps {
           sshagent(['tomcat']) {
//sh 'cp target/*.war /home/tas/prod/apache-tomcat-9.0.41/webapps/webapp.war'
         sh 'scp -o StrictHostKeyChecking=no target/*.war natasha_1998@130.211.221.9:/var/lib/tomcat8/webapps/air.war'
            // sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@34.122.205.85:webapps'
              }      
           }       
   }
    
    
 /*  stage ('DAST') {
      steps {
        echo 'DAST'
         // sh 'ssh -o  StrictHostKeyChecking=no ubuntu@35.193.155.239 "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://35.225.146.167:8080/shoppingcartapp-web-V2/" || true'
        }
      }
    */
  }
}
