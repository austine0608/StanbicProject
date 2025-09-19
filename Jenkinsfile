pipeline{
    agent any

    environment{
        DOCKER _IMAGE = 'tonbra/stanbic:latest'
    }
    stages{
        stage('Checkout From GitHub'){
            steps{
                git branch: 'main',
                url: 'https://github.com/austine0608/StanbicProject.git'
                echo 'Building..'
            }
        }
        stage('Test'){
            steps{
                script{
                    sh 'chmod +x test/test_app.sh'
                    sh './test/test_app.sh'
                }
                echo 'Testing..'
            }
        }
        stage('Build Docker Image'){
            steps{
                script{
                    sh 'docker build -t nginx:alpine'
                }
                echo 'Testing..'
            }
        }
        stage('Login To Deploy Hub & Push Image'){
            steps{
                script{
                    sh 'docker push nginx:alpine'
                }
                echo 'Loging In....'
            }
        }
        stage('Deploy To Kubernetics'){
            steps{
                script{
                    sh"""
                            kubectl set image deployment/my-web-development my-web-container=$DOCKER_IMAGE:\£BUILD_NUMBER --RECORD ||\
                            kubectl apply -fk8s-deploy.yam
                    """
                }
                echo 'Deploying..'
            }
        }
    }
}
