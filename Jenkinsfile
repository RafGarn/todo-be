pipeline {
    agent any
    environment {     
    DOCKERHUB_CRE=credentials('dockerhub credentials')
    } 
    stages {
        stage('Build') {
            steps {
                sh "DOCKER_BUILDKIT=1 docker build -t rafaelagar/todo-be:jenkins-${env.BUILD_ID} -t rafaelagar/todo-be:latest --target BUILD -f Dockerfile-pipeline"
            }
        }
   
        stage('Delivery artifact') {
            steps {
                sh "DOCKER_BUILDKIT=1 docker build -t rafaelagar/todo-be:jenkins-${env.BUILD_ID} -t rafaelagar/todo-be:latest --target DELIVERY -f Dockerfile-pipeline"
            }
        }
        stage('Cleanup') {
            steps {
                echo 'cleanup stage'
            	sh 'docker system prune'
            }
        }    


        stage('Push') {
            steps {
            	echo 'Pushing image to dockerhub'
                sh "sudo docker login -u ${DOCKERHUB_CRE} -p ${DOCKERHUB_CRE_PSW}"
                sh 'sudo docker push rafaelagar/todo-be'
                sh 'docker logout' 
            }
        }
    }
}
