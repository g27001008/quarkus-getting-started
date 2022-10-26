pipeline {
    
    agent none
    
    stages {
        stage("load-test") {                    
            agent {
                dockerfile {
                    filename "Dockerfile"
                    dir 'src/test/jmeter'       
                }
            }
            
            environment {
                JMETER_TEST_PLAN = "src/test/jmeter/TestQuarkusGettingStarted.jmx"
                JMETER_OUT_DIR = "${WORKSPACE}/${BUILD_NUMBER}/jmeter-outputs"        
            }
            
            steps {
                
                sh "mkdir -p ${JMETER_OUT_DIR}"
                sh "jmeter -n -t ${JMETER_TEST_PLAN} -l ${JMETER_OUT_DIR}/result.jtl -e -o ${JMETER_OUT_DIR}/report"
                sh "rm -rf ${JMETER_OUT_DIR}"
                                                
                publishHTML target: [
                        allowMissing: true,
                        alwaysLinkToLastBuild: false,
                        keepAll: true,
                        reportDir: "${JMETER_OUT_DIR}/report",
                        reportFiles: "index.html",
                        reportName: "JMeter_Report"
                ]
            }
        }
    }
}
