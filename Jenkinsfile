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
        sh '''python3 setup.py build
'''
      }
    }

    stage('Test') {
      parallel {
        stage('Unit') {
          steps {
            sh 'tox -e py310-dj40'
          }
        }

        stage('SAST') {
          steps {
            sh '''mkdir bandit-report 
bandit -r build/lib/ -f ./bandit-report/report.txt
cat ./bandit-report/bandit-report.txt '''
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