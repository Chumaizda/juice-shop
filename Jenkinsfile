pipeline {
    parameters {
        string{defaultValue: "0.0.1", description: "application Version", name: "version"}
    }
    environment {
        registry = "chumaizda"
        imageName = 'testapp'
        registryCred = 'chumaizda'
        gitProject = "https://github.com/chumaizda/juice-shop.git"
    }
    agent any
    options {
        timeout(time: 1, unit: 'HOURS')
    }
    stages {
        stage ('preparation') {
            steps {
                deleteDir()
            }
        }
        stage ('get src from git') {
            steps {
                git url: "${gitProject}"
            }
        }
        stage ('run tests') {
            steps {
                echo 'npm run test'
            }
        }
        stage ('build docker') {
            steps {
                script {
                    //pannoi + / + my-juicy-shop = chumaizda/my-juicy-shop
                    dockerImage = docker.build("${registry}/${imageName}")
                }
            }
        }
        stage ('docker publish') {
            steps {
                script {
                    docker.withRegistry("${registry}/${imageName}", "${registryCred}") {
                        dockerImage.push()
                        dockerImage.push("${version}")
                    }
                }
            }
        }
        stage ('cleaning') {
            steps {
                sh 'docker rmi -f $(docker images -a -q)'
            }
        }
    }
}