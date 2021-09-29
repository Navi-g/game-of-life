pipeline {
    agent { label 'UBN'}
    triggers {
        cron('H * * * *')
        pollSCM('* * * * *')
    }
    parameters {
        string(name: 'BRANCH', defaultValue: 'qa', description: 'Branch to build' )
        // choice(name: 'GOAL', choices: ['package', 'clean package', 'install'], defaultValue: 'clean package', description: 'maven goals')
    }
    options {
        timeout(time: 1, unit: 'HOURS')
        retry(2)
    }
    // environment {
    //     CI_ENV = 'DEV'
    // }
    stages {
        stage('scm') {
            // environment {
            //     DUMMY = 'FUN'
            // }
            steps {
                mail subject: 'BUILD Started '+env.BUILD_ID, to: 'devops@qt.com', from: 'jenkins@qt.com', body: 'EMPTY BODY'
                git branch: "${params.BRANCH}", url: 'https://github.com/Navi-g/game-of-life.git'
                //input message: 'Continue to next stage? ', submitter: 'qtaws,qtazure'
                // echo env.CI_ENV
                // echo env.DUMMY
            }
        }
        stage('build') {
            steps {
                // echo env.GIT_URL
                timeout(time:10, unit: 'MINUTES') {
                    sh "mvn ${params.GOAL}"
                }
                // stash includes: '**/gameoflife.war', name: 'golwar'
            }
        }
        // stage('devserver'){
        //     agent { label 'RHEL,'}
        //     steps {
        //         unstash name: 'golwar'
        //     }
        // }
    }
    post {
        success {
            archive '**/gameoflife.war'
            junit '**/TEST-*.xml'
            mail subject: 'BUILD Completed Successfully '+env.BUILD_ID, to: 'devops@qt.com', from: 'jenkins@qt.com', body: 'EMPTY BODY'
        }
        failure {
            mail subject: 'BUILD Failed '+env.BUILD_ID+'URL is '+env.BUILD_URL, to: 'devops@qt.com', from: 'jenkins@qt.com', body: 'EMPTY BODY'
        }
        always {
            echo "Finished"
        }
        // changed {
        //     echo "Changed"
        
        unstable {
            mail subject: 'BUILD Unstable '+env.BUILD_ID+'URL is '+env.BUILD_URL, to: 'devops@qt.com', from: 'jenkins@qt.com', body: 'EMPTY BODY'

          }
        }
    
}