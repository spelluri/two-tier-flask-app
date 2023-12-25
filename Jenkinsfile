pipeline {
    agent any 
    stages {
        stage("Clone") {
            steps {
                echo "Git Clone"
                git url: "https://github.com/spelluri/two-tier-flask-app.git", branch: "main"
                echo "Git Repo Cloned Sucessfully"
            }
        }
        stage("Build") {
            steps {
                echo "Build docker image"
                sh "docker build -t my-python-app ."
            }
        }
        stage("Login and push") {
            steps {
                echo "login to dockerhub and push image"
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubPass",usernameVariable:"dockerhubUser")]) {
                    echo "Login to Dockerhub"
                    sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPass}"
                    echo "Login Successful"
                    echo "Tagging the Image"
                    sh "docker tag my-python-app ${env.dockerhubUser}/my-python-app:$BUILD_NUMBER"
                    echo "Pushing the image"
                    sh "docker push ${env.dockerhubUser}/my-python-app:$BUILD_NUMBER"
                    echo "Docker image push done succesfully"
                }
            }
        }
        stage ("Deploy") {
            steps {
                echo "Deploying image"
                sh "docker-compose down && docker-compose up -d"
                echo "Docker Image deployed sucesfully"
                
            }
        }
       
    }
    
}
