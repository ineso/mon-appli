pipeline {
    agent any
    

    stages {
        stage('SCM') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            parallel {
                stage('Compile') {
                    agent {
                        docker {
                            image 'maven:3.6.0-jdk-8-alpine'
                            reuseNode true
                        }
                    }
                    steps {
                        sh ' mvn -B -DskipTests clean package'
                    }
                }
            }
        }
        
        stage('Test') { 
            steps {
                sh 'mvn test' 
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml' 
                }
            }
        }
        
        stage('checkstyle') { 
            steps {
                sh 'mvn checkstyle:checkstyle' 
            }
            post {
                always {
                    recordIssues enabledForFailure: true, tool: checkStyle()
                }
            }
        }
        
        stage('SonarQube') {
            steps {
                sh "mvn clean verify sonar:sonar \
  -Dsonar.projectKey=mavensonar \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.login=sqp_75d14dde080f014d4ee9d6e9d4e5e090b8100750"
                    }
         }        
        
       

        
    }
}
