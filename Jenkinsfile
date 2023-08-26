def getRemoteConfig() {
    return [
        name: 'test',
        host: env.REMOTE_HOST,
        user: env.USERNAME,
        password: env.PASSWORD,
        allowAnyHosts: true
    ]
}

pipeline {
    agent {
      label 'squaier'
    }
    parameters {
        choice(name: 'ExecutionMode', choices: ['Verbose', 'Quiet'], description: 'Select the execution mode')
    }

    environment {
        REMOTE_HOST = 'staging.alfilo.org'
    }

    stages {
        stage('Clean Builders') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'creds2', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    script {
                        sshCommand remote: getRemoteConfig(), command: "cd /home/alfilo/docker-compose/ && echo ${env.PASSWORD} | sudo -S docker-compose down"
                    }
                }
            }
        }
        stage('Build Web') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'creds1', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    script {
                        if (params.ExecutionMode == 'Verbose') {
                           sshCommand remote: getRemoteConfig(), command: "cd /home/alfilo/docker-compose/ && echo ${env.PASSWORD} | sudo -S docker-compose build --no-cache"
                           sshCommand remote: getRemoteConfig(), command: "cd /home/alfilo/docker-compose/ && echo ${env.PASSWORD} | sudo -S docker-compose up -d"
                          
                        } else if (params.ExecutionMode == 'Quiet') {
                            sshCommand remote: getRemoteConfig(), command: "cd /home/alfilo/docker-compose/ && echo ${env.PASSWORD} | sudo -S docker-compose build --no-cache --quiet"
                            sshCommand remote: getRemoteConfig(), command: "cd /home/alfilo/docker-compose/ && echo ${env.PASSWORD} | sudo -S docker-compose up -d"
                        }
                    }
                }
            }
        }
    }
    post {
        always {
            // Clean workdir or perform other cleanup tasks
            sh 'rm -rf *'
            sh 'ls -l'
        }
    }
}
