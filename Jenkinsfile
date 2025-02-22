pipeline {
    
    agent any
    
    tools{
        jdk 'jdk17'
        maven 'maven'
    }
    
    stages {
        stage("Code Checkout"){
            steps{
                git branch:'main',url:'https://github.com/vijeshdevops89/Task-Master.git'
            }
        }
        
        stage('Code Build'){
            steps{
                sh 'mvn clean package'
            }
        }
        
        stage('Docker build and tag') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-creds') {
                        sh '''
                        docker build -t task .
                        docker tag task vijesh89/taskmaster:${BUILD_NUMBER}
                        '''
                    }
                }
            }
            
        }
        
        stage('Docker Push'){
            steps{
                script{
                    withDockerRegistry(credentialsId:'docker-creds'){
                        sh'''
                        docker push vijesh89/taskmaster:${BUILD_NUMBER}
                        '''
                    }
                }
            }
        }
        
        stage('Stop and Remove Running Container'){
            steps{
                script{
                    withDockerRegistry(credentialsId:'docker-creds'){
                        sh'''
                        docker stop taskapp
                        docker rm taskapp
                        '''
                    }
                }
            }
        }
        
        stage('Create and Run Container'){
            steps{
                script{
                    withDockerRegistry(credentialsId:'docker-creds'){
                        sh'''
                        docker run -d --name taskapp -p 8081:8080 vijesh89/taskmaster:${BUILD_NUMBER}
                        '''
                    }
                }
            }
        }
    }
}
