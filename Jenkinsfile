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
                    //sh "nohup timeout 10s java -jar target/simple-parcel-service-app-1.0-SNAPSHOT.jar > output.log 2>&1 &"
                    // Sleep for a while to allow the application to start
                    sleep 3
                }
            }
        }
        
        stage('deploy') {
            steps {
                sh 'ssh root@172.31.15.207'
                sh "curl -v --user sanj:sanj --upload-file /home/slave1/workspace/deployingtomcat/target/firstSpringBootApplication-0.0.1-SNAPSHOT.war "http://13.233.9.29:8081/manager/text/deploy?path=/firstSpringBootApplication-0.0.1-SNAPSHOT"
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
