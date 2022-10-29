pipeline {
    agent any
    environment {     
    DOCKERHUB_CRE=credentials('dockerhub credentials')
    } 
    stages {
        stage('Build') {
            steps {
                sh "DOCKER_BUILDKIT=1 docker build -f Dockerfile-pipeline -t rafaelagar/todo-be:jenkins-${env.BUILD_ID} --target BUILD ."
            }
        }
   
        stage('Delivery artifact') {
            steps {
                sh "DOCKER_BUILDKIT=1 docker build -f Dockerfile-pipeline -t rafaelagar/todo-be:jenkins-${env.BUILD_ID}--target DELIVERY ."
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
