pipeline {
    agent {label "dev"};

    stages {
        stage("code Clone") 
        {
            steps {
                git url: "https://github.com/Ujjwal-k19/flask-app.git", branch: "master"}
        }
        stage('Build')
        {
            steps {sh "docker build -t 2-tier-flask-app ."}
        }
        stage('Test')
        {
            steps{
                echo 'Devloper/tester code dega'}
        }
        stage('push to dockerhub'){
            steps{
                withCredentials([usernamePassword(
                    credentialsId: "dockerhubcreds",
                    usernameVariable: "dockerhubuser",
                    passwordVariable: "dockerhubpass"
                    )]){
                
                sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                sh "docker image tag 2-tier-flask-app ${env.dockerhubuser}/2-tier-flask-app"
                sh "docker push ${env.dockerhubuser}/2-tier-flask-app:latest"
            }
            }
        }
        stage('deploy')
        {
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
    }
post{
    success {
            emailext (
                to: 'ujjwalk19@hotmail.com',
                subject: "SUCCESS: ${env.JOB_NAME} - Build #${env.BUILD_NUMBER}",
                body: "Build ${env.BUILD_NUMBER} of ${env.JOB_NAME} succeeded.\n\nView details: ${env.BUILD_URL}"
            )
        }

    failure {
            emailext (
                to: 'ujjwalk19@hotmail.com',
                subject: "FAILURE: ${env.JOB_NAME} - Build #${env.BUILD_NUMBER}",
                body: "Build ${env.BUILD_NUMBER} of ${env.JOB_NAME} failed.\n\nView details: ${env.BUILD_URL}",
                attachments: 'true' // Example: include build log as attachment
            )
        }
}
}