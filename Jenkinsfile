pipeline {
    agent any

    tools {
        maven 'Maven3'   // correspond au nom configuré dans Jenkins (Manage Jenkins > Tools)
        jdk 'Java17'     // si tu as configuré un JDK dans Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/MohamedSkanderHaguiga/Projet_Devops.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'  // rapports de tests Maven
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
