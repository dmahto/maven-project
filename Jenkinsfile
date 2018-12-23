pipeline {
    agent any

    parameters {
		 string(name: 'tomcat_dev', defaultValue: '18.219.205.149', description: 'Staging Server')
         	 string(name: 'tomcat_prod', defaultValue: '18.191.121.165', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

	  stage ('Copy artifact'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        echo 'copying artifact'
		    	bat "xcopy .\webapp\target\*.war D:/Softwares/DevOpsTraining/artifact"
                    }
                }
            }
        }
	
        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        bat "echo y | pscp -i D:/Softwares/DevOpsTraining/tomcat-demo.ppk 'D:/Softwares/DevOpsTraining/artifact/*.war' ec2-user@${params.tomcat_dev}:/var/lib/tomcat/webapps/"
                    }
                }
            }
        }
    }
}
