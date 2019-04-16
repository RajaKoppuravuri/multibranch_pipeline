pipeline
{
    agent
    {
        label 'Linux_redhat'
    }
    environment
    {
        Name = "Gopi"
    }
  //  triggers
  //  {
  //      cron('*/1 * * * *')
  //  }
 parameters 
 {
    choice choices: ['Dev', 'QA', 'Non-prod', 'prod'], description: 'Please select your environment', name: 'Environment'
    }   
    
    options
    {
        buildDiscarder (logRotator(numToKeepStr:'5'))
        retry(3)    
    }
    stages
    {
        stage ("stage 1")
        {
            agent
            {
                label 'Linux'
            }
            environment
            {
                Name = "Raja"
            }
            steps
            {
                sh '''
                    echo "Hello $Name from satge 1"
                   ''' 

            }
        }

        stage ("stage 2")
        {
           input
           {
               message "Enter your name"
               
               parameters
               {
                   string(name: 'PERSON', defaultValue: '', description: 'Enter your name')
               }
           }

           when
           {
               beforeInput false
               environment name: 'PERSON', value: 'Divya'
               
           }
            steps
                {
                    sh '''
                        echo "Hello $Name from stage 2"
                        echo "Hello $PERSON"
                       '''
                }

        }
        stage ("stage checkout")
        {
            parallel
            {
                stage ("stage_1 in stages")
                {
                    steps
                    {
                        sh '''
                            echo "stage_1 in stages"
                          '''
                        dir('parents') 
                        {
                            git branch:'feature', url:'git@192.168.3.41:/home/git/gitrepo/stormpath-spring-boot-jpa-example.git', credentialsId:'git'
                        }
                    }
                }
            
                stage ("stage_2 in stages")
                {

                    steps
                    {
                        sh '''
                            echo "stage_2 in stages"
                          '''                        
                        
                        dir('students') 
                        {
                            git branch:'feature', url:'git@192.168.3.41:/home/git/gitrepo/stormpath-spring-boot-jpa-example.git', credentialsId:'git'
                        }
                    }
                }
            
            }
        }
    }
}
