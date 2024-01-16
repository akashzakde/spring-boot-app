pipeline{
    agent any
    tools{
        maven 'MVN'
        jdk 'JDK17'
        git 'git'
    }
    environment {
        DOCKER_LOGIN_NAME = 'akashz'
        DOCKER_PASSWORD = credentials('docker_token')
        IMAGE_NAME = 'myapp'
        IMAGE_TAG = "${BUILD_NUMBER}"
        JADMIN_TOKEN = credentials('Jenkins_admin_token')
    }
    stages{
        stage('Pull Code'){
            steps{
            // Clean before build
            cleanWs()
            git branch: 'main', url: 'https://github.com/akashzakde/spring-boot-app.git'
            }
        }
         stage('Code Analysis'){
            steps{
                withSonarQubeEnv(credentialsId: 'sonarqube-creds',installationName: 'sonarqube') {
                sh '''mvn clean verify sonar:sonar \
                      -Dsonar.projectKey=gitops-project \
                      -Dsonar.projectName='gitops-project' '''
                    }
                }
            }
        stage('QualityGate Result'){
            steps {
                    timeout(time: 1, unit: 'HOURS'){
                    waitForQualityGate abortPipeline: true
                    }
                }
            }
        stage('Build Code'){
            steps{
                sh 'mvn clean -DskipTests package'
            }
        }
        stage('Test Code'){
            steps{
                sh 'mvn test'
            }
        }
        stage('Docker Image Build'){
            steps{
                sh '''
                    docker build -t $DOCKER_LOGIN_NAME/$IMAGE_NAME .
                    docker tag $DOCKER_LOGIN_NAME/$IMAGE_NAME $DOCKER_LOGIN_NAME/$IMAGE_NAME:V$IMAGE_TAG
                    '''
            }
        }
        stage('Push Image'){
            steps{
                sh '''
                    docker login -u $DOCKER_LOGIN_NAME -p $DOCKER_PASSWORD
                    docker push $DOCKER_LOGIN_NAME/$IMAGE_NAME:latest
                    docker push $DOCKER_LOGIN_NAME/$IMAGE_NAME:V$IMAGE_TAG
                    '''
            }
        }
        stage('Clean Images'){
            steps{
                sh '''
                docker rmi $DOCKER_LOGIN_NAME/$IMAGE_NAME:latest
                docker rmi $DOCKER_LOGIN_NAME/$IMAGE_NAME:V$IMAGE_TAG
                '''
            }
        }
        stage('Trigger CD Pipeline'){
            steps{
                sh "curl -v -k --user admin:$JADMIN_TOKEN -X POST -H 'cache-control: no-cache' -H 'content_type: application/x-www-form-urlencoded' --data 'IMAGE_TAG=${IMAGE_TAG}' 'http://172.31.43.75:8080/job/GitOps-CD-Pipeline/buildWithParameters?token=GitOps-CD-Token'"
            }
        }
    }
}
