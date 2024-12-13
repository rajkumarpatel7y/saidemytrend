pipeline {
    agent any
    environment {
        PATH = "/opt/maven/bin:$PATH"
    }

    stages {
         stage("Build") {
            steps {
               echo "----------- Build started -----------"

               sh 'mvn clean deploy -Dmaven.test.skip=true'

               echo "---------- build completed -----------"
            }
        }


        stage("test") {
            steps {
               echo "----------- unit test started -----------"

               sh 'mvn surefire-report:report'

               echo "---------- unit test completed -----------"
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

        stage("Quality Gate") {               
            steps {                           
                script {                      
                    timeout(time: 1, unit: 'HOURS') {  
                                              
                        def qg = waitForQualityGate()  
                                              
                        if (qg.status != 'OK') {  
                                              
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"  
                                              
                        }
                    }
                }                            
            }                                 
        } 
         
    }  
}
