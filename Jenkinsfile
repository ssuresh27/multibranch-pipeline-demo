pipeline {
    agent { 
        node {
            label 'ubuntu'
            }
      }
    stages {
        stage(" execute Ansible") {
           steps {
                echo 'Pulling...' + env.BRANCH_NAME
                echo 'Pulling..._'+env.BRANCH_NAME+'.yml'
            }    
        }    
    }
}