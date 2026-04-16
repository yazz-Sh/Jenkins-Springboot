pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "shiliyasmine/jenkins-springboot:latest"
        KUBE_NAMESPACE = "devops"
        SONAR_HOST = "http://192.168.56.10:9000"
    }

    stages {

        stage('GIT') {
            steps {
                echo "Getting Project from Git"
                git branch: 'main',
                    url: 'https://github.com/yazz-Sh/Jenkins-Springboot.git'
            }
        }

<<<<<<< HEAD

        stage('MVN BUILD') {
=======
	stage('MVN BUILD') {
>>>>>>> 1400a6d (Add Maven build stage before SonarQube)
            steps {
                sh 'mvn clean compile'
            }
        }

<<<<<<< HEAD
        
=======

>>>>>>> 1400a6d (Add Maven build stage before SonarQube)
        stage('MVN SONARQUBE') {
            steps {
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                    sh """
                        mvn sonar:sonar \
                        -Dsonar.host.url=$SONAR_HOST \
                        -Dsonar.login=$SONAR_TOKEN
                    """
                }
            }
        }

        stage('DOCKER BUILD') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('DOCKER PUSH') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push $DOCKER_IMAGE
                        docker logout
                    '''
                }
            }
        }

        stage('K8S DEPLOY') {
            steps {
                sh '''
                    kubectl apply -f k8s/
                    kubectl rollout restart deployment/spring-app-deployment -n $KUBE_NAMESPACE
                    kubectl get pods -n $KUBE_NAMESPACE
                    kubectl get svc -n $KUBE_NAMESPACE
                    kubectl get deployment spring-app-deployment -n $KUBE_NAMESPACE -o jsonpath='{.spec.template.spec.containers[0].image}'
                    echo
                '''
            }
        }
    }

    post {
        success {
            echo "Pipeline réussi !"
            echo "Résultats : http://192.168.56.10:9000"
        }
        failure {
            echo "Pipeline échoué."
        }
    }
}
