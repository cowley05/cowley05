pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        echo 'hello ryan'
      }
    }
    stage('test') {
      steps {
        echo 'tests all passed'
      }
    }
    stage('deploy Test') {
      parallel {
        stage('deploy Test') {
          steps {
            echo 'test deployed succesfully'
          }
        }
        stage('') {
          steps {
            sh 'exit 1'
          }
        }
      }
    }
    stage('hmmm') {
      steps {
        echo 'blah'
      }
    }
  }
}
