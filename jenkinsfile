pipeline {
    agent any
         stages{
    
    stage ('git clone') {
            steps {
        echo "code is building"
         git 'https://github.com/mounika8500/voting-app.git'
            }
        }

        stage ('Bulding docker docker image') {
            steps {
                echo "build docker image"
                sh 'docker build --no-cache -t vot .'
                sh 'docker tag vot:latest 156739282338.dkr.ecr.ap-south-1.amazonaws.com/vot:latest'
            }
        }
        stage ('Uploading to ECR') {
            steps {
                echo "uploading to ECR"
                sh 'docker push 156739282338.dkr.ecr.ap-south-1.amazonaws.com/vot:latest'
            }
        }

        stage ('deploying to EKS') {
           steps {
                echo "deploying imges to EKS"
                sh 'kubectl apply -f kube-deployment.yml'
                sh 'kubectl apply -f vote-service.yaml'
                sh 'docker rmi -f $(docker images --filter "dangling=true" -q --no-trunc)'
               
           }
    }  
        
}

}
