pipeline {
    agent {
        label 'npmbuilds'
    }
    stages{
        stage('Install bun'){
            steps{
                script{
                    container('bun-container') {
                        // Your Bun commands go here
                        sh 'bun install'
                        sh 'bun run build'
                        sh 'cp -r dist/al-filo/* /home/app/web/'
                        sh 'rm -rf'
                        // Add more Bun commands as needed
                    }
                }
            }
        }
    }
}
