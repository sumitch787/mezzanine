pipeline {
  agent none
  stages {
    stage('Linting') {
      steps {
        dockerNode(image: 'ubuntu:22.04') {
          sh 'echo "Hello From Docker Inside"'
        }

      }
    }

  }
}