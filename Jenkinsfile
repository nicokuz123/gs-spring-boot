node {

    /* Requires the Docker Pipeline plugin to be installed */
    docker.image('maven:3-alpine').inside('-v $HOME/.m2:/root/.m2') {
        stage('Build docker') {
            sh 'cd complete; mvn -B clean install'
        }
    }
}