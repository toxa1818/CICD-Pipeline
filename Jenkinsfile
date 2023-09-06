pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                script {
                    sh 'scripts/build.sh'
                }
            }
        }
        
        stage('Test') {
            steps {
                script {
                     sh 'scripts/test.sh'
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        sh 'docker build -t nodemain:v1.0 .'
                    } else if (env.BRANCH_NAME == 'dev') {
                        sh 'docker build -t nodedev:v1.0 .'
                    }
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        sh 'docker stop nodemain || true'
                        sh 'docker rm nodemain || true'
                        sh 'docker run -d --expose 3000 -p 3000:3000 nodemain:v1.0'
                    } else if (env.BRANCH_NAME == 'dev') {
                        sh 'docker stop nodedev || true'
                        sh 'docker rm nodedev || true'
                        sh 'docker run -d --expose 3001 -p 3001:3000 nodedev:v1.0'
                    }
                }
            }
        }
    }

}
