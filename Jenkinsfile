pipeline{
    agent any
    tools{
        maven 'Maven_3.9.0'
    }
    stages{
        stage('SCM checkout'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/thanigaivels21/java-maven-app.git']])
            }
        }
        stage('Maven Build'){
            steps{
                sh 'mvn clean install'
            }
        }
        stage('sonar scan'){
            steps{
                withSonarQubeEnv('Sonarqube'){
                    sh "${tool("sonar_4.8")}/bin/sonar-scanner \
                    -Dsonar.host.url=http://ec2-13-233-115-54.ap-south-1.compute.amazonaws.com:9000/ \
                    -Dsonar.login=sqp_3736f04ec910bc7a25a990227446b9a45be7e56a \
                    -Dsonar.projectKey=java-maven-app \
                    -Dsonar.java.binaries=target/'*'"
                }
            }
        }
        stage('Nexus upload'){
            steps{
                sh 'mvn -s settings.xml clean deploy'
            }
        }
    }

}