pipeline {
    agent {
        label 'win10 -> java && sonar'
    }
    tools {
      maven 'Maven 3.9.3'
    }
    
    stages {  
        stage('Clean') {
            steps {
                echo "Clean"
                
                bat "mvn clean"    
            }
        }
        
        stage('Build') {
            steps {
                echo "Building"
                
                bat "mvn compile" 
                
                echo "Building done"
            }
        }
        
        stage('Tests') {
            steps {
                bat "mvn test" 
            }
            
            post {
                success {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        
        stage('Package') {
            steps {
                bat "mvn package"
            }
            
            post {
                success {
                    archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
                }
            }
        }
        
        stage('QA') {
            environment {
                SCANNER_HOME = tool 'SonarQubeScanner 4.8'
                PROJECT_NAME = "hello_word"
                PROJECT_BINARIES = "target"
            }
            steps {
                withSonarQubeEnv(credentialsId: '22b19fb2-5a3a-484b-bbc6-abbc8fdf8ace', installationName: 'SonarQube 4.8') {
                    echo "$SCANNER_HOME"
                    bat """$SCANNER_HOME/bin/sonar-scanner \
                        -Dsonar.java.binaries=$PROJECT_BINARIES \
                        -Dsonar.projectKey=$PROJECT_NAME"""
                }
            }
        }

    }
}
