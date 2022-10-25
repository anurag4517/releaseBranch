

pipeline{
    agent {
        label 'main'
    }
    environment {
        
        auth = credentials('githubaccesspat')
        
    }
    parameters
    {
        string(description: 'Specify github username for who needs to create release branch', name: 'username', defaultValue: 'htiwari1987') 
        string(description: 'Specify name of organization in which repo recides', name: 'organization', defaultValue: 'salesforcedocs')
        string(description: 'Name of repo to create release branch ', name: 'reponame',defaultValue: 'sfdocs-training')
        string(description: 'Specify the release branch name', name: 'releasebranch',defaultValue: '236')
        booleanParam(description: 'Force create branch ', name: 'forcecreate', defaultValue: false)
    }
        
        
    stages{
        stage('Initialize the variables') {
            // Each stage is made up of steps
            steps{
                script{
                     baseUrl = "https://api.github.com"
                     accept = "Accept: application/vnd.github+json"
                   }
            }                
        }
        stage("Check if Organization & repo exists")
        {
           steps {
            script 
            {
                validate()
                
            }
           } 
        }
        stage("Check if user is maintainer for repo")
        {
           steps {
            script 
            {
                validate()
                
            }
           } 
        }
        stage("Create a release branch ")
        {
           steps {
            script 
            {
                createReleaseBranch()
                checkIfBranchExists()
                
            }
           } 
        }

        

        
        
        
         
        }   
}




