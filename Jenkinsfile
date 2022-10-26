pipeline {
    agent {
        dockerfile {
            filename 'src/test/jmeter/Dockerfile'
        }
    }
    
    environment {
        JMETER_OUT_DIR = '/tmp/jmeter-outputs'        
        WORKSPACE = pwd()
    }
    
    stages {
        stage('load-test') {
            
            steps {
                
                //sh 'mkdir -p ${JMETER_OUT_DIR}'
                //sh 'jmeter -n -t src/test/jmeter/TestQuarkusGettingStarted.jmx -l ${JMETER_OUT_DIR}/test-result.jtl -e -o ${JMETER_OUT_DIR}/reports'
                //sh 'cat ${JMETER_OUT_DIR}/test-result.jtl'
                
                sh 'echo "Hola Mundo"'
                sh 'echo ${WORKSPACE}'
                                                
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
