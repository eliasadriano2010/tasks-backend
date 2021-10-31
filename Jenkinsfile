pipeline{
    agent any

    tools {
        maven 'MAVEN_LOCAL'
    }

    stages{
        stage('Build backend'){
            steps{
                sh 'mvn clean package -DskipTests=true'
            }
        }
        stage('Unit tests'){
            steps{
                sh 'mvn test'
            }
        }
        stage('Sonar analysis'){
            environment{
                scannerHome = tool 'SONAR_SCANNER'
            }
            steps{
                withSonarQubeEnv('SONAR_LOCAL'){
                    sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://9a4e-186-211-15-215.ngrok.io -Dsonar.login=b9bba2124e1aebae820734970a804ae9c86b5ad5 -Dsonar.java.binaries=target"
                }
            } 
        }
        stage('Quality Gates'){
            steps{
                sleep(10)
                timeout(time: 2, unit: 'MINUTES'){
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        
    }
}

