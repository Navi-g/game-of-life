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
    stages {
        stage('scm') {
            steps {
                mail subject: 'BUILD Started '+env.BUILD_ID, to: 'devops@nk.com', from: 'jenkins@nk.com', body: 'EMPTY BODY'
                git branch: "${params.BRANCH}", url: 'https://github.com/Navi-g/game-of-life.git'
                   }
                      }
        stage('build') {
            steps {
               sh "mvn ${params.GOAL}"
                    }
                        }
    post {
        success {
            archive '**/gameoflife.war'
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
}
