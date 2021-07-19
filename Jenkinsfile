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
        stage('Checkout') {
            steps {
                sh 'mvn --version'
                sh 'docker version'
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
                sh 'mvn clean compile'

            }
        }
        stage('Unit Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Integration Test') {
            steps {
                sh 'mvn failsafe:integration-test failsafe:verify'
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
