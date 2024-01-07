def COLOR_MAP = [
    'SUCCESS' : 'good',
    'FAILURE' : 'danger',
]
pipeline {
    agent any

    tools {
        jdk "OracleJDK8"
        maven "MAVEN3"
    }

    environment {
        scannerHome = tool 'sonar4'
        aws_cred = 'ecr:us-east-1:aws-token'
        app_url = 'https://064729727835.dkr.ecr.us-east-1.amazonaws.com'
        image_url = '064729727835.dkr.ecr.us-east-1.amazonaws.com/vprofile'
        image_name = 'vprofile'
  
    }

    stages {

        stage('fecth code') {

            steps {
                git branch:'main', url: 'https://github.com/sriuday19/vprofile-ci-docker.git'
            }
        }

        stage('Test') {

            steps {
                sh 'mvn test'
            }
        }

        stage('Checkstyle Analysis') {

            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }

        stage('Code Analysis withe Sonqrqube') {

            steps {

                withSonarQubeEnv("sonar") {
                    sh "${scannerHome}/bin/sonar-scanner \
                    -Dsonar.projectKey=vprofile \
                    -Dsonar.projectName=vprofile-app \
                    -Dsonar.projectVersion=1.0 \
                    -Dsonar.sources=src/ \
                    -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                    -Dsonar.junit.reportsPath=target/surefire-reports/ \
                    -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                    -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml "
                    
                }
            }
        }

        stage('Quality-gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Building image') {
            steps {
                script {
                    docker_image = docker.build(image_url + ":${env.BUILD_NUMBER}", "./Dockerfiles/")
                }
            }
        }

        stage('Pushing image to ecr') {
            steps {
                script {
                    docker.withRegistry(app_url, aws_cred) {
                        docker_image.push("${env.BUILD_NUMBER}")
                        docker_image.push('latest')
                    }
                }
            }
        }
    }
    post {
       always {
          echo "slack notification"
          slackSend(
            channel: '#vprofile-app',
            color: COLOR_MAP[currentBuild.currentResult],
            message: "*${currentBuild.currentResult}:* - job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n More info at ${env.BUILD_URL}"
          )
       }
    }
}