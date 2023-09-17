#!groovy
//  groovy Jenkinsfile
properties([disableConcurrentBuilds()])

pipeline  {
        agent { 
           label ''
        }

    options {
        buildDiscarder(logRotator(numToKeepStr: '10', artifactNumToKeepStr: '10'))
        timestamps()
    }
    stages {
        stage("Create docker image") {
            steps {
                echo 'Creating docker image ...'
                dir('.'){
                    sh "docker build --no-cache -t yurashupik/website  . "
                }
            }
        }
        stage("docker login") {
            steps {
                echo " ============== docker login =================="
                withCredentials([usernamePassword(credentialsId: 'DockerHub-Credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh '''
                    docker login -u $USERNAME -p $PASSWORD
                    '''
                }
            }
        }
        stage("docker push") {
            steps {
                echo " ============== pushing image =================="
                sh '''
                docker push yurashupik/website:latest
                '''
            }
        }
        stage("docker stop") {
            steps {
                echo " ============== stopping all images =================="
                sh '''
                docker stop website
                '''
            }
        } 
        stage("docker remove") {
            steps {
                echo " ============== removing all docker containers =================="
                sh '''
                docker rm  website 
                '''
            }
        }
        stage("docker run") {
            steps {
                echo " ============== start server =================="
                sh '''
                docker run -d --restart=always --name website -p 80:80 yurashupik/website
                '''
            }
        }
    }
}

