// SCRIPTED
// node {
// 	stage('Build') {
// 		echo "Build"
// 	}
// 	stage('Unit Test') {
// 		echo "UnitTest"
// 	}
// 	stage('Integration Test') {
// 		echo "Integration Test"
// 	}
// }

// DECLARATIVE
pipeline {
    agent any
    // agent { docker { } }    stages {
    environment {
        dockerHome = tool 'myDocker'
        mavenHome = tool 'myMaven'
        PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
    }
    stages {
        stage('Checkout') {
            steps {
                sh "mvn --version"
                // sh "docker version"
                echo "Checkout"
                echo "PATH: $PATH"
                echo "BUILD NUMBER: $env.BUILD_NUMBER"
                echo "BUILD ID: $BUILD_ID"
                echo "JOB NAME: $JOB_NAME"
                echo "BUILD TAG: $BUILD_TAG"
                echo "BUILD URL = $BUILD_URL"
            }
        }
        stage('Compile') {
            steps {
                sh "mvn clean compile"

            }
        }
        stage('Unit Test') {
            steps {
                sh "mvn test"
            }
        }
        stage('Integration Test') {
            steps {
                sh "mvn failsafe:integration-test failsafe:verify"
            }
        }
        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }
        stage('Build Docker Image') {
            steps {
                // "docker build -t grillsgt/currency-exchange-devops:$env.BUILD_TAG"
                script {
                    dockerImage = docker.build("grillsgt/currency-exchange-devops:${env.BUILD_TAG}");
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', 'dockerHub') {
                        dockerImage.push();
                        dockerImage.push('latest')
                    }
                }
            }
        }
    }
    post {
        always {
            echo 'Build complete.'
        }
        success {
            echo 'Build successful.'
        }
        failure {
            echo 'Build failed.'
        }
    }
}
