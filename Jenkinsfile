
pipeline {
    agent any

    environment {
        IMAGE_NAME = "salhianis20/todo-app"
        IMAGE_TAG = "latest"
        REGISTRY_CREDENTIALS = "docker-hub-credentials"
        SONARQUBE_ENV = "SonarQubeServer"  // Set this in Jenkins config
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        // stage('SonarQube Analysis') {
        //     environment {
        //         SONAR_TOKEN = credentials('sonarqube-token')
        //     }
        //     steps {
        //         withSonarQubeEnv(SONARQUBE_ENV) {
        //             sh """
        //                 sonar-scanner \
        //                 -Dsonar.projectKey=your-project-key \
        //                 -Dsonar.sources=. \
        //                 -Dsonar.host.url=$SONAR_HOST_URL \
        //                 -Dsonar.login=$SONAR_TOKEN
        //             """
        //         }
        //     }
        // }

        // stage("Quality Gate") {
        //     steps {
        //         timeout(time: 1, unit: 'MINUTES') {
        //             waitForQualityGate abortPipeline: true
        //         }
        //     }
        // }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }

        // stage('Scan with Trivy') {
        //     steps {
        //         script {
        //             sh "trivy image --exit-code 1 --severity HIGH,CRITICAL ${IMAGE_NAME}:${IMAGE_TAG}"
        //         }
        //     }
        // }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', REGISTRY_CREDENTIALS) {
                        sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
                    }
                }
            }
        }
    }

}
