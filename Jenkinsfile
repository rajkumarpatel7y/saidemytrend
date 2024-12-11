pipeline {
    agent any
    environment {
        PATH = "/opt/maven/bin:$PATH"
    }

    stages {
        stage("Build") {
            steps {
                sh 'mvn clean deploy'
            }
        }    
            
        stage('SonarQube analysis') {
            environment {
                scannerHome = tool 'Sonar-qube-scanner'
            }
            
            steps {
                withSonarQubeEnv('project-sonarqube-server') {
                    
                    sh "${scannerHome}/bin/sonar-scanner"
                
                }
                
            }
        }  
         
    }  
}
