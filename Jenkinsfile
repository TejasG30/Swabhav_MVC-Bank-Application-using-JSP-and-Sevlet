pipeline {
    agent any

    tools {
        jdk 'jdk17'
        maven 'maven3'
    }

    environment {
        APP_NAME = 'bankapp'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/TejasG30/Swabhav_MVC-Bank-Application-using-JSP-and-Sevlet.git',
                    credentialsId: 'github-pat'
            }
        }

        stage('Validate') {
            steps {
                sh 'ls -l'
            }
        }

        // 🔥 Build Java App
        stage('Build (Maven)') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        // 🔥 SonarQube for Java
        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'sonar-scanner'
                    withSonarQubeEnv('sonarqube') {
                        sh """
                        mvn sonar:sonar \
                        -Dsonar.projectKey=bankapp \
                        -Dsonar.projectName=BankApp
                        """
                    }
                }
            }
        }

        // 🔥 Archive artifact
        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.war', fingerprint: true
            }
        }

        // 🔥 Deploy to Tomcat (local example)
        stage('Deploy to Tomcat') {
            steps {
                sh '''
                cp target/*.war /opt/tomcat/webapps/
                '''
            }
        }
    }
}
