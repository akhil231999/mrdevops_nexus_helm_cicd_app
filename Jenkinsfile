pipeline{

    agent any

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


    }










}
