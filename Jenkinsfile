
pipeline {
    agent any
    /*environment {
        MAVEN_HOME = tool('maven-3.8')
    }*/
    stages {
        stage('Check-Git-Secrets') {
            steps {
               sh 'whoami'
               /*sh 'docker ps -aq'
               sh 'docker rmi -f $(docker images -aq)'*/
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
                sh 'mvn -B sonar:sonar -Dsonar.projectKey=testing -Dsonar.host.url=http://20.198.108.138:9000 -Dsonar.login=ac9b4b0648bf88b682a3d487aa8a2227bc46575c'
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
                   sh 'sl auth --org "20b42bc3-da37-4a84-ace8-7bc23abd58df" --token "eyJhbGciOiJSUzUxMiIsInR5cCI6IkpXVCJ9.eyJpYXQiOjE2NjQ5NDcyOTcsImlzcyI6IlNoaWZ0TGVmdCIsIm9yZ0lEIjoiMjBiNDJiYzMtZGEzNy00YTg0LWFjZTgtN2JjMjNhYmQ1OGRmIiwidXNlcklEIjoiMTA2Y2ZlNWUtNWI3NS00NTRhLTgzNWUtMWNiZWYxMmI3MzFkIiwic2NvcGVzIjpbInNlYXRzOndyaXRlIiwiZXh0ZW5kZWQiLCJhcGk6djIiLCJ1cGxvYWRzOndyaXRlIiwibG9nOndyaXRlIiwicGlwZWxpbmVzdGF0dXM6cmVhZCIsIm1ldHJpY3M6d3JpdGUiLCJwb2xpY2llczpjdXN0b21lciJdfQ.dy89yqPy2fJFKGlh7pLXEi781hET2G2rFtKkT8Gz2Rjcn3MRIFTgZ5U8trFpw2gnK4t5dMwwPhG6jJJ7r8esjye1sZwz4h9K0hUMov1wGULLROcDKEDQycGy_2o9q1kucBZ6ZeQ_sDS5aQMG7OHIQLIi6agrTFU1BdhyLuyINiHmkdoAL-JkyeP2MFLYKshHlyxRn6DLTQWuYoq7i35kNYAMe63oiL7mrWm7ZJhpCFqrYGA95GQO2rpv4W3DeXFrHN8yTpJrfqFb4PP-p7Eglbps2OVfyypIF4nYISQTirlJA43RSOBEG2vJzJ6PlxWfsMhMuwziDETyx1s34X14hQ"'  
               }
            }
        }
         
      
       /* stage('mvn-Deploy-nexus-backup') {
            steps {
               sh 'mvn deploy'
            }
        }*/
    }
}
