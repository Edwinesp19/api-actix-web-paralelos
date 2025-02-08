pipeline {
    agent any

    stages {
        stage('Descargar Código') {
            steps {
                git branch: 'main', url: 'https://github.com/Edwinesp19/tarea-2-api-paralelos'
            }
        }

        stage('Construir y Probar') {
            steps {
                sh 'echo "Compilando código..."'
                sh 'echo "Ejecutando pruebas..."'
            }
        }

        stage('Desplegar en Servidor') {
            when {
                branch 'main'
            }
            steps {
                sh 'echo "Desplegando en el servidor..."'
                // Aquí puedes agregar comandos para desplegar la imagen Docker en tu servidor
            }
        }
    }
}
