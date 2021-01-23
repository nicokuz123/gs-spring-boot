// node {
//     checkout scm
    /* Requires the Docker Pipeline plugin to be installed */
 //    docker.image('maven:3-alpine').inside('-v $HOME/.m2:/root/.m2') {
//         stage('Build docker') {
 //            sh 'cd complete; mvn -B clean install'
//         }
 //    }
// } 

pipeline {
    agent any
    environment {
        ECR_URL = "052496584881.dkr.ecr.eu-central-1.amazonaws.com"
        AWS_PROFILE_NAME = "jenkins_shared"
        BASE_MVN_TAG = "0.2.0"
        AWS_ACC = "022535336011"
        AWS_CLUSTER_NAME = "dev"
        AWS_REGION = "eu-central-1"
        KUBE_CONTEXT_FLAG = "--kube-context arn:aws:eks:${AWS_REGION}:${AWS_ACC}:cluster/${AWS_CLUSTER_NAME}"
        SVC = getSVCName(SVC_ID)
        MVNTestProperty = "com.vw.mbbb.e2etests.${SVC}.**"
    }
    parameters {
        booleanParam(name: 'RELEASE', defaultValue: false, description: '')
        string(name: 'GIT_TAG', defaultValue: '', description: 'Git Tag')
        choice(name: 'ENVIRONMENT', choices: ['dev'], description: 'Target environment')
        string(name: 'NAMESPACE', defaultValue: 'blue', description: 'Target namespace')
        choice(name: 'SVC_NAME', choices: ['cityevents',
                                           'countryinfo',
                                           'efpi',
                                           'flightinfo',
                                           'fpi',
                                           'poi',
                                           'traininfo',
                                           'travelinfo',
                                           'weather',
                                           'parkinfo'], description: 'Service name')
        choice(name: 'SVC_ID', choices: ['86008_mbbb_fpi', '86024_mbbb_poi', '86007_mbbb_news',
                                         '86009_mbbb_twit', '86014_mbbb_park', '86054_mbbb_efpi',
                                         '86006_mbbb_reise', '86010_mbbb_train', '86005_mbbb_wetter',
                                         '86012_mbbb_cityev', '86017_mbbb_countr', '86021_mbbb_flight',
                                         'tdisp1-86064-core', 'tdisp1-8606401-anonym', 'tdisp1-xfcd-backend', 'evproxy', 'tqrs'], description: 'Service id')
    }
    stages {
        stage('Build') {
            steps {
                echo 'checkout scm..'
                checkout scm
                echo "pull mvm-image"
                sh '''sudo -E docker run --rm -v ${WORKSPACE} -v ~/.m2:/root/.m2 maven:3-alpine' sh 'cd complete; mvn -B clean install'''      
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