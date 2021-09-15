pipeline {
    agent {
        docker {
            image 'maven:3.6.3-jdk-11'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package -B -ntp -DskipTests -Dcheckstyle.skip'
            }
        }
        stage('Deploy Artifactory') {
            steps {
                script {
                    def releases = 'pet-clinic-war-release'
                    def snapshots = 'pet-clinic-war-snapshot'
                    def server = Artifactory.server 'artifactory'
                    def rtMaven = Artifactory.newMavenBuild()
                    
                    env.MAVEN_HOME = '/usr/share/maven'
                    
                    rtMaven.deployer server: server, releaseRepo: releases, snapshotRepo: snapshots
                    def buildInfo = rtMaven.run pom: 'pom.xml', goals: 'clean install -B -ntp -DskipTests'
                    server.publishBuildInfo buildInfo
                }
            }
        }
    }
}
