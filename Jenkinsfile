#!groovy

node  {
    try {
        def dockerfile = 'Dockerfile'
        // def repo_name = 'node-jenkins-demo'
        def imageName = 'mostuf556/node-jenkins-demo'
        def docker_tag = "1.0.${env.BUILD_ID}"
        def err = false
        def testImage 


        stage('init') {
            checkout scm
        }

        stage('Build Docker Image and run test') {
            testImage = docker.build("${imageName}",  "-f ${dockerfile} .")
            testImage.inside {
                sh 'npm install'
                sh 'npm test'
            }
        }

//         stage('Push Docker Image') {
//             if (env.BRANCH_NAME == 'master') {
//                 echo 'I only execute on the master branch'

//                 try {
//                     withCredentials([usernamePassword(credentialsId: 'docker_hub_access_token', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
//                         sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin' }

//                     sh "docker push ${imageName}:${docker_tag}"
//                     sh "docker push ${imageName}:latest"

//                 } catch (e) {
//                     throw e
//                 } finally {
//                     sh 'docker logout'
//                 }
//             } else {
//                 echo 'skip docker push for non-master branch'
//             }
//         }

        stage('Push image 2') {
            docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_access_token') {
                testImage.push("${env.BUILD_NUMBER}")
                testImage.push("latest")
            }
        }

    } catch(Exception error) {
        // err = true
        echo "caught error ${error}"
    } finally {
        sh 'docker logout'
    }


    // if (!err) {
    //     echo 'build and deploy ran successfully'
    // }
    echo "currentBuild.currentResult: ${currentBuild.currentResult}"

}
