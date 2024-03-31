pipeline{
    
    agent any 
    
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
                script{
                    
                   git branch: 'main', url: 'https://github.com/maxy1998/demo-counter-app.git'
            }
        }

            }
             stage('UNIT testing'){

            steps{

                script{

                    sh 'mvn test'
                }
            }
        }
        stage('Integration testing'){

            steps{

                script{

                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }
        stage('Maven build'){

            steps{

                script{

                    sh 'mvn clean install'
                }
            }
        }
        stage('Static code analysis'){

            steps{

                script{

                    withSonarQubeEnv(credentialsId: 'SonarTokenDemos') {

                        sh 'mvn clean package sonar:sonar'
                    }
                   }

                }
            }
         stage('Quality Gate Status'){

                steps{

                    script{

                        waitForQualityGate abortPipeline: false, credentialsId: 'SonarTokenDemos'
                    }
                }
            }

            stage('Nexus Repository'){

                steps{

                    script{

                        nexusArtifactUploader artifacts:
                        [[artifactId: 'springboot', classifier: '', file: 'target/Uber.jar', type: 'jar']],
                        credentialsId: 'dca7d6b2-e7e9-4b59-ae69-42f847905f91', groupId: 'com.example',
                        nexusUrl: 'localhost:8081', nexusVersion: 'nexus3', protocol: 'http',
                        repository: 'demo-app-release', version: '1.0.0'
                    }
                }
            }
    }
}
