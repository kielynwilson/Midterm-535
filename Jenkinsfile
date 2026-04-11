pipeline {
    agent any

    tools {
        jdk 'JDK17'
        maven 'Maven3'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/kielynwilson/Midterm-535.git'
            }
        }

        stage('Build - Java 17') {
            tools {
                jdk 'JDK17'
                maven 'Maven3'
            }
            steps {
                bat 'echo JAVA_HOME=%JAVA_HOME%'
                bat 'java -version'
                bat 'mvn -version'
                bat 'mvn clean compile'
            }
        }

        stage('Test - Java 11') {
            tools {
                jdk 'JDK11'
                maven 'Maven3'
            }
            steps {
                bat 'echo JAVA_HOME=%JAVA_HOME%'
                bat 'java -version'
                bat 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            tools {
                jdk 'JDK17'
                maven 'Maven3'
            }
            steps {
                withSonarQubeEnv('SonarQube') {
                    withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                        bat '''
mvn clean verify sonar:sonar ^
-Dsonar.login=%SONAR_TOKEN%
'''
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t kielynwilson/java-app:latest .'
            }
        }

        stage('Push Docker Image') {
            steps {
                bat 'docker push kielynwilson/java-app:latest'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat 'kubectl apply -f deployment.yaml'
                bat 'kubectl apply -f service.yaml'
                bat 'kubectl get pods'
                bat 'kubectl get services'
            }
        }
    }
}
