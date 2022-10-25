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
                sh 'mkdir -p /tmp/jmeter-outputs'
                sh 'jmeter -n -t src/test/jmeter/TestQuarkusGettingStarted.jmx -l /tmp/jmeter-outputs/test-result.jtl -e -o /tmp/jmeter-outputs/report'
            }
        }
    }
}
