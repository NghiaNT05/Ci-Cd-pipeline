pipeline{
    agent{
        label "jenkinsagent"

    }
    tools{
        jdk 'Java21'
        maven 'Maven3'
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
        scripts{
        withSonarQubeEnv(credentialsId: 'jenkins-sonarqube') {
            sh "mvn sonar:sonar"
            }
        }
    }
}
    }
}