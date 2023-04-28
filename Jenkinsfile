pipeline {

    agent any

    stages {

        stage("Git Checkout"){
            steps {
                git branch: 'main', url: 'https://github.com/vishalchauhan91196/java-app.git'
            }
        }

        stage("Unit Testing"){
            steps {
                sh 'mvn test'
            }
        }

        stage("Integration Testing"){
            steps {
                sh 'mvn verify -DskipUnitTests'
            }
        }

        stage("Build"){
            steps {
                sh 'mvn clean install'
            }
        }

        stage("Static Code Analysis"){
            steps {
                script {
                    withSonarQubeEnv(credentialsId: '43ab139d-9696-44b8-8c19-7b43b5ee5b26') {
                        sh 'mvn clean package sonar:sonar'
                    }
                }
            }
        }

        stage("Quality Gate Analysis"){
            steps {
                script {
                   waitForQualityGate abortPipeline: false, credentialsId: '43ab139d-9696-44b8-8c19-7b43b5ee5b26' 
                }
            }
        }

        stage("Upload Artifacts to Nexus"){
            steps {
                script {
                   nexusArtifactUploader artifacts:
                    [[
                    artifactId: 'springboot',
                    classifier: '',
                    file: 'target/UPES.jar',
                    type: 'jar'
                    ]], 
                    credentialsId: 'nexusid', 
                    groupId: 'com.example', 
                    nexusUrl: '44.200.37.98:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'java-release', 
                    version: '1.0.0'
                }
            }
        }
    }
}
