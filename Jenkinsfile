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

        stage('buolding artifact') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('sonarcloud') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
                
            }
        }
    }
}