pipeline {
    agent any 
    tools {
        maven 'M2_HOME'
    }
    stages{
        stage('sonarqube scan'){
            agent {docker { image 'maven:3-amazoncorretto-17-debian' }}
            steps{
                withSonarQubeEnv('sonarQube'){ 
              sh 'mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=tdh1961_week16healthcareApp'
             // sh 'mvn sonar:sonar'
            }
            }
        }
        stage('all maven commands'){
            steps{
                sh 'mvn clean test compile install package'
            }
        }
        stage('upload artifact'){
            steps{
                 
                sh 'curl -uadmin:AP2y1UiRRbNYAGwPGsz56ztnu46 -T \
                target/bio*.jar \
                "http://3.89.61.195:8081/artifactory/geoapp/"'
            }
        }
        stage('image build'){
            steps{
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 751794912847.dkr.ecr.us-east-1.amazonaws.com'
                sh 'docker build -t geoapp .'
            }
        }
        stage('push image'){
            steps{
                sh 'docker tag geoapp:latest 751794912847.dkr.ecr.us-east-1.amazonaws.com/geoapp:latest'
                sh 'docker push 751794912847.dkr.ecr.us-east-1.amazonaws.com/geoapp:latest'
            }
        }
        
    }
}