node(){
    stages {
        stage("git checkout") {
            steps {
            url: "https://github.com/i-m-mohit/foodbox_backend-main.git"
        }
        }
        stage("clean") {
            steps {
                echo 'maven clean'
                sh 'mvn clean'
            }
        }

        stage("install") {
            steps {
                echo 'maven install'
                sh 'mvn install -DskipTests'
            }
            post{
                    success{
                    archiveArtifacts artifacts: 'target/foodbox.war'
                }
            }
        }
    }
       node('awsnode') {
           echo 'Unstash'
           unstash 'buildArtifacts'
           echo 'Artifacts copied'
           sh '/opt/apache-tomcat-9.0.64/bin/shutdown.sh'
           echo 'Copy'
           sh 'yes | sudo cp -R foodbox.war /opt/apache-tomcat-9.0.64/webapps/ROOT'
           sh '/opt/apache-tomcat-9.0.64/bin/startup.sh'
           echo 'Copy completed'
       }
    }

