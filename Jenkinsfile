pipeline {
    agent any
    environment {
        DOCKERHUB_USER = 'sarraah'
    }
    stages {
        stage('Récupération du projet') {
            steps { checkout scm }
        }
        stage('Build image backend') {
            steps {
                sh 'docker build -t $DOCKERHUB_USER/gestion-projets-back:${BUILD_NUMBER} -t $DOCKERHUB_USER/gestion-projets-back:latest ./backend'
            }
        }
        stage('Build image frontend') {
            steps {
                sh 'docker build -t $DOCKERHUB_USER/gestion-projets-front:${BUILD_NUMBER} -t $DOCKERHUB_USER/gestion-projets-front:latest ./frontend'
            }
        }
        stage('Push Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DH_USER', passwordVariable: 'DH_PASS')]) {
                    sh '''
                      echo "$DH_PASS" | docker login -u "$DH_USER" --password-stdin
                      docker push $DOCKERHUB_USER/gestion-projets-back:${BUILD_NUMBER}
                      docker push $DOCKERHUB_USER/gestion-projets-back:latest
                      docker push $DOCKERHUB_USER/gestion-projets-front:${BUILD_NUMBER}
                      docker push $DOCKERHUB_USER/gestion-projets-front:latest
                    '''
                }
            }
        }
    }
    post {
        success { echo 'Images construites et poussées sur Docker Hub' }
        failure { echo 'Échec : voir la Console Output' }
    }
}
