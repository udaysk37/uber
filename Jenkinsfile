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
                        def readPomVersion = readMavenPom file: 'pom.xml'
                        def nexusRepo = readMavenPom.version.endsWith("SNAPSHOT") ? "maven-snapshots" : "java-demoapp"
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
                            repository: nexusRepo, 
                            version: "${readPomVersion.version}"

                    }
                }
            }
        }
    }
