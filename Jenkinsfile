// Note. Build result must be set manually in scripted pipelines :-(
// SUCCESS, FAILURE, UNSTABLE
        @Library('jenkins-pipeline-shared-library-template') _
        
        node {
          def slackResponse
          wrap([$class: 'BuildUser']) {
            try {
              stage('SCM') {
                checkout scm
                echo sh(script: 'env|sort', returnStdout: true)
              }
              stage('Init') {
                echo sh(script: 'env|sort', returnStdout: true)
                slackResponse = slackPublish(currentBuild)
                sh 'echo "Init..."'
              }
              stage('Build') {
                sh 'echo "Building..."; sleep 5'
                echo currentBuild.result
                slackPublish(currentBuild, slackResponse?.threadId, "Build stage complete")
              }
              stage('Deploy') {
                sh 'echo "Deploying..."; sleep 3; exit 0'
              }
              currentBuild.result = 'SUCCESS'
            } catch (ex) {
              echo "catch"
              currentBuild.result = 'FAILURE'
              echo currentBuild.result
              throw ex
            } finally {
              echo "finally"
              echo currentBuild.result
              slackPublish(currentBuild,slackResponse?.threadId)
            }
          }
        }
