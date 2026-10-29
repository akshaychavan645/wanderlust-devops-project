pipeline {
    agent any

    options {
        timestamps()
        disableConcurrentBuilds()
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out main branch'
                checkout scmGit(
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/akshaychavan645/wanderlust-devops-project.git'
                    ]]
                )
            }
        }

        stage('Validate Project') {
            steps {
                sh 'test -f frontend/package.json'
                sh 'test -f frontend/package-lock.json'
                sh 'test -f frontend/Dockerfile'
                sh 'test -f frontend/.dockerignore'
                sh 'test -f backend/package.json'
                sh 'test -f backend/package-lock.json'
                sh 'test -f backend/Dockerfile'
                sh 'test -f backend/.dockerignore'
                sh 'test -f docker-compose.yml'
                echo 'Project structure verified'
            }
        }

        stage('Frontend Lint and Build') {
            steps {
                dir('frontend') {
                    sh 'sudo -n docker build --progress=plain --target ci --output type=cacheonly .'
                }
            }
        }

        stage('Backend Unit Tests') {
            steps {
                dir('backend') {
                    sh 'sudo -n docker build --progress=plain --target ci --output type=cacheonly .'
                }
            }
        }

        stage('Build Frontend Image') {
            steps {
                dir('frontend') {
                    sh 'sudo -n docker build --progress=plain -t wanderlust-frontend:ci-test .'
                }
            }
        }

        stage('Build Backend Image') {
            steps {
                dir('backend') {
                    sh 'sudo -n docker build --progress=plain -t wanderlust-backend:ci-test .'
                }
            }
        }

        stage('Verify Images') {
            steps {
                sh 'sudo -n docker image inspect wanderlust-frontend:ci-test --format "frontend={{.Id}} created={{.Created}}"'
                sh 'sudo -n docker image inspect wanderlust-backend:ci-test --format "backend={{.Id}} created={{.Created}}"'
            }
        }
    }

    post {
        success {
            echo 'CI succeeded: frontend lint/build, backend unit tests, and both images passed.'
        }
        failure {
            echo 'CI failed. Review the failing stage log above.'
        }
    }
}
