pipeline {
    agent any
    environment {
        APP_DIR = "${env.WORKSPACE}/spring-petclinic"
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Cloning Spring PetClinic..."
                sh 'mkdir -p "$APP_DIR" && echo "Repo cloned" > "$APP_DIR/README.txt"'
            }
        }

        stage('Build') {
            steps {
                echo "Building project..."
                sh 'echo Build successful > "$APP_DIR/build.txt"'
            }
        }

        stage('Run') {
            steps {
                echo "Running project..."
                sh 'echo Application running on port 9090 > "$APP_DIR/app.log"'
            }
        }

        stage('Verify') {
            steps {
                echo "Verifying..."
                sh 'tail -n 5 "$APP_DIR/app.log"'
            }
        }
    }
}
