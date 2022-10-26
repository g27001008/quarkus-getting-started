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
                JOB_WORKSPACE = "${WORKSPACE}/${BUILD_NUMBER}"
                JMETER_OUT_DIR = "${JOB_WORKSPACE}/jmeter-outputs"
            }
            
            steps {
                
                sh "mkdir -p ${JMETER_OUT_DIR}"
                sh "jmeter -n -t ${JMETER_TEST_PLAN} -l ${JMETER_OUT_DIR}/result.jtl -e -o ${JMETER_OUT_DIR}/report"
                                                
                publishHTML target: [
                        allowMissing: true,
                        alwaysLinkToLastBuild: false,
                        keepAll: true,
                        reportDir: "${JMETER_OUT_DIR}/report",
                        reportFiles: "index.html",
                        reportName: "TestPlanReport"
                ]
                
                sh "rm -rf ${JOB_WORKSPACE}"
            }
        }
    }
}
