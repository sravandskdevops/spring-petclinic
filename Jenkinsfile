pipeline {
    agent any

    environment {
        APP_DIR = "${env.WORKSPACE}/spring-petclinic"
    }

    stages {

        stage('Checkout') {
            steps {
                echo "📥 Cloning Spring PetClinic into workspace..."
                sh '''
                rm -rf "$APP_DIR"
                git clone https://github.com/spring-projects/spring-petclinic.git "$APP_DIR"
                '''
            }
        }

        stage('Build') {
            steps {
                echo "🏗️ Building with Maven..."
                sh '''
                cd "$APP_DIR"
                mvn clean package -DskipTests
                '''
            }
        }

        stage('Stop Previous Instance') {
            steps {
                echo "🛑 Stopping any process on port 9090..."
                sh '''
                PID=$(lsof -ti:9090 || true)
                if [ -n "$PID" ]; then
                    echo "Killing process $PID"
                    kill -9 $PID || true
                fi
                '''
            }
        }

        stage('Run Application') {
            steps {
                echo "🚀 Running Spring PetClinic on port 9090..."
                sh '''
                cd "$APP_DIR"
                nohup java -jar target/*.jar --server.port=9090 > "$APP_DIR/app.log" 2>&1 &
                echo $! > "$APP_DIR/pid.txt"
                echo "Application started, PID=$(cat $APP_DIR/pid.txt)"
                '''
            }
        }
    }
}
