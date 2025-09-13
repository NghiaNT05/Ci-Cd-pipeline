pipeline{
    agent{
        label "jenkinsagent"

    }
    tools{
        jdk 'Java21'
        maven 'Maven3'
    }
    environment{
        APP_NAME = "ci-cd-pipeline"
        RELEASE = "1.0.0"
        DOCKER_USER = "nghiant2005"
        DOCKER_PASS = 'dockerhub'
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG   = "${RELEASE}-${BUILD_NUMBER}"
	    JENKINS_API_TOKEN= credentials("JENKINS_API_TOKEN")
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
                waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube'                    }
                }
            }
	 stage("Trivy Scan") {
            steps {
                script {
		   sh ('docker run -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image ${IMAGE_NAME}:${IMAGE_TAG} --no-progress --scanners vuln --exit-code 0 --severity HIGH,CRITICAL --format table')
                }
            }

        }

        stage ('Cleanup Artifacts') {
            steps {
                script {
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"
                }
            }
        }


             stage("Build & Push Docker Image") {
            steps {
                script {
                    def docker_image = null

                    docker.withRegistry('', 'dockerhub') {
                        docker_image = docker.build("${IMAGE_NAME}")
                    }

                    docker.withRegistry('', 'dockerhub') {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }
        }
              stage("Trigger cd pipeline") {
            steps {
                script {
                    sh """
                        curl -v -k --user admin:${JENKINS_API_TOKEN} \\
                        -X POST \\
                        -H 'cache-control: no-cache' \\
                        -H 'content-type: application/x-www-form-urlencoded' \\
                        --data 'IMAGE_TAG=${IMAGE_TAG}' \\
                        'http://192.168.122.237:8080/job/gitops-complete-pipeline/buildWithParameters?token=gitops-token'
                    """
                }
            }
        }
    }  
}     
