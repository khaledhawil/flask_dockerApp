pipeline{
    agent {
        label 's1'
    }
    
        stages{
            stage('build '){
                steps{
                    sh "docker build -t maro4299311/flask_app:$BUILD_NUMBER ."
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
    sh "docker login -u $USER -p $PASS"
    sh "docker push maro4299311/flask_app:$BUILD_NUMBER"
}
                }
            }
            stage ('test'){
                steps{
                    echo "no test cases found"
                }
            }

            
            stage ('deploy'){
                steps{
                    withCredentials([file(credentialsId: 'k8s', variable: 'k8s')]) {
    sh "sed -i \"s|image: .*|image: maro4299311/flask_app:$BUILD_NUMBER|g\" deployment.yml"
    sh "kubectl --kubeconfig=$k8s apply -f deployment.yml "
}
                }
            }
        }
    
}
