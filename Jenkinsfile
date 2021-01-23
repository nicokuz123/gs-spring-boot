// node {
//     checkout scm
    /* Requires the Docker Pipeline plugin to be installed */
 //    docker.image('maven:3-alpine').inside('-v $HOME/.m2:/root/.m2') {
//         stage('Build docker') {
 //            sh 'ls -la'
//             sh 'pwd'
 //            sh 'cd complete; mvn -B clean install'
//         }
 //    }
// } 

pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'checkout scm..'
                checkout scm
                docker.image('maven:3-alpine').inside('-v $HOME/.m2:/root/.m2') {
                  stage('Build docker') {
                  sh 'ls -la'
                  sh 'pwd'
                  sh 'cd complete; mvn -B clean install'
                  }
                }  
            }    
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}