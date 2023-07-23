def registry = 'https://tejasnaik.jfrog.io'
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
    stage("Quality Gate"){
        steps{
            script{
          timeout(time: 1, unit: 'HOURS') {
              def qg = waitForQualityGate()
              if (qg.status != 'OK') {
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }
          }
        }
    }      
    
  }
         stage("Jar Publish") {
        steps {
            script {
                    echo '<--------------- Jar Publish Started --------------->'
                     def server = Artifactory.newServer url:registry+"/artifactory" ,  credentialsId:"artifact_cred"
                     def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}";
                     def uploadSpec = """{
                          "files": [
                            {
                              "pattern": "jarstaging/(*)",
                              "target": "libs-release-local/{1}",
                              "flat": "false",
                              "props" : "${properties}",
                              "exclusions": [ "*.sha1", "*.md5"]
                            }
                         ]
                     }"""
                     def buildInfo = server.upload(uploadSpec)
                     buildInfo.env.collect()
                     server.publishBuildInfo(buildInfo)
                     echo '<--------------- Jar Publish Ended --------------->'  
            
            }
        }   
    }
}
}
