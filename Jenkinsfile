// Note. Build result must be set manually in scripted pipelines :-(
// SUCCESS, FAILURE, UNSTABLE
        @Library('jenkins-pipeline-shared-library-template') _
        
        node {
                // Test PRs                
                echo "${currentBuild.getRawBuild().getCauses().size()}"
                for(hudson.model.Cause cause : currentBuild.getRawBuild().getCauses()){
                    echo "Class ${cause.class}"
                    if(cause.class == hudson.model.Cause$UserIdCause){
                        echo "${cause.getUserId()}"
                    }
                }
                                
          def slackResponse
          wrap([$class: 'BuildUser']) {
            try {
              stage('SCM') {
                scmVars = checkout scm
                echo "scmVars: ${scmVars}"
                echo "scmVars.GIT_COMMIT: ${scmVars.GIT_COMMIT}"
                echo "scmVars.GIT_BRANCH: ${scmVars.GIT_BRANCH}"
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
