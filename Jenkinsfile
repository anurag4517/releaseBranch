

pipeline{
    agent {
        label 'main'
    }
    environment {
        
        GITHUB_API_TOKEN = credentials('GITHUB_API_TOKEN')
        auth = credentials('githubaccesspat')
        
    }
    parameters
    {
        string(description: 'Specify github username for who needs to create release branch', name: 'username', defaultValue: 'htiwari1987') 
        string(description: 'Specify name of organization in which repo recides', name: 'organization', defaultValue: 'salesforcedocs')
        string(description: 'Name of repo to create release branch ', name: 'REPO_NAME',defaultValue: 'anurag-test-repo')
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
       
        stage("Check if user is maintainer for repo")
        {
           steps {
            script 
            {
                isMaintiner()
                
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
def createReleaseBranch()
{
    cloneRepo()
    checkIfBranchExists()
}

def cloneRepo()
{
    boolean status=checkIfBranchExists() 
    if(!status)
    {
    sh(script: """ rm -rf ${REPO_NAME};git clone --single-branch https://git:${GITHUB_API_TOKEN}@github.com/salesforcedocs/${REPO_NAME}.git ${REPO_NAME}; cd ${REPO_NAME} ; git checkout -b ${release_name};echo \"Creating a new release ${release_name}\">> README.md ; git add .;git commit -m \"Creating a new release ${release_name}\";git push --set-upstream origin ${release_name}

      """, returnStdout: true).trim()
    }else { echo 'Branch already exists '}
}

def checkIfBranchExists()
{
    try {
    sh(script: """ rm -rf ${REPO_NAME};git clone https://git:${GITHUB_API_TOKEN}@github.com/salesforcedocs/${REPO_NAME}.git --branch ${release_name} ${REPO_NAME} ; """, returnStdout: true).trim()
    return true;
    }catch(Exception ex) {
        return false

    }
}
def hitGetApi(String urlasked)
{
      
      def(String response , int code) = sh(script: "curl ${urlasked}  -H \"${accept}\" -H \"${auth}\" -o /dev/null -w \"%{http_code}\"", returnStdout: true).trim().tokenize("\n")
      echo "HTTP response response : ${response}"
      return response
      

}

def isMaintiner()
{
    String team_slug= "ccx-" + params.REPO_NAME
    flag=checkRoleOfUser(team_slug)
    if(flag ==0) 
    {
        error( " User is not a maintainer of repo so cannot create a Branch . Kindly connect with #SFDocs and get added to ${team_slug} ")
    }
}


def checkRoleOfUser(String team_slug)
{
   
    String url = "${baseUrl}/orgs/salesforcedocs/teams/${team_slug}/memberships/${params.username}"
    userRole = returnRole(url)
    if(userRole!='NA')
    {
    int flag =0 ;
    for(String myrole : userRole)
    {
        echo myrole
        if(myrole == 'maintainer') {
            echo 'Success------> Maintainer  '
            
            flag=1 ; return flag;
        }else { 
            echo myrole 
            echo params.userrole
            echo '---Looping----- '
            
        }
    }
    return flag 

    }
    else { return 0 }
    
}

def returnRole(String urlasked)
{
    check = hitGetApi(urlasked)
    if(check!="404"){
        echo 'Previous role is assigned returning role '
    def role = sh(script: "curl ${urlasked}  -H \"${accept}\" -H \"${auth}\" | python -c \"import sys,json; print json.load(sys.stdin)['role']\" ", returnStdout: true).trim().tokenize("\n")
    return role
    }else { 
        echo 'No previous role was assigned here---> Need to assign a role  '
    return 'NA'
    }
   

}




