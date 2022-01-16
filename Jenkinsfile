pipeline {
  agent any
  stages {
    stage('Prep') {
      agent {
        docker {
          image 'python-build:v1'
        }

      }
      steps {
        sh 'python3 setup.py sdist'
      }
    }

    stage('Lint') {
      agent {
        docker {
          image 'python-build:v1'
        }

      }
      steps {
        sh 'tox -e lint'
      }
    }

    stage('Build') {
      agent {
        docker {
          image 'python-build:v1'
        }

      }
      steps {
        sh '''python3 setup.py build
'''
      }
    }

    stage('Test') {
      parallel {
        stage('Unit') {
          agent {
            docker {
              image 'python-build:v1'
            }

          }
          steps {
            sh 'tox -e py310-dj40'
          }
        }

        stage('SAST') {
          agent {
            docker {
              image 'python-build:v1'
            }

          }
          steps {
            sh 'mkdir bandit-report '
            sh '''
bandit -r build/lib/ -f txt -o ./bandit-report/report.txt --exit-zero
'''
            sh 'cat ./bandit-report/report.txt '
          }
        }

      }
    }

    stage('Report') {
      agent {
        docker {
          image 'python-build:v1'
        }

      }
      steps {
        junit 'junit/*.xml'
      }
    }

    stage('Docker') {
      agent {
        docker {
          image 'ubuntu:22.04'
        }

      }
      steps {
        sh 'echo "Hello"'
      }
    }

  }
}