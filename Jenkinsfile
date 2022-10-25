pipeline {
    agent {
        dockerfile {
            filename 'src/test/jmeter/Dockerfile'
        }
    }
    
    stages {
        stage('build') {
            steps {
                sh 'java -version'
                sh 'jmeter -version'
            }
        }
    }
}
