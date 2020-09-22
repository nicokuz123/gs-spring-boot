node {
    def mvnHome
    stage('Preparation') { // for display purposes
        // Get some code from a GitHub repository
        // git 'https://github.com/nicokuz123/gs-spring-boot.git'
        // Get the Maven tool.
        // ** NOTE: This 'M3' Maven tool must be configured
        // **       in the global configuration.
        mvnHome = tool 'M3'
    }
    stage('Build') {
        // Run the maven build
        withEnv(["MVN_HOME=$mvnHome"]) {
        //    if (isUnix()) {
                sh 'cd complete;"$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore clean package'
         //   } else {
         //       bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
        }
       //  }
    }
     stage('Results') {
        junit '**/target/surefire-reports/TEST-*.xml'
        archiveArtifacts 'complete/target/*.jar'
    }

    //    stage('Results') {
    //    junit '**/target/surefire-reports/TEST-*.xml'
    //    archiveArtifacts 'complete/target/*.jar'
    //}
}

node {
    /* Requires the Docker Pipeline plugin to be installed */
    docker.image('maven:3-alpine').inside('-v $HOME/.m2:/root/.m2') {
        stage('Build') {
            sh 'cd complete; mvn -B clean package'
        }
    }
}
