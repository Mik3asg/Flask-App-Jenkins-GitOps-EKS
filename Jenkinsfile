pipeline {
    agent any
    
    stages {
        stage('Clone Repository') {
            steps {
                checkout scm
            }
        }
        
        stage('Update K8s Manifest') {
            steps {
                script {
                    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                        withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                            sh "git config user.email mik3asg@gmail.com"
                            sh "git config user.name Mik3asg"
                            sh "cat deployment.yaml"
                            sh "sed -i 's+mik3asg/flask-cicd.*+mik3asg/flask-cicd:${DOCKERTAG}+g' deployment.yaml"
                            sh "cat deployment.yaml"
                            sh "git add deployment.yaml"
                            sh "git commit -m 'Updated deployment.yaml by Jenkins UpdateK8sManifestJob: ${env.BUILD_NUMBER}'"
                            sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/Flask_App_Jenkins_GitOps_EKS.git HEAD:main"
                        }
                    }
                }
            }
        }
    }
}
