pipeline {
    agent {label 'slave'}
    

    stages 
    {
        stage('git checkout') {
            steps {
                git branch: 'main', 
                changelog: false, 
                poll: false, 
                url: 'https://github.com/mahmoud-m-selim/ITI-Final-App.git'
            }
        }
        
        stage('build') {
            steps {
                sh 'docker build -f dockerfile . -t mahmoudmselim/nodejs:latest'
            }
        }
        
        stage('artifact') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'pass', usernameVariable: 'username')]){
                sh 'sudo docker login -u ${username} -p ${pass}'
                sh 'sudo docker push mahmoudmselim/nodejs:latest'
            }
          }
        }
        
        stage('deploy') {
            steps {
                //sh 'kubectl create namespace my-app'
                sh 'kubectl apply -f ./app.yml -n my-app'
                sh 'kubectl apply -f ./svc.yml -n my-app'
            }
          }
        }
   }
