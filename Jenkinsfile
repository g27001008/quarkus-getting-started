pipeline {
    
    stages {
        stage("load-test") {                    
            agent {
                dockerfile {
                    filename "src/test/jmeter/Dockerfile"
                }
            }
            
            environment {
                JMETER_TEST_PLAN = "src/test/jmeter/TestQuarkusGettingStarted.jmx"
                JMETER_OUT_DIR = "${WORKSPACE}/${BUILD_NUMBER}/jmeter-outputs"        
            }
            
            steps {
                
                sh "mkdir -p ${JMETER_OUT_DIR}"
                sh "jmeter -n -t  -l ${JMETER_OUT_DIR}/test-result.jtl -e -o ${JMETER_OUT_DIR}/reports"        
                                                
                publishHTML target: [
                        allowMissing: true,
                        alwaysLinkToLastBuild: false,
                        keepAll: true,
                        reportDir: "${JMETER_OUT_DIR}/reports",
                        reportFiles: "index.html",
                        reportName: "JMeter_Report"
                ]
            }
        }
    }
}
