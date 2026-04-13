pipeline {
    agent any
    environment {
        scannerHome = tool 'mysonar';
    }
    stages {
        stage ("Code") {
            steps {
                git "https://github.com/damodharbotta-devops/ltibbhackathon.git"
            }
        }
        stage ("CQA") {
            steps {
                withSonarQubeEnv('mysonar') {
                    sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=myproject"
                }
            }
        }
        stage ('Quality gates') {
            steps {
                waitForQualityGate abortPipeline: false, credentialsId: 'mysonar-cred'
            }
        }
        stage ('Images') {
            steps {
                sh 'docker build -t dbimage database'
                sh 'docker build -t appimage .'
            }
        }
        stage ("Image Scan") {
            steps {
                sh 'trivy image dbimage'
                sh 'trivy image appimage'
            }
        }
        stage ('Tag Image') {
            steps {
                sh 'docker tag dbimage damodharbotta/bloodapp:dbimage'
                sh 'docker tag appimage damodharbotta/bloodapp:appimage'
            }
        }
        stage ('Push Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'dockerhub') {
                        sh 'docker push damodharbotta/bloodapp:dbimage'
                        sh 'docker push damodharbotta/bloodapp:appimage'
                    }
                }
            }
        }
        stage ('Deploy into container') {
            steps {
                sh 'docker run -d --name mysqldb -p 3306:3306 damodharbotta/bloodapp:dbimage'
                sh 'docker run -d --name appcont -p 1111:80 --link mysqldb:msqlcont damodharbotta/bloodapp:appimage'
            }
        }
    }
}
