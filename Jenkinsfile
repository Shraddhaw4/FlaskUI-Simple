def img
pipeline {
    environment {
        registry = "shraddhawaghukar/flask-simpleui" //To push an image to Docker Hub, you must first name your local image using your Docker Hub username and the repository name that you created through Docker Hub on the web.
        registryCredential = 'docker-hub'
        dockerImage = ''
    }
    agent any
    stages {
        stage('checkout') {
            steps {
                git 'https://github.com/Shraddhaw4/FlaskUI-Simple.git'
            }
        }

        stage('Build Image') {
            steps {
                script {
                    img = registry + ":${env.BUILD_ID}"
                    println ("${img}")
                    dockerImage = docker.build("${img}")
                }
            }
        }



        stage('Test - Run Docker Container on Jenkins node') {
           steps {

                sh label: '', script: "docker run -d --name ${JOB_NAME} -p 5000:5000 ${img}"
          }
        }

        stage('Push To DockerHub') {
            steps {
                script {
                    docker.withRegistry( 'https://registry.hub.docker.com ', registryCredential ) {
                        dockerImage.push()
                    }
                }
            }
        }
    }
}
          
