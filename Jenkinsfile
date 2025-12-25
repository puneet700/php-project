pipeline {
    agent any
    stages{
        stage('git cloned'){
            steps{
                git url:'https://github.com/puneet700/php-project/', branch: "master"
              
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker buildx create --use || true'
                    sh 'docker buildx build --platform linux/amd64 -t puneetdhankad/myapp:v1 --push .'
                    //sh 'docker build -t puneetdhankad/myapp:v1 .'
                    sh 'docker images'
                }
            }
        }
          stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push puneetdhankad/myapp:v1'
                }
            }
        }
        
     stage('Deploy') {
            steps {
               script {
                   def dockerrm = 'sudo docker rm -f My-first-containe2211 || true'
                    def dockerCmd = 'sudo docker run -itd --name My-first-containe2211 -p 8083:80 puneetdhankad/myapp:v1'
                    sshagent(['sshkeypair']) {
                        //chnage the private ip in below code
                        // sh "docker run -itd --name My-first-containe2111 -p 8083:80 akshu20791/2febimg:v1"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.19.249 ${dockerrm}"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.19.249 ${dockerCmd}"
                    }
                }
            }
        }
    }
}
