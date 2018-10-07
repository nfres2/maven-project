pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.191.97.37', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.223.1.57', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /home/nikos/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /home/nikos/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to docker tomcat"){
                    steps{
                        sh "sudo usermod -a -G docker $USER"
                        sh "docker build . -t tomcatwebapp:${env.BUILD_ID}"
                    }
                }
            }
        }
    }
}