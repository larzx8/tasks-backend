pipeline {
    agent any
    stages {
        stage ('Build Backend') {
            steps {
                sh 'mvn clean package -DskipTests=true'
            }
        }
        stage ('Sonar Analysis') {
            environment {
               scannerHome = tool 'SONAR_SCANNER'
            }
            steps {
                withSonarQubeEnv('SONAR_LOCAL') {
                    sh "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=6fb27814888348e7b23a5438f7b8655f3a9948e0 -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test**,**/model/**,**Application.javasonar.projectKey=DeployBack sonar.host.url=http://localhost:9000 sonar.login=6fb27814888348e7b23a5438f7b8655f3a9948e0 sonar.java.binaries=target sonar.coverage.exclusions=**/.mvn/**,**/src/test**,**/model/**,**Application.java"
                }                
            }
        }
    }
}

