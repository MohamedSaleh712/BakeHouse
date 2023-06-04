pipeline { //لازم يبدأ بـكلمة دي
    agent any // هترن كل الخطوات الجاية في الحتة الفلانية
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
                sh '''
                    echo ${BUILD_NUMBER}
                '''
            }
        }
        stage('deploy') {
            steps {
                sh "echo 'Hello World'"
            }
        }
    }
}
