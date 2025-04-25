pipeline{
    agent any

    environment {
        VENV_DIR = 'venv'
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
                        source $VENV_DIR/bin/activate
                        pip install --upgrade pip
                        pip install -e .
                    '''
                }
            }
        }
    }
}