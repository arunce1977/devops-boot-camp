pipeline {
  agent {label "agentfarm" }
  stages {
    stage('Installing Maven') {
      steps {
        sh 'sudo apt-get update -y && sudo apt-get upgrade -y'
        sh 'sudo apt-get install -y wget tree unzip maven'
      }
    }
    stage('Compiling and Running Test Cases') {
      steps {
        sh 'mvn clean'
        sh 'mvn compile'
        sh 'mvn test'
      }
    }
    stage('Creating Package') {
      steps {
        sh 'mvn package'
      }
    }
    stage ('Install sonarqube cli') {
      steps {
        sh 'wget -O sonar-scanner.zip  https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli4.6.2.2472-linux.zip'
        sh 'unzip -o -q sonar-scanner.zip'
        sh 'sudo rm -rf /opt/sonar-scanner'
        sh 'sudo mv --force sonar-scanner-4.6.2.2472-linux /opt/sonar-scanner'
        sh 'sudo sh -c \'echo "#/bin/bash \nexport PATH=\\\"$PATH:/opt/sonar-scanner/bin\\\"" > /etc/profile.d/sonar-scanner.sh\''
        sh 'chmod +x /opt/sonar-scanner/bin/sonar-scanner'
        sh '. /etc/profile.d/sonar-scanner.sh'
      }
    }

    stage('Deploying Application') {
      steps {
        script{
          withEnv(['JENKINS_NODE_COOKIE=dontkill']){
            sh 'nohup java -jar ./target/springboot-bootcamp-0.0.1-SNAPSHOT.jar &'
          }
        }
      }
    }
  }
}
