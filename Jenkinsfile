pipeline {
    agent any





stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
        }

        stage ("Deploy to docker tomcat"){
            steps{
                sh "docker build . -t tomcatwebapp:${env.BUILD_ID}"
            }
        }


    }
}