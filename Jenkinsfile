pipeline {
    agent { docker { image 'eclipse-temurin:11.0.16.1_1-jdk' } }
    stages {
        stage('build') {
            steps {
                sh 'java -version'
                sh 'cat src/main/java/com/vilathi/GreetingResource.java'
            }
        }
    }
}
