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
                 sh "scp -o StrictHostKeyChecking=no deployment.yml shahid@192.168.0.7:/home/shahid/"
                 script{
                   try{
                     sh "ssh shahid@192.168.0.7 kubectl apply -f ."
                   }catch(error){
                     sh "ssh shahid@192.168.0.7 kubectl create -f ."
                   }

                 }

               }
             }
        }
    }
}
