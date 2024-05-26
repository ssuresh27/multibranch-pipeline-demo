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
            withCredentials([
            usernamePassword(credentialsId: JENKINS_GITHUB_CREDENTIALS_ID, usernameVariable: "GIT_USERNAME", passwordVariable: "GIT_PASSWORD")]) 
            {
            sh """
            git config --local credential.helper "!f() { echo username='${GIT_USERNAME}'; echo password='${GIT_PASSWORD}'; }; f"
            git config --local user.name '${GIT_USERNAME}'
            git config --local user.email '${GIT_USERNAME}@example.tld'
            make docs-publish
            """
        }
      }    
    }
    }
}