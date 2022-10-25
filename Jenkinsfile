pipeline {
    agent { docker { image 'justb4/jmeter:latest' } }
    environment {
        @ = '-Dlog_level.jmeter=DEBUG -n -t src/test/jmeter/TestQuarkusGettingStarted.jmx'
    }
    stages {
        stage('build') {
            steps {
                sh 'java -version'                
            }
        }
    }
}
