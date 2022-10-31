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
                dockerfile {
                    filename "Dockerfile"
                    dir 'src/test/jmeter'       
                }
            }
            
            environment {
                JMETER_TEST_PLAN = "src/test/jmeter/TestQuarkusGettingStarted.jmx"
                JOB_WORKSPACE = "${WORKSPACE}/${BUILD_NUMBER}"
                JMETER_OUT_DIR = "${JOB_WORKSPACE}/jmeter-outputs"
                S3_BUCKET = "s3://jmeter.reports"
            }
            
            steps {
                
                sh "mkdir -p ${JMETER_OUT_DIR}"
                
                script {
                    testScenarios.eachWithIndex { scenario, index -> 
                        index+=1
                        resultFile = "${JMETER_OUT_DIR}/result-sce-${index}.jtl" 
                        reportDir = "${JMETER_OUT_DIR}/dash-report-sce-${index}"

                        sh "jmeter -JnoThreads=${scenario.noThreads} -n -t ${JMETER_TEST_PLAN} -l ${resultFile} -e -o ${reportDir}"

                        withCredentials([[
                            $class: "AmazonWebServicesCredentialsBinding",
                            credentialsId: "aws-s3-jenkins",
                            accessKeyVariable: "AWS_ACCESS_KEY_ID",
                            secretKeyVariable: "AWS_SECRET_ACCESS_KEY"]])
                        {
                            //sh "aws s3 rm ${S3_BUCKET}/${JOB_NAME} --recursive --region=us-east-1"
 
                            sh "aws s3 sync ${reportDir} ${S3_BUCKET}/${JOB_NAME}/${BUILD_NUMBER}-${index} --delete --region=us-east-1"
                        }
                    }                    
                }
                
                sh "rm -rf ${JOB_WORKSPACE}"
            }
        }
    }
}
