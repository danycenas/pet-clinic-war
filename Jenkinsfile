pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'maven:3.6.3-jdk-11'
                }
            }
            steps {
                sh 'mvn clean package -B -ntp -DskipTests -Dcheckstyle.skip'
            }
        }
    }
}
