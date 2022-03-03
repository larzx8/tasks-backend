pipeline {
    agent any
    stages {
        stage ('Build Backendd') {
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
                    sh "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=6fb27814888348e7b23a5438f7b8655f3a9948e0 -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test**,**/model/**,**Application.java"
                }                
            }
        }
        stage ('Quality Gate') {
            steps {
                sleep(5)
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }                
            }
        }
        stage ('Deploy Backend') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'TomcatLogin2', path: '', url: 'http://localhost:8001')], contextPath: '/tasks-backend', war: 'target/tasks-backend.war'
            }
        }
        stage ('API Test') {            
            steps {
                dir('api-test'){
                    git credentialsId: 'github_login', url: 'https://github.com/larzx8/tasks-api-tests'
                    sh 'mvn test'
                }
            }
        }
       stage ('Deploy Frontend') {
            steps {
                dir('frontend') {
                git credentialsId: 'github_login', url: 'https://github.com/larzx8/tasks-frontend'
                sh 'mvn clean package'
                deploy adapters: [tomcat9(credentialsId: 'TomcatLogin2', path: '', url: 'http://localhost:8001')], contextPath: 'tasks', war: 'target/tasks.war'
            }
         } 
       }
    }
}



