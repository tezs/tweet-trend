pipeline {
    agent {
        node {
            
            label 'maven'
        }
    }
environment {

    PATH = "/opt/apache-maven-3.9.3/bin:$PATH"
}

    stages {
        stage('build') {
            steps {
                echo "-------buid started-------"
                sh 'mvn clean deploy -Dmaven.test.skip=true'

            }
        }
        stage('test') {
            steps {
                echo " -------test started -----"
                sh 'mvn surefire-report:report'
                echo "------test completed------"

            }
        }    
    stage('SonarQube analysis') {
    environment {
      scannerHome = tool 'Tejas_sonar_scanner'
    }
    steps{
    withSonarQubeEnv('Tejas_sonar_qube') { // If you have configured more than one global server connection, you can specify its name
      sh "${scannerHome}/bin/sonar-scanner"
    }
    }
  } 
 }
}
