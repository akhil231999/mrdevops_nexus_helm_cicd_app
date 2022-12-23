pipeline{

    agent any

    environment {

        VERSION = "${env.BUILD_ID}"
    }

    stages{
         
        stage('sonar quality check'){
            
            agent{

                docker {
                    image 'maven'
                }
            }
            steps{

                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                    
                    sh 'mvn clean package sonar:sonar'
                 }
                }
            }
        }

        stage('Quality Gate Status'){

            steps{

                sript{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                }
            }
        }

        stage('docker build and docker push to nexus repo'){

            steps{
                script{
                    withCredentials([string(credentialsId: 'nexus-passwd', variable: 'nexus_cred')]) {
                    sh '''
                     docker build -t 34.125.97.46:8083/springapp:${VERSION} .

                     docker login -u admin -p $nexus_cred 34.125.97.46:8083

                     docker push 34.125.97.46:8083/springapp:${VERSION}

                     docker rmi 34.125.97.46:8083/springapp:${VERSION}
                    '''
    
                     }
                }
            }
        }
    }
}
