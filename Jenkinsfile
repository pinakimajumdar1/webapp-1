/*Install Jenkins plugin sshagent before running the pipeline*/
pipeline {
    agent any
    tools {
        maven 'maven'
        jdk 'java'
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }

        stage ('Build') {
            steps {
                sh 'mvn clean -Dmaven.test.failure.ignore=true install' 
            }
            post {
                success {
                    junit 'target/surefire-reports/**/*.xml' 
                }
            }
        }
          stage ('Deploy To Tomcat') {
                    steps {
                            sshagent(['tomcat-dev']){
                                sh '''
                                    scp -o StrictHostKeyChecking=no target/*.war ec2-user@18.224.136.77:/opt/apache-tomcat-7.0.94/webapps
                                   '''
                            }                
                    }
                }

       
        }
    
}
