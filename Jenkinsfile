pipeline {
    agent any
    options {
        checkoutToSubdirectory('PYHANDLE')
    }
    environment {
        PROJECT_DIR="PYHANDLE"
        GH_USER = 'newgrnetci'
        GH_EMAIL = '<argo@grnet.gr>'
        
    }
    stages {
        
       stage ('Deploy Docs') {
           // when {
           //     changeset 'docs/source/**'
           // }   
            agent {
                docker {
                    image 'eudat-pyhandle:py3.10'
                
                }
            }
            steps {
                echo 'Build Sphinx docs...'
                sh '''
                    cd $WORKSPACE/$PROJECT_DIR
                    cd docs
                    make html
                '''
                echo 'Sending to gh-pages...'
                sshagent (credentials: ['jenkins-master']) {
                    sh '''
                        mkdir ~/.ssh && ssh-keyscan -H github.com > ~/.ssh/known_hosts
                        git config --global user.email ${GH_EMAIL}
                        git config --global user.name ${GH_USER}
                        GIT_USER=${GH_USER} USE_SSH=true 
                        cd $WORKSPACE/$PROJECT_DIR/docs/build/html
                        git checkout gh-pages
                        
                    '''
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
