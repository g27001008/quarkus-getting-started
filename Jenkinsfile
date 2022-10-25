pipeline {
    agent {
        dockerfile {
            filename 'src/test/jmeter/Dockerfile'
        }
    }
    
    stages {
        stage('build') {
            steps {
                sh 'jmeter --version'
                sh 'jmeter -n -t src/test/jmeter/TestQuarkusGettingStarted.jmx -l /tmp/jmeter/test-result.jtl -e -o /tmp/jmeter/report
            }
        }
    }
}
