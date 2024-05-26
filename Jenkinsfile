pipeline {
    agent { 
        node {
            label 'ubuntu'
            }
      }
    triggers {
        githubPush()
    }
    stages {
        stage("Print ENV") {
           steps {
                echo 'Pulling...' + env.BRANCH_NAME
                echo 'Pulling..._'+env.BRANCH_NAME+'.yml'
                sh 'printenv'
        } 
        }  
        stage("Execute Ansible Play") {
            steps {
                 ansiblePlaybook colorized: true, credentialsId: 'jenkins-agent', disableHostKeyChecking: true, installation: 'Ansible', inventory: 'inventory_'+env.BRANCH_NAME, playbook: 'tranfer_file_'+env.BRANCH_NAME+'.yml', vaultTmpPath: ''
            }      
        }
        stage('Publish artefacts') {
            steps {
            sh """
            git config --local user.name '${GIT_COMMITTER_NAME}'
            git config --local user.email '${GIT_AUTHOR_EMAIL}'
            touch testfile
            git add testfile
            git commit -m 'Add testfile from Jenkins Pipeline'
            git push origin HEAD:${env.BRANCH_NAME}
            """  
            }
        }
    }
}