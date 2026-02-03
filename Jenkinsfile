pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Pulls source code from GitHub - gradle-demo repo [cite: 67, 70]
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                // Ensure the wrapper is executable before running 
                sh 'chmod +x gradlew'
                sh './gradlew clean test build' 
            }
        }

        stage('Archive Artifact') {
            steps {
                // Archives the generated JAR for Jenkins UI download [cite: 73, 74]
                archiveArtifacts artifacts: 'build/libs/*.jar', fingerprint: true
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Executes scan after build and test using JaCoCo reports [cite: 85, 86]
                    withSonarQubeEnv('SonarQube') {
                        sh "./gradlew sonar " +
                           "-Dsonar.projectKey=gradle-demo " +
                           "-Dsonar.projectName=gradle-demo " +
                           "-Dsonar.java.binaries=build/classes " +
                           "-Dsonar.coverage.jacoco.xmlReportPaths=build/reports/jacoco/test/jacocoTestReport.xml"
                    }
                }
            }
        }
    }
}
