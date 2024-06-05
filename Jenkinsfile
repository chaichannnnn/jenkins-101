pipeline {
    agent any
    tools{
        maven 'maven_3_5_0'
    }
    // environment {
    //     DOCKER_HOME = 'C://Program Files//Docker//Docker//resources//bin//docker.exe'  // Path to the Docker executable
    // }
    stages{
        stage('Build Maven'){
            steps{
                // checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Janescience/discovery-service']]])
                bat 'mvn clean install'
            }
        }
        stage('Build image'){
            steps{
                script{
                    bat "docker build -t discovery-service:${env.BUILD_NUMBER} ."
                }
            }
        }
        
        stage('Push image to Hub'){
            steps{
                script{
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        bat "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                        bat "docker tag discovery-service:${env.BUILD_NUMBER} $DOCKER_USERNAME/discovery-service:${env.BUILD_NUMBER}"
                        bat "docker push $DOCKER_USERNAME/discovery-service:${env.BUILD_NUMBER}"
                        //bat 'docker rmi -f "$(docker images -f "dangling=true" -q)"'
                    }
                }
            }
        }
        // stage('Deploy to k8s'){
        //     steps{
        //         script{
        //             kubernetesDeploy (configs: 'k8s/deployment.yml',kubeconfigId: 'k8sconfigpwd')
        //             kubernetesDeploy (configs: 'k8s/service.yml',kubeconfigId: 'k8sconfigpwd')
        //             // bat 'kubectl apply -f k8s/deployment.yml'
        //             // bat 'kubectl apply -f k8s/service.yml'
        //         }
        //     }
        
        // }
    }
}
