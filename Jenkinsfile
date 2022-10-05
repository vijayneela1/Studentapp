
pipeline {
    agent any
    environment {
        MAVEN_HOME = tool('maven-3.8')
    }
    stages {
        stage('Check-Git-Secrets') {
            steps {
               sh 'whoami'
               sh 'docker ps -aq'
               sh 'docker rmi -f $(docker images -aq)'
               sh 'echo "scanning github repository URL to detect secrets post-push"'
               sh 'docker run gesellix/trufflehog --json https://github.com/vijayneela1/Studentapp.git'
           }
    }
    /*stages {*/
        stage('mvn-clean') {
            steps {
               sh 'mvn clean'
            }
        }
        stage('mvn-compile') {
            steps {
               sh 'mvn compile'
            }
        }
    /*    stage('OWASP-Dependecy-check') {
            steps {
               sh 'sudo bash /usr/bin/dependency-check.sh --scan ./../Studentapp/'
               sh 'cat /var/lib/jenkins/workspace/DevSecOps/Studentapp/./dependency-check-report.html'
            }
        } */
        stage('Unit-Testing-sonar'){
            steps{
                sh '${MAVEN_HOME}/bin/mvn -B sonar:sonar -Dsonar.projectKey=testing -Dsonar.host.url=http://20.198.108.138:9000 -Dsonar.login=ac9b4b0648bf88b682a3d487aa8a2227bc46575c'
            }
        } 
        stage('mvn-Package') {
            steps {
               sh 'mvn package'
            }
        }
        stage('SAST-Shiftleft') {
            steps {
               dir("target/*.war") {
                   sh '/usr/local/bin/sl analyze --app Studentapp --java target/*.war'
            }
        }
      
       /* stage('mvn-Deploy-nexus-backup') {
            steps {
               sh 'mvn deploy'
            }
        }*/
    }
}
