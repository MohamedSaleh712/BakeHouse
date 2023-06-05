pipeline { //لازم يبدأ بـكلمة دي
    agent { label 'iti-smart-slave-paa' } 
    // pipeline اللي هترن عليه node دا اسم الlabel المحطوط على
                //any: اي حد فاضي اي حد متاح رن عليه السكريبت
    stages {
        stage('test') { // يكون بنفس الاسم عادي stage ينفع
            steps { // جوه الـstep بنكتب حاجتنا كلها
                echo 'Hello World' // this's echo belong to Groovy
                sh "ls" // if you want to write BASH script one line
            }
        }
        stage('build') {
            steps {
                echo 'Hello World' // if you want write BASH script in multi lines
                script {
                    // without script scoopt it will crash 
                    withCredentials([usernamePassword(credentialsId:'dockerhub-secret',usernameVariable:'DOCKER_USERNAME',passwordVariable:'DOCKER_PASSWORD')]){
                        sh '''
                            docker build -t 712199425/lab2iti:v${BUILD_NUMBER} .
                            docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}
                            docker push 712199425/lab2iti:v${BUILD_NUMBER}
                            echo ${BUILD_NUMBER}
                        '''
                    }
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                    sh "echo 'Hello World'"
                    withCredentials([file(credentialsId:'iti-smart-kubeconfig', variable: 'KUBECONFIG_ITI')]) {
                        sh '''
                            mv Deployment/deploy.yaml Deployment/deploy.yaml.tmp
                            cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml
                            rm -f Deployment/deploy.yaml.tmp
                            kubectl apply -f Deployment --kubeconfig ${KUBECONFIG_ITI}
                        '''
                    }
                }
            }
        }
    }
}
