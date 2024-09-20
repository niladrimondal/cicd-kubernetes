pipeline {
    agent any
    stages{
        stage('Checkout the Source Code') {
            steps {
                echo 'Checkout the code..........................................'
                git 'https://github.com/niladrimondal/cicd-kubernetes.git'
            }
        }
        stage('Build Maven'){
            steps{
                // git url:'https://github.com/niladrimondal/cicd-kubernetes/', branch: "main"
               sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t niladrimondaldcr/app:v1 .'
                }
            }
        }
          stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push niladrimondaldcr/app:v1'
                }
            }
        }
        
        
        stage('Deploy to k8s'){
            when{ expression {env.GIT_BRANCH == 'master'}}
            steps{
                script{
                     kubernetesDeploy (configs: 'deploymentservice.yaml' ,kubeconfigId: 'k8sconfigpwd')
                   
                }
            }
        }
    }
}
