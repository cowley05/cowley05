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
    stage('approval') {
      steps {
        timeout(time: 120, unit: 'SECONDS') {
          input 'Approve deployment?'
        }

      }
    }
    stage('deploy Test') {
      steps {
        echo 'test deployed succesfully'
      }
    }
    stage('hmmm') {
      steps {
        echo 'blah'
      }
    }
  }
}