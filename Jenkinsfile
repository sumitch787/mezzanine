pipeline {
  agent any
  stages {
    stage('Checks') {
      parallel {
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
          steps {
            sh 'tox -e lint'
            milestone(ordinal: 1, label: 'lint')
          }
        }

        stage('Unit-Tests') {
          steps {
            sh 'tox -e py310-dj40'
            milestone(ordinal: 2, label: 'Tests')
          }
        }

        stage('SAST') {
          steps {
            sh 'mkdir bandit-report '
            sh 'bandit -r build/lib/ -f txt -o ./bandit-report/report.txt --exit-zero'
            sh 'cat ./bandit-report/report.txt '
            milestone(ordinal: 3, label: 'SAST')
          }
        }

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