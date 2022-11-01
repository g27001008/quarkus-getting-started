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
                AWS_REGION = "us-east-1"
                AWS_S3_BUCKET = "jmeter.reports"                
            }
            
            steps {               
                
                sh "mkdir -p ${JMETER_OUT_DIR}"
                
                script { 
                    s3bucketRoute = "s3://${AWS_S3_BUCKET}"
                    
                    sh "export AWS_DEFAULT_REGION=${AWS_REGION}"
                    
                    withCredentials([[
                        $class: "AmazonWebServicesCredentialsBinding",
                        credentialsId: "aws-s3-jenkins",
                        accessKeyVariable: "AWS_ACCESS_KEY_ID",
                        secretKeyVariable: "AWS_SECRET_ACCESS_KEY"]])
                    {
                        sh "aws s3 rm ${s3bucketRoute}/${JOB_NAME} --recursive"
                        
                        htmlLinks = ""
                        
                        testScenarios.eachWithIndex { scenario, index -> 
                            index += 1
                            resultFile = "${JMETER_OUT_DIR}/result-sce-${index}.jtl" 
                            reportDir = "${JMETER_OUT_DIR}/dash-report-sce-${index}"

                            sh "jmeter -JnoThreads=${scenario.noThreads} -n -t ${JMETER_TEST_PLAN} -l ${resultFile} -e -o ${reportDir}"
                            
                            sh "aws s3 sync ${reportDir} ${s3bucketRoute}/${JOB_NAME}/${BUILD_NUMBER}-${index}"
                            
                            htmlLinks += """
                                        <li>
                                            <a href="/${AWS_S3_BUCKET}/${JOB_NAME}/${BUILD_NUMBER}-${index}/index.html">jmeter-report-${BUILD_NUMBER}-${index}</a>
                                        </li>"""
                        }                         
                        
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
                                    ${htmlLinks}
                                </ul>
                            </body>
                            </html>"""
                         
                        sh "cat << '</html>' > ${JMETER_OUT_DIR}/index.html ${html}"
                        
                        sh "aws s3 cp ${JMETER_OUT_DIR}/index.html ${s3bucketRoute}/${JOB_NAME}/index.html"
                    }
                }
                
                sh "rm -rf ${JOB_WORKSPACE}"
                
                sh "echo 'JMeter Reports published at:  ${s3bucketRoute}/${JOB_NAME}/index.html'"
            }
        }
    }
}
