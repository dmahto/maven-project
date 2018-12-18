pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                echo 'starting mvn clean'
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
    }
}
