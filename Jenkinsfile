pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'printenv'
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        
        echo ">>>>>>>>>> Branch name: ${env.GIT_BRANCH} <<<<<<<<<<"
        if (env.GIT_BRANCH == 'origin/master') {
            stage('Deliver') {
                steps {
                    sh './jenkins/scripts/deliver.sh'
                }
            }
        }
    }
}
