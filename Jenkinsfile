pipeline {
    agent {
        dockerfile {
            filename 'src/test/jmeter/Dockerfile'
        }
    }
    
    environment {
        JMETER_OUT_DIR = '/tmp/jmeter-outputs'        
    }
    
    stages {
        stage('load-test') {
            def workspace = pwd()
            steps {
                
                //sh 'mkdir -p ${JMETER_OUT_DIR}'
                //sh 'jmeter -n -t src/test/jmeter/TestQuarkusGettingStarted.jmx -l ${JMETER_OUT_DIR}/test-result.jtl -e -o ${JMETER_OUT_DIR}/reports'
                //sh 'cat ${JMETER_OUT_DIR}/test-result.jtl'
                
                sh 'cat ${workspace}'
                                                
                /*publishHTML target: [
                        allowMissing: true,
                        alwaysLinkToLastBuild: false,
                        keepAll: true,
                        reportDir: '${JMETER_OUT_DIR}/reports',
                        reportFiles: 'index.html',
                        reportName: 'JMeter Report'
                ]*/
                
                
            }
        }
    }
}
