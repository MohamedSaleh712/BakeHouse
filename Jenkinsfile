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
                script { // without script scoopt it will crash 
                    withCredentials([usernamePassword(credentialsId:'dockerhub-secret',usernameVariable:'DOCKER_USERNAME',passwordVariable:'DOCKER_PASSWORD')])
                    sh '''
                        docker build -t 712199425/lab2ITI:v1 .
                        docker login -u ${DOCKER_USERNAME} -p {DOCKER_PASSWORD}
                        docker push
                        echo ${BUILD_NUMBER}
                    '''
                    }
                }
        }
        stage('deploy') {
            steps {
                sh "echo 'Hello World'"
            }
        }
    }
}
