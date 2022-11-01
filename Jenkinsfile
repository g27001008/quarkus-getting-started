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
                AWS_S3_BUCKET_NAME = "jmeter.reports"
                AWS_S3_BUCKET_ROUTE = "s3://${AWS_S3_BUCKET_NAME}"
                AWS_REGION = "us-east-1"
            }
            
            steps {
                
                sh "mkdir -p ${JMETER_OUT_DIR}"
                
                script {
                     withCredentials([[
                            $class: "AmazonWebServicesCredentialsBinding",
                            credentialsId: "aws-s3-jenkins",
                            accessKeyVariable: "AWS_ACCESS_KEY_ID",
                            secretKeyVariable: "AWS_SECRET_ACCESS_KEY"]])
                     {
                        sh "aws s3 rm ${AWS_S3_BUCKET_ROUTE}/${JOB_NAME} --recursive --region=${AWS_REGION}"
                         
                        html = """
                            <!DOCTYPE html>
                            <html lang="en">
                            <head>
                                <meta charset="UTF-8">
                                <meta http-equiv="X-UA-Compatible" content="IE=edge">
                                <meta name="viewport" content="width=device-width, initial-scale=1.0">
                                <title>${JOB_NAME} JMeter Reports</title>
                            </head>
                            <body>  
                                <ul>
                                    <li>
                                        <a href="/${AWS_S3_BUCKET_NAME}/${JOB_NAME}/${BUILD_NUMBER}-1/index.html">jmeter-report-${BUILD_NUMBER}-1</a>
                                    </li>
                                    <li>
                                        <a href="/${AWS_S3_BUCKET_NAME}/${JOB_NAME}/${BUILD_NUMBER}-2/index.html">jmeter-report-${BUILD_NUMBER}-2</a>
                                    </li>
                                    <li>
                                        <a href="/${AWS_S3_BUCKET_NAME}/${JOB_NAME}/${BUILD_NUMBER}-3/index.html">jmeter-report-${BUILD_NUMBER}-3</a>
                                    </li>
                                </ul>
                            </body>
                            </html>"""
                         
                        sh "cat << '</html>' > ${JMETER_OUT_DIR}/index.html ${html}"
                         
                        testScenarios.eachWithIndex { scenario, index -> 
                            index+=1
                            resultFile = "${JMETER_OUT_DIR}/result-sce-${index}.jtl" 
                            reportDir = "${JMETER_OUT_DIR}/dash-report-sce-${index}"

                            sh "jmeter -JnoThreads=${scenario.noThreads} -n -t ${JMETER_TEST_PLAN} -l ${resultFile} -e -o ${reportDir}"
                            
                            sh "aws s3 sync ${reportDir} ${AWS_S3_BUCKET_ROUTE}/${JOB_NAME}/${BUILD_NUMBER}-${index} --region=${AWS_REGION}"
                        }                         
                         
                        sh "aws s3 cp ${JMETER_OUT_DIR}/index.html ${AWS_S3_BUCKET_ROUTE}/${JOB_NAME}/index.html"
                     }
                }
                
                sh "rm -rf ${JOB_WORKSPACE}"
            }
        }
    }
}
