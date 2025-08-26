pipeline {
    agent any

    environment {
        SONAR_HOST_URL = 'http://3.95.138.160:9000'
        SONAR_AUTH_TOKEN = credentials('sonarcube')
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Checking out source code...'
                git branch: 'main', url: 'https://github.com/Tejas-kandregula/addressbook-cicd-project'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                sh 'mvn -B clean compile'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running unit tests...'
                sh 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube analysis...'
                sh """
                    mvn sonar:sonar \
                        -Dsonar.projectKey=sample_project \
                        -Dsonar.host.url=${SONAR_HOST_URL} \
                        -Dsonar.login=${SONAR_AUTH_TOKEN}
                """
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}