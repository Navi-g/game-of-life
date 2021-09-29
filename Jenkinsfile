pipeline {
    agent { label 'UBN'}
    triggers {
        cron('H * * * *')
        pollSCM('* * * * *')
    }
    parameters {
        string(name: 'BRANCH', defaultValue: 'master', description: 'Branch to build' )
        choice(name: 'GOAL', choices: ['package', 'clean package', 'install'], description: 'maven goals')
    }
    options {
        timeout(time: 1, unit: 'HOURS')
        retry(2)
    }
    
    }
    stages {
        stage('scm') {
            }
            steps {
                mail subject: 'BUILD Started '+env.BUILD_ID, to: 'devops@nk.com', from: 'jenkins@nk.com', body: 'The status can be viewed by clicking on below link "http://15.206.189.145:8080/" '
                git branch: "${params.BRANCH}", url: 'https://github.com/asquarezone/game-of-life.git'
                //input message: 'Continue to next stage? ', submitter: 'qtaws,qtazure'
               
            }
        }
        stage('build') {
            steps {
                echo env.GIT_URL
                timeout(time:10, unit: 'MINUTES') {
                    sh "mvn ${params.GOAL}"
                }
            }
        }
    }
    post {
        success {
            archive '**/*.war'
            junit '**/TEST-*.xml'
            mail subject: 'BUILD Completed Successfully '+env.BUILD_ID, to: 'devops@nk.com', from: 'jenkins@nk.com', body: 'EMPTY BODY'
        }
        failure {
            mail subject: 'BUILD Failed '+env.BUILD_ID+'URL is '+env.BUILD_URL, to: 'devops@nk.com', from: 'jenkins@nk.com', body: 'EMPTY BODY'
        }
        always {
            echo "Finished"
        }
        changed {
            echo "Changed"
        }
        unstable {
            mail subject: 'BUILD Unstable '+env.BUILD_ID+'URL is '+env.BUILD_URL, to: 'devops@nk.com', from: 'jenkins@nk.com', body: 'EMPTY BODY'

        }
    }
}