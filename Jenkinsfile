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
                // Read the console log
                def consoleLog = Jenkins.getInstance().getItemByFullName(env.JOB_NAME).getBuildByNumber(Integer.parseInt(env.BUILD_NUMBER)).logFile.text
                //Write the log to a file
                writeFile(file: "Log_${BUILD_NUMBER}.txt", text: consoleLog, encoding: "UTF-8")
                sh'''
                git add *
                git commit -m "Add console log"
                git push
                '''
            }    
        }    
        stage("Execute Ansible Play") {
            steps {
                 ansiblePlaybook colorized: true, credentialsId: 'jenkins-agent', disableHostKeyChecking: true, installation: 'Ansible', inventory: 'inventory_'+env.BRANCH_NAME, playbook: 'tranfer_file_'+env.BRANCH_NAME+'.yml', vaultTmpPath: ''
            }      
        }    
    }
}