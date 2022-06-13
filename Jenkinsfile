pipeline{
    agent any
    stages {
        stage("git checkout") {
            steps {
            git "https://github.com/i-m-mohit/foodbox_backend-main.git"
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
        
         stage('Stash changes') {
            steps {
        stash allowEmpty: true, includes: 'target/foodbox.war', name: 'buildArtifacts'
        }
         }
     
    }
}
      node('awsnode') {
          echo 'Unstash'
           unstash 'buildArtifacts'
          echo 'Artifacts copied'
        //   sh 'sudo /opt/apache-tomcat-9.0.64/bin/shutdown.sh'
          echo 'Copy'
          sh 'yes | sudo cp -R target/foodbox.war /home/ec2-user/project'
          
           sh 'sudo nohup java -jar /home/ec2-user/project/foodbox.war > log.log 2>&1 &'
          echo 'Copy completed'
      }
