pipeline {
    agent any
    stages {
        stage("Checkout code") {
            steps {
                checkout scm
            }
        }
        stage("Build image") {
            steps {
                script {
                    myapp = docker.build("shahid9741/kubeapp:${env.BUILD_ID}")
                }
            }
        }
        stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }
        stage('Deploying'){
             steps {
               sshagent(['kops-machine']) {
                 sh "echo admin@123 | sudo -S cp deployment.yml /home/shahid/"
                 script{
                   try{
                     sh "sudo kubectl apply -f ."
                   }catch(error){
                     sh "sudo kubectl create -f ."
                   }

                 }

               }
             }
        }
    }
}
