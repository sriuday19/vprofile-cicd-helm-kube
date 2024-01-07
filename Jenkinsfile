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

                withSonarQubeEnv('SonarCloud') {
                    sh ''' ${scannerHome}/bin/sonar-scanner \
                    -Dsonar.projectKey=vprofile \
                    -Dsonar.projectName=vprofile-app \
                    -Dsonar.projectVersion=1.0 \
                    -Dsonar.sources=src/ \
                    -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                    -Dsonar.junit.reportsPath=target/surefire-reports/ \
                    -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                    -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml '''

                }

            }
        }
    }
}