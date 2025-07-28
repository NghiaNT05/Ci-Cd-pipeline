pipeline{
    agent{
        label "jenkinsagent"

    }
    tools{
        jdk 'Java21'
        maven 'Maven3'
    }
    environment{
        APP_NAME = "Ci-Cd-pipeline"
        RELEASE = "1.0.0"
        DOCKER_USER = "nghiant2005"
        DOCKER_PASS = 'dockerhub'
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG   = "${RELEASE}-${BUILD_NUMBER}"
        }
    stages{
        stage("Clean workSpace"){
            steps{
                cleanWs()
            } 
        }
        stage("Checkout from scm"){
            steps{
                git branch: 'main',credentialsId: 'github',url: 'https://github.com/NghiaNT05/Ci-Cd-pipeline'
            } 
        }
        stage("Build Application"){
            steps{
                sh "mvn clean package"
            } 
        }
        stage("Test Application"){
            steps{
                sh "mvn test"
            } 
        }
       stage("Sonarqube analysis") {
            steps {
                script {
                    withSonarQubeEnv('sonarqube') {
                        sh "mvn sonar:sonar"
                    }
                }
            }
        }
        stage("Quality Gate") {
          steps {
            script {
                waitForQualityGate abortPipeline: false,credentialsId'jenkins-sonarqube'
                    }
                }
            }
             stage("Build & Push Docker Image") {
                steps {
                    script {
                        docker.withRegistry('',DOCKER_PASS) {
                             docker_image = docker.build "${IMAGE_NAME}"}
                            docker.withRegistry('',DOCKER_PASS) {
                            docker_image.push("${IMAGE_TAG}")
                            docker_image.push('latest')
                     }
                 }
             }

        }

    }
}