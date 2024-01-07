pipeline {
    agent any

    tools {
        jdk "OracleJDK11"
        maven "MAVEN3"
    }

    environment {
        scannerHome = tool "sonar4"
    }
    
    stages {

        stage('performing unit test') {
            steps {
                sh 'mvn test'
                
            }
        }

        stage('checkstyle analysis') {
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }

        stage('sonarcloud analysis') {
            steps {
                sh 'mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=sriudayprofile-projects_su-analysis'
            }
        }
    }
}