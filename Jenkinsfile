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
                    testScenarios.eachWithIndex{ scenario, index -> 
                        stage("load-test-scenario${index}")
                        
                        resultFile = "${JMETER_OUT_DIR}/result-${index}.jtl" 
                        reportDir = "${JMETER_OUT_DIR}/report-${index}"
                        reportName = "TestPlanReport-${index}"
                            
                        sh "jmeter -JnoThreads=${scenario.noThreads} -n -t ${JMETER_TEST_PLAN} -l ${resultFile} -e -o ${reportDir}"
                        
                        publishHTML target: [
                            allowMissing: true,
                            alwaysLinkToLastBuild: false,
                            keepAll: true,
                            reportDir: "${reportDir}",
                            reportFiles: "index.html",
                            reportName: "${reportName}"
                        ]
                    }
                }
                
                sh "rm -rf ${JOB_WORKSPACE}"
            }
        }
    }
}
