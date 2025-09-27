pipeline {
    agent any

    stages {
        stage('Code Pull') {
            steps {
                echo 'Pulling Code'
                git url:"https://github.com/vibhishh/static-app.git",branch: "main"
                echo 'Code Pulled Successfully'
            }
        }
       stage('Docker Build & Push') {
            steps {
                script {
                    // Use DockerHub credentials securely
                    withCredentials([usernamePassword(credentialsId: 'dockerID', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh '''
                        #!/bin/bash
                        echo ">>> Logging into DockerHub..."
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin

                        echo ">>> Building image..."
                        docker build -t $DOCKER_USER/static-app:latest .

                        echo ">>> Pushing image..."
                        docker push $DOCKER_USER/static-app:latest
                        '''
                    }    
                }
            }
        }
       stage('Deploy the code') {
           steps {
                script {
                    // Use DockerHub credentials securely
                    withCredentials([usernamePassword(credentialsId: 'dockerID', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                      sh 'docker run -d -p 80:80 --name=static-app $DOCKER_USER/static-app:latest'
                    }
                }
           }
        }
    }
}
