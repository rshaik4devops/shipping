pipeline{
    agent any
    tools{
        maven 'maven'
        jdk 'JDK17'
    }
    options{
        timestamps()
    }
    environment{
        SONAR_PROJECT_KEY = 'shipping-1'
        SONAR_PROJECT_NAME = 'shipping-1'

    }
    stages{
        stage('SCM Checkout'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/rshaik4devops/shipping.git']])
            }
        }
        stage('Build'){
            steps{
                sh 'mvn -B clean package'
            }

        }
        stage('SonarQube Scan'){
            steps{
                withSonarQubeEnv('sonar'){
                    sh '''
                      mvn -B org.sonarsource.scanner.maven:sonar-maven-plugin:sonar \
                      -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                      -Dsonar.projectName=${SONAR_PROJECT_NAME}
                      

                    '''
                }
            }
        }
        stage('Qualitygate'){
            steps{
                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }



    }
}

