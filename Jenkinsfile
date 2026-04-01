pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/kielynwilson/Pipeline.git'
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
                    bat 'mvn clean verify org.sonarsource.scanner.maven:sonar-maven-plugin:5.5.0.6356:sonar'
                }
            }
        }
    }
}
