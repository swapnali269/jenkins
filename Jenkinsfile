pipeline {
    agent any
    stages {
        stage('Pull') {
            steps {
                sh 'git init '
                echo "Successful pull from Git"
                git 'https://github.com/swapnali269/jenkins.git'
            }
        }
        stage('Build') {
            agent {
                label 'master'
            }
            steps {
                echo "Building with Maven"
                sh 'mvn clean package'
            }
        }
    
        stage('creating tomcat image Tomcat') {
            agent {
                label 'master'
            }
            steps {
                script {
                    sh '''cp -r /var/lib/jenkins/workspace/deploy/target/*.war .
                    docker build -t swapnali269/studentapp . 
                    docker login 
                    docker push swapnali269/studentapp'''
                }
            }
        }
        stage('build image on k8 ') {
            agent {
                label 'master'
            }
            steps {
                script {
                    sh 'kubectl apply -f deployment.yaml'
                }
            }
        }
        stage('getting info') {
            agent {
                label 'master'
            }
            steps {
                script {
                    sh '''kubectl get pods -o wide 
                    kubectl get nodes -o wide 
                    kubectl get svc -o wide 
                    ls /var/lib/jenkins/workspace/deploy/target/'''
                }
            }
        }
    }
}
