pipeline{
    agent any
    environment{
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials')
    }
    stages{
        stage('Clone Repository'){
            steps{
                git branch: 'main' , url:'https://github.com/SunidhiSG/todo-application.git
            }
        }

        stage('Build with maven') {
            steps{
                sh 'mvn clean package -DskipTests'
            }
        } 

        stage('Build Docker image'){
            steps{
                sh 'docker build -t todo-application-image:latest .'
            }
        }  
        stage ('Push Docker image to Docker Hub'){
            steps{
                sh 'docker login -u $DOCKER_HUB_CREDENTIALS_USER -p $DOCKER_HUB_CREDENTIALS_PSW'
                sh 'docker tag todo-application-image:latest sunidhig1/todo-application:latest'
                sh 'docker push sunidhi1/todo-application:latest' 
                }
        }

        stage('Deploy with docker compose'){
            steps{
                sh 'docker compose up -d'
                }
        }  

        stage('verify service'){
            steps{
                sh 'docker ps'
            }
        } 

        stage ('Clean workspace'){
            steps{
                sh 'rm -rf *'
            }
        } 
    }
    
    post{
        success{
            echo 'Build and Deployment successful'
        }
        failure{
            echo 'Build or Deployment failes'
        }
        
    }
}

