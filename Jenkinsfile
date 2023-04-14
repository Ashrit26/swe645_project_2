pipeline {
    environment {
        DOCKERHUB_PASS = credentials('Docker-Hub')
    }
    agent any

    stages{

            stage('Build survey page') {
                steps {
                    script {
                        sh 'rm -rf *.war'
                        sh 'jar -cvf swe645-assignment1.war -C src/main/webapp/ .'
                        sh 'echo ${BUILD_TIMESTAMP}'
                        
                        sh "docker login -u ashritmr -p ${DOCKERHUB_PASS}"
                        sh 'docker build -t ashritmr/studentsurvey645:${BUILD_TIMESTAMP} .'

                   }
                }
            }

            stage('Push to Docker Hub') {
                steps {
                    script {
                        sh 'docker push ashritmr/studentsurvey645:${BUILD_TIMESTAMP}'

                    }
                }
            }

          stage('Deploying Rancher to single node') {
             steps {
                script{
                    sh "kubectl set image deployment/project2nodeport container-0=ashritmr/studentsurvey645:${BUILD_TIMESTAMP}"
                }
             }
          }

        stage('Deploying Rancher to Load Balancer') {
           steps {
              script{
                  sh "kubectl set image deployment/project2loadbalancer container-0=ashritmr/studentsurvey645:${BUILD_TIMESTAMP}"
              }
           }
        }
    }
}
