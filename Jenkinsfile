// node {
//     checkout scm
    /* Requires the Docker Pipeline plugin to be installed */
 //    docker.image('maven:3-alpine').inside('-v $HOME/.m2:/root/.m2') {
//         stage('Build docker') {
 //            sh 'cd complete; mvn -B clean install'
//         }
 //    }
// } 
//String getSVCName(String SVC_ID) {
//    return SVC_ID.split("_")[2]
// }
pipeline {
    agent any
    environment {
        DOCKER_REGISTRY = "nicokuz/nicojenkins"
        registryCredential = 'dockerhub'
        AWS_PROFILE_NAME = "jenkins_shared"
        BASE_MVN_TAG = "0.2.0"
        AWS_ACC = "022535336011"
        AWS_CLUSTER_NAME = "dev"
        AWS_REGION = "eu-central-1"
        KUBE_CONTEXT_FLAG = "--kube-context arn:aws:eks:${AWS_REGION}:${AWS_ACC}:cluster/${AWS_CLUSTER_NAME}"
//        SVC = getSVCName(SVC_ID)
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
        choice(name: 'SVC_ID', choices: ['spring-boot', '86008_mbbb_fpi', '86024_mbbb_poi', '86007_mbbb_news',
                                         '86009_mbbb_twit', '86014_mbbb_park', '86054_mbbb_efpi',
                                         '86006_mbbb_reise', '86010_mbbb_train', '86005_mbbb_wetter',
                                         '86012_mbbb_cityev', '86017_mbbb_countr', '86021_mbbb_flight',
                                         'tdisp1-86064-core', 'tdisp1-8606401-anonym', 'tdisp1-xfcd-backend', 'evproxy', 'tqrs'], description: 'Service id')
    }
    stages {
        stage('Checkout') {
            steps {
                echo 'checkout scm..'
                checkout scm
            }
        }    
/*        stage('Build') {
            steps {            
                echo "pull mvm-image"
                sh '''docker run --rm -v ${WORKSPACE}:/src -v ~/.m2:/root/.m2 maven:3-alpine sh -c \"ls -la; cd /src/complete; mvn -B clean install\"'''
            }                
        }
*/
        stage('Create Tag') {
            steps {
                echo 'creating the tag'
                script {
                    def imgTag
                    if (env.GIT_TAG) {
                        imgTag = params.GIT_TAG
                    } else {
                        imgTag = env.GIT_COMMIT.take(8)
                    }
                    env.IMG_TAG = imgTag
                    println(imgTag)
                }
            }
        }

/*        stage('Dockerize App') {
            steps {
                echo 'dockerizing the app'
                sh 'cd complete && docker build -t ${DOCKER_REGISTRY}:${SVC_ID}_${IMG_TAG} .'
            }
        }
*/
/*        stage('Upload to privrepo') {
            steps {
                echo 'uploading app to ECR'
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                sh '''docker login --username $USERNAME --password $PASSWORD'''
                sh '''docker push ${DOCKER_REGISTRY}:${SVC_ID}_${IMG_TAG}'''
                }
            }
        }
*/
        stage('Push Tag') {
            when {
                expression { params.GIT_TAG }
            }
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'github-ssh', keyFileVariable: 'KEY_FILE')]) {
                    sh '''
                    eval `ssh-agent -s`
                    ssh-add ${KEY_FILE}
                    git config --global user.email "jenkins"
                    git config --global user.name "jenkins"
                    git tag ${IMG_TAG}
                    git push --set-upstream origin master --follow-tags
                    '''
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