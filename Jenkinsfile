pipeline {
    agent {
        dockerfile {
            filename "src/test/jmeter/Dockerfile"
        }
    }
    
    environment {
        JMETER_OUT_DIR = "${WORKSPACE}/${BUILD_NUMBER}/jmeter-outputs"
    }
    
    stages {
        stage("load-test") {
            
            steps {
                
                sh "mkdir -p ${JMETER_OUT_DIR}"
                sh "jmeter -n -t src/test/jmeter/TestQuarkusGettingStarted.jmx -l ${JMETER_OUT_DIR}/test-result.jtl -e -o ${JMETER_OUT_DIR}/reports"        
                                                
                publishHTML target: [
                        allowMissing: true,
                        alwaysLinkToLastBuild: false,
                        keepAll: false,
                        reportDir: "${JMETER_OUT_DIR}/reports",
                        reportFiles: "index.html",
                        reportName: "JMeter_Report"
                ]
            }
        }
    }
}
