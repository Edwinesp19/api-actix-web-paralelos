pipeline {
    agent any
    environment {
        DOCKER_REGISTRY = "167.71.164.51:8082"
        DOCKER_IMAGE = "api-actix-web"
        DOCKER_TAG = "latest"
        SERVER_USER = "root"
        SERVER_IP = "167.71.17.8"
        SSH_CREDENTIALS = "ssh-server-credentials"
    }
    stages {
        stage('Checkout') {
            steps {
                echo "📥 Clonando código fuente desde GitHub..."
                git branch: 'develop', credentialsId: 'github-credentials', url: 'https://github.com/Edwinesp19/api-actix-web-paralelos.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                echo "🔨 Construyendo imagen Docker..."
                sh "docker build -t $DOCKER_REGISTRY/$DOCKER_IMAGE:$DOCKER_TAG ."
            }
        }
        stage('Login to Nexus') {
            steps {
                echo "🔑 Iniciando sesión en Nexus..."
                sh "docker login -u admin -p 'Edwin1891' $DOCKER_REGISTRY"
            }
        }
        stage('Push to Nexus') {
            steps {
                echo "📤 Subiendo imagen a Nexus..."
                sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE:$DOCKER_TAG"
            }
        }
        stage('Deploy to Server') {
            steps {
                echo "🚀 Desplegando aplicación en el servidor..."
                script {
                    sshagent(credentials: [SSH_CREDENTIALS]) {
                        sh """
                        ssh -o StrictHostKeyChecking=no $SERVER_USER@$SERVER_IP << 'ENDSSH'
                        docker pull $DOCKER_REGISTRY/$DOCKER_IMAGE:$DOCKER_TAG
                        docker stop $DOCKER_IMAGE || true
                        docker rm -f $DOCKER_IMAGE || true
                        docker run -d --restart unless-stopped --name $DOCKER_IMAGE -p 8081:8081 \
                        -e DATABASE_URL=mysql://root:root@mysql:3306/rest_db \
                        -e RUST_LOG=debug \
                        $DOCKER_REGISTRY/$DOCKER_IMAGE:$DOCKER_TAG
                        exit
                        ENDSSH
                        """
                    }
                }
            }
        }
    }
    post {
        success {
            echo "🎉 Despliegue exitoso de API Actix Web!"
        }
        failure {
            echo "🚨 ERROR en el despliegue!"
        }
    }
}