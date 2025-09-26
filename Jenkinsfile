pipeline {
    agent any

    tools {
        // Nom exact de Maven configuré dans Jenkins
        maven 'Maven-3.6.3'
        // Nom exact du JDK configuré dans Jenkins
        jdk 'Java-17'
    }

    environment {
        // Définir JAVA_HOME explicitement si nécessaire
        JAVA_HOME = '/usr/lib/jvm/java-17-openjdk-amd64'
        PATH = "${env.JAVA_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Récupération du code') {
            steps {
                echo "🔄 Checkout du projet depuis GitHub"
                git branch: 'main', url: 'https://github.com/MohamedSkanderHaguiga/Projet_Devops.git'
            }
        }

        stage('Compilation') {
            steps {
                echo "⚙️ Compilation du projet Maven"
                sh 'mvn clean compile'
            }
        }

        stage('Tests') {
            steps {
                echo "🧪 Exécution des tests Maven"
                sh 'mvn test'
            }
            post {
                always {
                    echo "📄 Publication des rapports de tests"
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Packaging') {
            steps {
                echo "📦 Création du package Maven"
                sh 'mvn package -DskipTests'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo "✅ Build et pipeline réussis !"
        }
        failure {
            echo "❌ Échec du pipeline, vérifier les logs."
        }
    }
}
