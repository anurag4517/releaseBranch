

pipeline{
    agent {
        label 'main'
    }
    environment {
        
        GITHUB_API_TOKEN = credentials('GITHUB_API_TOKEN')
        
    }
    parameters
    {
        string(description: 'Specify github username for who needs to create release branch', name: 'username', defaultValue: 'htiwari1987') 
        string(description: 'Specify name of organization in which repo recides', name: 'organization', defaultValue: 'salesforcedocs')
        string(description: 'Name of repo to create release branch ', name: 'REPO_NAME',defaultValue: 'sfdocs-training')
        string(description: 'Specify the release branch name', name: 'release_name',defaultValue: 'rel/230')
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
       
        // stage("Check if user is maintainer for repo")
        // {
        //    steps {
        //     script 
        //     {
        //         validate()
                
        //     }
        //    } 
        // }
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
def createReleaseBranch()
{
    cloneRepo()
    checkIfBranchExists()
}

def cloneRepo()
{
    sh(script: """ rm -rf ${REPO_NAME};git clone --single-branch https://git:${GITHUB_API_TOKEN}@github.com/salesforcedocs/${REPO_NAME}.git ${REPO_NAME}; cd ${REPO_NAME} ; git checkout -b ${release_name};echo \"Creating a new release ${release_name}\">> README.md ; git add .;git commit -m \"Creating a new release ${release_name}\";git push --set-upstream origin ${release_name}

      """, returnStdout: true).trim()
}

def checkIfBranchExists()
{

}




