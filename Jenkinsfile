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
Post{
    success{
        emailtest(
            subject: "Build successful!",
            body: "build was successfull.",
            to: 'ujjwalk19@hotmail.com'
        )
    }

    failure{
        emailtest(
            subject: "Build Failed!",
            body: "build was Failure.",
            to: 'ujjwalk19@hotmail.com'
        )
    }
}
}