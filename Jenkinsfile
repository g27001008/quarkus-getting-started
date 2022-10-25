pipeline {
    agent { docker { image 'justb4/jmeter:latest' } }
    stages {
        stage('build') {
            steps {
                sh 'java -version'                
            }
        }
    }
}
