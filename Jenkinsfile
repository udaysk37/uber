pipeline{
    
    agent any 
        environment {
           PATH="/opt/maven/bin/:$PATH"
   }    
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
                script{
                    
                    git branch: 'main', url: 'https://github.com/udaysk37/uber.git'
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
                    
                    withSonarQubeEnv(credentialsId: 'sonar-api') {
                        
                        sh 'mvn clean package sonar:sonar'
                    }
                   }
                }
            }
            stage('store artifacts'){
                
                steps{
                    
                    script{
                        nexusArtifactUploader artifacts: 
                            [
                                [
                                    artifactId: 'springboot', 
                                    classifier: '', 
                                    file: 'target/Uber.jar', 
                                    type: 'jar'
                                ]
                            ], 
                            credentialsId: 'nexus', 
                            groupId: 'com.example', 
                            nexusUrl: '13.53.168.120:8081', 
                            nexusVersion: 'nexus3', 
                            protocol: 'http', 
                            repository: 'java-demoapp', 
                            version: '1.0.0'

                    }
                }
            }
        }
    }
