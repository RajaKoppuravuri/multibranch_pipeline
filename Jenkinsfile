pipeline{

  agent any


    environment {
      Name = "Devops"
      }

      parameters {
    string defaultValue: 'DevOPS', description: '', name: 'Student', trim: false
  }

    tools {
        jdk 'jdk8'
        maven 'mvn3.6.3'
    }

    stages
    {
      stage('checkout')
      {
        steps
        {
         git branch: 'master', credentialsId: 'GitCredentials', url: 'https://github.com/RajaKoppuravuri/stormpath-spring-boot-jpa-example.git'
        }
      }

     stage('Code compile') {
       steps
       {
          sh 'mvn clean package'
       }
    }
    stage('Pritnt environment variables') {
      steps
      {
         sh 'printenv'
      }
   }
    stage('Archive artifacts'){
     steps
     {
      archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
     }

    }
    stage('Execute shell script'){
    agent {
  label 'amazon_linux'
  }

    when {
      environment name: 'Student', value: 'DevOPS'
    }
    steps
    {
     sh '''
          echo "This is a demo pipeline job."
          echo "This is working as $Name."
          echo "This is executed by  the $Student student"

        '''
    }
    }

    stage('parallel execution') {

      parallel {
        stage('stage_1_execution') {
          steps {
          sh '''
               echo "This is a demo pipeline job."
               echo "This is working as expected."
               echo "This is from stage_1_execution"

             '''
            }
          }
          stage('stage_2_execution') {
            steps {
            sh '''
                 echo "This is a demo pipeline job."
                 echo "This is working as expected."
                 echo "This is from stage_2_execution"

               '''
              }
              }
      }
    }

   }
   post {
     always {
       cleanWs()
     }
   }

}
