node {
    def app

    stage('Clone repository') {
      

        checkout scm
    }

    stage('Update GIT') {
            script {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        //def encodedPassword = URLEncoder.encode("$GIT_PASSWORD",'UTF-8')
                        sh "git config user.email shivak981@gmail.com"
                        sh "git config user.name spasham"
                        //sh "git switch master"
                        sh "git checkout main"
                        sh "git pull https://github.com/spasham/kubemanifest.git --allow-unrelated-histories"
                        sh "cat deployment.yaml"
                        sh "sed -i 's+shivak981/meters2feet.*+shivak981/meters2feet:${DOCKERTAG}+g' deployment.yaml"
                        sh "cat deployment.yaml"
                        sh "git merge --strategy-option ours FETCH_HEAD || true"
                        sh "git add ."
                        sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                        sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/kubernetesmanifest.git HEAD:main --force"
      }
    }
  }
}
}
