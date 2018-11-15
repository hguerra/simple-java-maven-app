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
        
        stage('Deliver') {
            steps {

                println ">>>>>>>>>> Branch name: ${env.GIT_BRANCH} <<<<<<<<<<"

                script {
                    if (env.GIT_BRANCH == 'origin/master') {
                        println "\e[32m>>>>>>>>>> Deploy <<<<<<<<<<\e[0m"
                        sh './jenkins/scripts/deliver.sh'
                    } else {
                        println "\e[31m>>>>>>>>>> Build skipping the 'deployment' section :) <<<<<<<<<<\e[0m"
                    }
                }

            }
        }

    }
    
    post {
        always {
            echo 'I will always say Hello again!'

            emailext body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}",
            recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']],
            subject: "Jenkins Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}",
            to: "heitorgcarneiro@gmail.com heitorguerra@ymail.com" 
        }
    }
    
}
