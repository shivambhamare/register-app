pipeline {
    agent any
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
    stages {
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Checkout from SCM') {
            steps {
                checkout([$class: 'GitSCM', 
                          branches: [[name: '*/main']], 
                          userRemoteConfigs: [[url: 'https://github.com/shivambhamare/CICD']]])
            }
        }
        stage('List Workspace Files') {
            steps {
                script {
                    // List files to verify the presence of pom.xml and modules
                    sh 'ls -R'
                }
            }
        }
        stage('Build Application') {
            steps {
                script {
                    // Build the entire project
                    sh 'mvn clean package'
                }
            }
        }
        stage('Test Application') {
            steps {
                script {
                    // Run tests for the entire project
                    sh 'mvn test'
                }
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
            junit '**/target/surefire-reports/*.xml'
        }
        failure {
            // Notify of the failure (if using email notifications or other methods)
            echo 'Build failed!'
        }
        success {
            echo 'Build succeeded!'
        }
    }
}
