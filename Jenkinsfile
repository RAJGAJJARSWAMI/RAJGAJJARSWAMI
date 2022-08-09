pipeline {
    agent {
        label {
        label "built-in"
        customWorkspace "/RAJGAJJARSWAMI"
        }
    }
    stages{
        stage("clone the Repo") {
            steps {
                                sh "rm -rf *"
                sh "git clone https://github.com/RAJGAJJARSWAMI/RAJGAJJARSWAMI.git"
                                }
                        }
        stage ("build the docker image") {
            steps {
                dir ("/raj") {
                sh 'docker build -t rajgajjar/my-app .'
		echo "Hello"
                    }
                }
            }
        stage ("Upload the Image file in Docker Registry") {
            steps {
                        script{
                                withCredentials([string(credentialsId: 'rajgajjar', variable: 'dockerhub')]) {
                sh 'docker login -u rajgajjar -p${dockerhub}'
                                }
                                sh "docker push rajgajjar/my-app"
                                }
                        }
                }
                stage ("Deploy the Manifest at minikube cluster") {
            steps {
                dir ("/mithi/Assessment") {
                                    sshagent(['37bc9e8e-e305-47a3-ab36-8a620b696a57']) {
                    sh "scp -o StrictHostKeyChecking=no deploymentservice.yml ubuntu@172.31.36.143:/home/ubuntu"
					sh "ssh ubuntu@172.31.17.59 sudo kubectl delete -f ."
                    script{
                        try{
                            sh "ssh ubuntu@172.31.17.59 sudo kubectl apply -f ."
                        }catch(error){
                            sh "ssh ubuntu@172.31.17.59 sudo kubectl create -f ."
                        }
                        }
                    }
                }
            }
        }
        }
}

