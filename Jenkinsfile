pipeline {
    agent any

    environment {
        JAVA_HOME = '/usr/lib/jvm/java-17-openjdk-amd64'
        PATH = "${env.JAVA_HOME}/bin:${env.PATH}:/usr/share/maven/bin"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/MohamedSkanderHaguiga/Projet_Devops.git'
            }
        }

        stage('Build') {
            steps {
                echo "⚙️ Compilation du projet Maven"
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                echo "🧪 Exécution des tests Maven"
                // Ajouter -B pour mode batch et éviter les interactions
                sh 'mvn test -B'
            }
            post {
                always {
                    echo "📄 Publication des rapports de tests"
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package') {
            steps {
                echo "📦 Création du package Maven"
                // Skip tests car ils sont déjà exécutés à l'étape Test
                sh 'mvn package -DskipTests -B'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo "✅ Build et pipeline réussis !"
        }
        failure {
            echo "❌ Build échoué, vérifier les logs"
        }
    }
}
