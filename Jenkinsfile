pipeline {
    agent any

    environment {
        VENV = "venv"
        STREAMLIT_PORT = "8501"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/<your-username>/streamlit-calculator.git'
            }
        }

        stage('Setup Python Environment') {
            steps {
                sh '''
                python3 -m venv $VENV
                . $VENV/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Deploy Streamlit App') {
            steps {
                sh '''
                . $VENV/bin/activate
                pkill -f streamlit || true
                nohup streamlit run app.py \
                  --server.port $STREAMLIT_PORT \
                  --server.headless true \
                  > streamlit.log 2>&1 &
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Streamlit app deployed successfully"
        }
        failure {
            echo "❌ Deployment failed"
        }
    }
}
