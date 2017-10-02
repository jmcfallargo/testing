node('master') {
   
   stage('init'){
    sh 'pwd'
    sh 'ls -la'
   }
  
    stage('source'){
        
         commitId = handleGitHubSource('abc123');   
        
    }
        
        
}


/////////////////////////////////////////////////////////////////////////////
// Purpose: Acquire source from Git via SCM object 
//              ( SCM only available in multi scm/pipeline jobs)
//
// args:
//          relativetargetdir: relative directory to check out into
//
// returns:
//          GIT_COMMIT
//////////////////////////////////////////////////////////////////////////////
def handleGitHubSource(relativetargetdir){
    println("Checkout out SCM multiscm into target: ${relativetargetdir}")

    try{

        def commitID = checkout ([
            $class: 'GitSCM',
            branches: scm.branches,
            extensions: [
                    [$class: 'RelativeTargetDirectory', relativeTargetDir: relativetargetdir],
                    [$class: 'PruneStaleBranch'],
                    //[$class: 'CleanCheckout'],
                    [$class: 'CloneOption', depth: 0, noTags: false, reference: '', shallow: true]
            ],
            userRemoteConfigs: scm.userRemoteConfigs
        ]).GIT_COMMIT

        return commitID;

    }catch(Exception ex){
        println("Exception acquiring source from Multi SCM source with exception ${ex}")
        throw ex
    }
}

def checkoutGitSource(url, branch, name, relativetargetdir, credentials){

    try{
        println("Checkout out source from: ${url} on branch ${branch} into target dir: ${relativetargetdir}")
        
        checkout([$class: 'GitSCM', branches: [[name: branch]], 
                    doGenerateSubmoduleConfigurations: false, 
                    extensions: [
                        [$class: 'RelativeTargetDirectory', relativeTargetDir: relativetargetdir], 
                        //[$class: 'WipeWorkspace'],
                        [$class: 'CloneOption', depth: 0, noTags: false, reference: '', shallow: true]
                    ], 
                    submoduleCfg: [], userRemoteConfigs: [[credentialsId: credentials, name: name, url: url]]])
    }catch(Exception ex){
        println("Exception acquiring source from repo: ${url} with exception: ${ex}")
        throw ex
    }
}
