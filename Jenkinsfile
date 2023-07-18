pipeline {
    agent {
        node {
            
            label 'maven'
        }
    }

    stages {
        stage('clone_tweet-trend-app') {
            steps {
                git branch: 'main', url: 'https://github.com/tezs/tweet-trend.git'
            }
        }
    }
}
