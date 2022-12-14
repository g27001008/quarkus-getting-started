def testScenarios = [
    [noThreads: 5],
    [noThreads: 10],
    [noThreads: 15]
]

pipeline {
    
    agent none
    
    stages {
        stage("Load Testing") {                    
            agent {
                docker { image 'gabvillacis93/jmeter:1.0' }
            }
            
            environment {
                JMETER_TEST_PLAN = "src/test/jmeter/TestQuarkusGettingStarted.jmx"
                JOB_WORKSPACE = "${WORKSPACE}/${BUILD_NUMBER}"
                JMETER_OUT_DIR = "${JOB_WORKSPACE}/jmeter-outputs"               
            }
            
            steps {               
                
                sh "mkdir -p ${JMETER_OUT_DIR}"
                
                script { 
                    testScenarios.eachWithIndex { scenario, index -> 
                        index += 1
                        resultFile = "${JMETER_OUT_DIR}/result-sce-${index}.jtl" 
                        reportDir = "${JMETER_OUT_DIR}/dash-report-sce-${index}"
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
