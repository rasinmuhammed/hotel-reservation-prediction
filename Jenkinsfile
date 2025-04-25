pipeline{
    agent any

    environment {
        VENV_DIR = 'venv'
        GCP_PROJECT = 'vast-art-454104-f1'
        GCLOUD_PATH = '/var/jenkins_home/google-cloud-sdk/bin'
    }


    stages{
        stage("Cloning Github repo to Jenkins"){
            steps{
                script{
                    echo 'Cloning Github repo to Jenkins'
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github-token', url: 'https://github.com/rasinmuhammed/hotel-reservation-prediction.git']])
                }
            }
        }

        stage("Setting up Python Virtual Environment and Installing Dependencies"){
            steps{
                script{
                    echo 'Setting up Python Virtual Environment and Installing Dependencies'
                    sh '''
                        python -m venv $VENV_DIR
                        . $VENV_DIR/bin/activate
                        pip install --upgrade pip
                        pip install -e .
                    '''
                }
            }
        }

        stage("Building and pushing Docker Image to GCR"){
            steps{
                withCredentials([file(credentialsId: 'gcp-key', variable : 'GOOGLE_APPLICATION_CREDENTIALS')]){
                    script{
                        echo 'Building and pushing Docker Image to GCR'
                        sh '''
                            export PATH=$PATH:${GCLOUD_PATH}
                           
                            gcloud auth activate-service-account --key-file ${GOOGLE_APPLICATION_CREDENTIALS}
                            
                            gcloud config set project ${GCP_PROJECT}

                            gcloud auth configure-docker --quiet

                            docker build -t gcr.io/${GCP_PROJECT}/hotel-reservation-prediction:latest .

                            docker push gcr.io/${GCP_PROJECT}/hotel-reservation-prediction:latest
                        '''
                    }
                } 
            }
        }
    }
}