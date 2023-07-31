pipeline{
    agent any
    
    stages{
        stage("code pull from git"){
            steps{
                echo "pulling from git"
                git url:"https://github.com/gshrey2002/django-notes-app.git" , branch: "main"
            }
            
        }
        stage("building image with docker"){
            steps{
                echo "building image"
                sh "docker build . -t note-app"
            }
            
        }
        stage("pushing image to dockerhub"){
            steps{
                echo "pushing to dockerhub"
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubpass",usernameVariable:"dockerhubuser")]){
                sh "docker tag note-app ${env.dockerhubuser}/notedjango"
                sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                sh "docker push ${env.dockerhubuser}/notedjango"
                }
            }
            
        }
        stage("deploy"){
            steps{
                echo "deploying to EC2"
                sh "docker-compose down && docker-compose up -d"
               // sh "docker run -d -p 8000:8000 gshrey2002/notedjango"
                
            }
            
        }
    }
}
