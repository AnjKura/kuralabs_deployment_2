pipeline {
  agent any
   stages {
    stage ('Build') {
      steps {
        sh '''#!/bin/bash
        python3 -m venv test3
        source test3/bin/activate
        pip install pip --upgrade
        pip install -r requirements.txt
        export FLASK_APP=application
        flask run &
        '''
     }
   }
    stage ('test') {
      steps {
        sh '''#!/bin/bash
        source test3/bin/activate
        py.test --verbose --junit-xml test-reports/results.xml
        ''' 
      }
    
      post{
        always {
          junit 'test-reports/results.xml'
        }
       
      }
    }
   stage ('Deploy') {
     steps {
       sh '/var/lib/jenkins/.local/bin/eb deploy url-shortener-dev'
       git clone https://github.com/AnjKura/kuralabs_deployment_2
       cd ./kuralabs_deployment_2
       python3 -m venv test3
       source test3/bin/activate
       pip install -r requirements.txt
       pip install gunicorn
       JENKINS_NODE_COOKIE=stayAlive
       gunicorn -w 4 application:app -b 0.0.0.0:8080 --daemon
       '''
      }
    }
  }
}
