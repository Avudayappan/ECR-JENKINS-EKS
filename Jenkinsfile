pipeline {
    agent any
    tools {
        maven 'Maven 3.3.9'
        jdk 'jdk8'
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }

        stage ('Build') {
            steps {
                sh 'mvn -Dmaven.test.failure.ignore=true install' 
            }
        }

        stage('Create Image'){
            steps{
                script{
                    docker.withRegistry(
                    "http://608310603824.dkr.ecr.us-east-2.amazonaws.com/precision",
                    "ecr:us-east-2.amazonaws.com:ecr_creds"){
                        image = docker.build("precision/monitoring:${env.BUILD_ID}")
                        myImage.push('${env.BUILD_ID}')
                    )
                    
                }
            }
        }
    }
}
