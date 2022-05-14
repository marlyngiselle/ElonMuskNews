pipeline {

    agent { label 'agente1' }

    tools { maven "Maven" }

    stages {

        stage('Build') {
            steps {
                sh 'mvn clean install'
                sh 'docker stop contenedor || echo "no existe contenedor corriendo"' 
                sh 'docker rm contenedor || echo "no existe contenedor para remover"'
                sh 'docker build -t vincenup/${JOB_NAME}:v${BUILD_NUMBER} .'
            }
        }

        stage('Test') {
            steps {
                echo 'Ingresa en la pagina y prueba tu mismo'
            }
        }

        stage('Release') {
            steps {
                sh 'docker tag vincenup/${JOB_NAME}:v${BUILD_NUMBER} vincenup/${JOB_NAME}:latest'
                sh 'docker login -u "vincenup" -p "85c91b79-68d8-496a-89d2-470d97fff5a6" docker.io'
                sh 'docker push vincenup/${JOB_NAME}:v${BUILD_NUMBER}'
                sh 'docker push vincenup/${JOB_NAME}:latest'
                sh 'docker rmi vincenup/${JOB_NAME}:v${BUILD_NUMBER}'
                sh 'docker rmi vincenup/${JOB_NAME}:latest'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker run -d -p 80:8080 --name contenedor vincenup/${JOB_NAME}:latest'
            }
        }

    }
}
