pipeline {
    agent any

    stages {
        stage('Clone Spring PetClinic') {
            steps {
                echo "Cloning Spring PetClinic..."
                sh '''
                rm -rf /opt/spring-petclinic || true
                cd /opt
                git clone https://github.com/spring-projects/spring-petclinic.git
                '''
            }
        }

        stage('Build with Maven') {
            steps {
                echo "Building with Maven..."
                sh '''
                cd /opt/spring-petclinic
                mvn clean package -DskipTests
                '''
            }
        }

        stage('Stop previous app (if any)') {
            steps {
                echo "Stopping any process on port 9090..."
                sh '''
                # kill process listening on 9090 (if any)
                PID=$(lsof -ti:9090 || true)
                if [ -n "$PID" ]; then
                  echo "Killing PID $PID"
                  kill -9 $PID || true
                fi
                '''
            }
        }

        stage('Run Application') {
            steps {
                echo "Starting Spring PetClinic on port 9090..."
                sh '''
                cd /opt/spring-petclinic
                nohup java -jar target/*.jar --server.port=9090 > /opt/spring-petclinic/app.log 2>&1 &
                echo $! > /opt/spring-petclinic/pid.txt
                echo "Started, PID=$(cat /opt/spring-petclinic/pid.txt)"
                '''
            }
        }
    }
}
