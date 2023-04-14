pipeline {
    environment {
        registry = "ashritmr/studentsurvey645"
        registryCredential = 'Docker-Hub'
        DOCKERHUB_PASS = credentials('Docker-Hub')
        TIMESTAMP = new Date().format("yyyyMMdd_HHmmss")
        //PASS=credentials('password')
    }
    agent any

    stages{

            stage('Build survey page') {
                steps {
                    script {
                        sh 'rm -rf *.war'
                        sh 'jar -cvf swe645-assignment1.war -C src/main/webapp/ .'
                        sh 'echo ${BUILD_TIMESTAMP}'
                           
                        
                        sh 'docker login -u ashritmr -p "Ashu&1234"'
                        sh 'docker push ashritmr/studentsurvey645:${BUILD_TIMESTAMP}'
                        //docker.withRegistry('',registryCredential){
                            //def customImage = docker.build("ashritmr/studentsurvey645:${env.TIMESTAMP}")
                        //}

                   }
                }
            }

            stage('Push to Docker Hub') {
                steps {
                    script {
                        docker.withRegistry('',registryCredential){
                          sh "docker push ashritmr/studentsurvey645:${BUILD_TIMESTAMP}"
                       }

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
Footer
