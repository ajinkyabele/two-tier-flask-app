pipeline {
    agent { label "dev" };
    stages{
        stage("Code Clone") {
        steps{
            git url: "https://github.com/ajinkyabele/two-tier-flask-app.git", branch: "master"
        }
    }
        stage("Build") {
        steps{
            sh "docker build -t my-flask-app ."
        }
    }
        stage("Test") {
        steps{
            echo " The code is Tested fine"
        }
    }
        stage("Docker hub") {
            steps{
                withCredentials([usernamePassword(
                    credentialsId: "credDockerHub",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                    )]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker tag my-flask-app ${env.dockerHubUser}/my-flask-app:latest"
                sh "docker push ${env.dockerHubUser}/my-flask-app:latest"
            }
        }
        }
        stage("Deploy") {
        steps{
            sh "docker compose up -d --build flask-app"
        }
    }
  }
    


    post {
        success {
            emailext subject: "Jenkins Build Success - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                     body: "Good news! 🎉 The Jenkins build **${env.JOB_NAME} #${env.BUILD_NUMBER}** completed successfully.\n\nCheck the build details: ${env.BUILD_URL}",
                     to: "ajinkyabele163@gmail.com"
        }
        
        failure {
            emailext subject: "Jenkins Build Failed - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                     body: "Oops! 🚨 The Jenkins build **${env.JOB_NAME} #${env.BUILD_NUMBER}** has failed.\n\nCheck the build logs for details: ${env.BUILD_URL}",
                     to: "ajinkyabele163@gmail.com"
        }
    }
    
}
