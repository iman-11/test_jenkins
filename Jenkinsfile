pipeline {
    agent any
    tools {
        maven 'maven_3_9_7'
    }
    environment {
        JAVA_HOME = 'C:\\Program Files\\Java\\jdk-17'
        PATH = "${env.JAVA_HOME}\\bin;${env.PATH}"
    }
    stages {
        stage('Checkout') {
            steps {
                script {
                    retry(3) {
                        checkout([
                            $class: 'GitSCM',
                            branches: [[name: '*/main']],
                            extensions: [],
                            userRemoteConfigs: [[url: 'https://github.com/iman-11/test_jenkins']]
                        ])
                    }
                }
            }
        }
        stage('Build Maven') {
            steps {
                bat 'mvn clean install'
            }
        }
         stage('Build docker image'){
            steps{
                script{
                    bat 'docker build -t javatechie/devops-integration .'
                }


            }
        }
               stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'dockerr', variable: 'docker')]) {
                       bat 'echo %docker% |docker login -u imanhrt -p ${docker}'
                         }



                      bat 'docker push javatechie/devops-integration'
                }
            }
        }

    }
}
