pipeline {
    agent any

    triggers {
       pollSCM("* * * * *")
       upstream(upstreamProjects:'project1', threshold: hudson.model.Result.SUCCESS)
    }
    parameters { string(name: 'DEPLOY_ENV', defaultValue: 'staging', description: '') }
    
    options { 
            buildDiscarder(logRotator(numToKeepStr: '2')) 
            disableConcurrentBuilds()
            retry(2)
            timeout( time: 2, unit: 'SECONDS')
        }
    environment {
            JAVA_VERSION='1.8'
            GIT_HUB_CREDS = credentials('github');
    }

    stages {
        stage('Compile') {
            steps {
                sh 'echo Hello World'
                sh  'echo $JAVA_VERSION'
                sh 'echo GITHUB USER : $GIT_HUB_CREDS_USR'
            
            }
        }
        stage('Test') {
            environment {
                    JAVA_VERSION='1.7'
                    GIT_HUB_CREDS = credentials('github');
                }
            steps {
                sh  'echo $JAVA_VERSION'
                sh 'echo Test'
                sh 'echo GITHUB USER : $GIT_HUB_CREDS'
                sleep 5;
        
            }
        }
    }

    post {
        always {
             slackSend channel: '#devopsjune', message:  env.BUILD_DISPLAY_NAME  + ' Completed'
        }
        changed {
             sh 'echo build changed'
    
        }
        fixed {
            sh 'echo wohha build is not fixed'
        }
        failure {
            slackSend channel: '#devopsjune', message: env.BUILD_ID + 'Failed'
        }
        success {
            sh 'echo Project is successfull'
        }
        unstable {
             slackSend channel: '#devopsjune', message: env.BUILD_ID + 'Failed'
        }
    }
}