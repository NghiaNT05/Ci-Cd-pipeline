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
    }
    stages{
        stage("Checkout from scm"){
            steps{
                git branch: 'main',credentialsId: 'github',url: 'https://github.com/NghiaNT05/Ci-Cd-pipeline'
            } 
        }
    }
}