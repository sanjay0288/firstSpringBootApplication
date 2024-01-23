pipeline {
    agent {
        label 'slave1'
    }

    stages {
        stage('checkout') {
            steps {
                sh 'rm -rf Parcel-service'
                sh 'git clone https://github.com/sanjay0288/Parcel-service.git'
            }
        }

        stage('build') {
            steps {
                script {
                    sh 'mvn --version'
                    sh 'mvn clean install'
                }
            }
        }
        
      //  stage("SonarQube analysis") {
    //     steps {
      //          withSonarQubeEnv('sonar') {
        //            sh 'mvn clean package sonar:sonar'
          //    }
          //  }
       // }

        stage('Show Contents of target') {
            steps {
                script {
                    // Print the contents of the target directory
                    sh 'ls -l target'
                }
            }
        }

        stage('Run JAR Locally') {
            steps {
                script {
                    // Run the JAR file using java -jar
                    sh "nohup timeout 10s java -jar target/simple-parcel-service-app-1.0-SNAPSHOT.jar > output.log 2>&1 &"
                    // Sleep for a while to allow the application to start
                    sleep 10
                }
            }
        }
        
        stage('deploy') {
            steps {
                sh 'ssh root@172.31.15.207'
                sh "scp /home/slave1/workspace/parcel_service_feature-2/target/simple-parcel-service-app-1.0-SNAPSHOT.jar root@172.31.15.207:/opt/apache-tomcat-8.5.98/webapps"
            }
        }
        
    }
        
    post {
        success {
            echo "Build, Run, and Deployment to Tomcat successful!"
        }
        failure {
            echo "Build, Run, and Deployment to Tomcat failed!"
        }
    }
}
