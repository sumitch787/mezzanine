pipeline {
  agent {
    docker {
      image 'python-build:v1'
    }

  }
  stages {
    stage('Prep') {
      steps {
        sh 'python3 setup.py sdist'
      }
    }

    stage('Lint') {
      steps {
        sh 'tox -e lint'
      }
    }

    stage('Build') {
      steps {
        sh '''python3 setup.py install
python3 setup.py build
'''
      }
    }

    stage('Test') {
      parallel {
        stage('Unit') {
          steps {
            sh 'tox -e package -e lint -e pyupgrade'
          }
        }

        stage('SAST') {
          steps {
            sh 'bandit '
          }
        }

      }
    }

    stage('Publish report') {
      steps {
        junit 'junit/*.xml'
        sh 'ls -al'
      }
    }

  }
}