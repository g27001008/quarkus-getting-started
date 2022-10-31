def testScenarios = [
    [noThreads: 5],
    [noThreads: 10],
    [noThreads: 15]
]

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
                
                script {
                    testScenarios.each{ scenario -> 
                        echo "$scenario"
                    }
                }
                
                sh "jmeter -JnoThreads=5 -n -t ${JMETER_TEST_PLAN} -l ${JMETER_OUT_DIR}/result1.jtl -e -o ${JMETER_OUT_DIR}/report1"
                                                
                publishHTML target: [
                        allowMissing: true,
                        alwaysLinkToLastBuild: false,
                        keepAll: true,
                        reportDir: "${JMETER_OUT_DIR}/report1",
                        reportFiles: "index.html",
                        reportName: "TestPlanReport1"
                ]
                
                
                
                
                sh "rm -rf ${JOB_WORKSPACE}"
            }
        }
    }
}
