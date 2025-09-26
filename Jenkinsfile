pipeline {
    agent any

 

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/MohamedSkanderHaguiga/Projet_Devops.git'
            }
        }

        stage('Build') {
            steps {
                // Sur Windows utiliser bat au lieu de sh
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo "✅ Build réussi pour le projet Maven"
        }
        failure {
            echo "❌ Build échoué, vérifier les logs."
        }
    }
}
