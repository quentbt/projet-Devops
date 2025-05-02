pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE_NAME = 'myapp-image'
        DOCKERFILE_PATH = 'projet-Devops/Dockerfile' 
        CONTAINER_NAME = 'myapp'
    }

    stages {
        stage('Nettoyer le workspace') {
            steps {
                script {
                    deleteDir() 
                }
            }
        }
        
        stage('Cloner le dépôt Git') {
            steps {
                script {
                    sh 'git clone https://github.com/quentbt/projet-Devops.git'
                }
            }
        }

        stage('Nettoyer les conteneurs et images Docker') {
            steps {
                script {
                    sh 'docker ps -a -q | xargs -r docker stop | xargs -r docker rm'
                    sh 'docker images -q | xargs -r docker rmi -f'
                }
            }
        }

        stage('Construire l\'image Docker') {
            steps {
                script {
                    sh 'docker build -f projet-Devops/Dockerfile -t ${DOCKER_IMAGE_NAME} projet-Devops'
                }
            }
        }

        stage('Déployer le conteneur Docker') {
            steps {
                script {
                    sh 'docker run -d --name ${CONTAINER_NAME} -p 8088:80 ${DOCKER_IMAGE_NAME}'
                }
            }
        }

        stage('Afficher l\'adresse IP du conteneur') {
            steps {
                script {
                    def containerIp = sh(script: 'docker inspect -f "{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}" ${CONTAINER_NAME}', returnStdout: true).trim()
                    echo "L'adresse IP du conteneur ${CONTAINER_NAME} est : ${containerIp}"
                }
            }
        }
    }
}
