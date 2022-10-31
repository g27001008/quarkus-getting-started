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
                
                sh "jmeter -JnoThreads=5 -n -t ${JMETER_TEST_PLAN} -l ${JMETER_OUT_DIR}/result1.jtl -e -o ${JMETER_OUT_DIR}/report1"
                                                
                publishHTML target: [
                        allowMissing: true,
                        alwaysLinkToLastBuild: false,
                        keepAll: true,
                        reportDir: "${JMETER_OUT_DIR}/report1",
                        reportFiles: "index.html",
                        reportName: "TestPlanReport1"
                ]
                
                
                sh "jmeter -JnoThreads=10 -n -t ${JMETER_TEST_PLAN} -l ${JMETER_OUT_DIR}/result2.jtl -e -o ${JMETER_OUT_DIR}/report2"
                                                
                publishHTML target: [
                        allowMissing: true,
                        alwaysLinkToLastBuild: false,
                        keepAll: true,
                        reportDir: "${JMETER_OUT_DIR}/report2",
                        reportFiles: "index.html",
                        reportName: "TestPlanReport2"
                ]
                
                sh "rm -rf ${JOB_WORKSPACE}"
            }
        }
    }
}
