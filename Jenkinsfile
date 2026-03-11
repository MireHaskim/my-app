pipeline {
    agent any
    environment {
        PROJECT_ID = 'My First Project'
        REGION = 'us-central1'
        CLUSTER = 'my-learning-cluster'
        REPO = 'us-central1-docker.pkg.dev/project-f66ece82-84d1-4937-b20/my-learning-repo'
        APP_NAME = 'my-app'

        COMMIT_HASH = sh(script: "git rev-parse --short HEAD", returnStdout: true).trim()
        FULL_IMAGE = "${REPO}/${APP_NAME}:${COMMIT_HASH}"

    }

    stages {
        stage('build') {    
            steps {
                sh "docker build -t ${FUll_IMAGE} ."
            }
        }
        stage('push to artificat registry') {
            steps {
                sh "gcloud auth configure-docker us-central1-docker.pkg.dev --quiet"
                sh "docker push ${FULL_IMAGE}"
            }
        }
        stage('deploy') {
            steps {
               sh "gcloud container clusters get-credentials ${CLUSTER} --region ${REGION} --project ${PROJECT_ID}"
               sh "kubectl set image deployment/my-app my-app=${FULL_IMAGE}"
            }

        }
    }
           
}