pipeline {
    agent {
        node {
            
            label 'maven'
        }
    }
environment {
    
    PATH = "/opt/apache-maven-3.9.2/bin:$PATH"
}

    stages {
        stage('build step') {
            steps {
                sh "mvn clean deploy"

            }
        }
    }
}
