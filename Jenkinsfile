pipeline {
    agent {label 'JenkinsSlave'}
    stages {
        stage('Create files') {
            steps {
                sh 'rm -rf `pwd`/Directory1'
                sh 'mkdir `pwd`/Directory1'
                sh 'ls `pwd`/Directory1'
                script
                {
                    for(int i=0;i<10;i++)
                        {
                                sh 'echo "some text" > `pwd`/Directory1/textFile'+i+'.txt'
                        }
//                        sh 'ls `pwd`/Directory1'
                         sh 'echo 1'
                        sh 'cat `pwd`/Directory1/textFile4.txt'
                }
            }
        }
        stage('Copy files and insert data') {
            steps {
                sh 'rm -rf `pwd`/Directory2'
                sh 'mkdir `pwd`/Directory2'
//                sh 'ls `pwd`/Directory2'
                script{
                    for(int i=0;i<10;i++)
                        {
                            sh 'cp `pwd`/Directory1/textFile'+i+'.txt `pwd`/Directory2/textFile'+i+'.txt'
                            sh 'date >> `pwd`/Directory2/textFile'+i+'.txt'
                        }
//                        sh 'ls `pwd`/Directory2'
                        sh 'echo 2'
                        sh 'cat `pwd`/Directory2/textFile4.txt'
                }
        }
    }
        stage('Copy files and make them read-only') {
                steps {
		sh 'echo 777'
                sh 'id ${USER}'
		sh 'sudo usermod -aG docker ${USER}'
                    sh 'rm -rf `pwd`/Directory3'
                    sh 'mkdir `pwd`/Directory3'
//                    sh 'ls -la `pwd`/Directory3'
                    script{
                        for(int i=0;i<10;i++)
                        {
                            sh 'cp `pwd`/Directory2/textFile'+i+'.txt `pwd`/Directory3/textFile'+i+'.txt'
//                            sh 'ls -la `pwd`/Directory3'
                            sh 'chmod 444 `pwd`/Directory3/textFile'+i+'.txt'
                        }
//                        sh 'ls -la `pwd`/Directory3'
                }
        }
    }
           stage('Build nginx image') {
		agent { dockerfile {
		filename 'Dockerfile'
		additionalBuildArgs '-t mynginx'
		reuseNode true			
		}
		}
            steps {
		sh 'echo 555'
		sh 'id ${USER}'
                sh 'rm -rf `pwd`/Directory4'
                sh 'mkdir `pwd`/Directory4'
            }
        }
            stage('Run container') {
            steps {
                echo 'Hello World'
		sh 'rm -rf `pwd`/Directory5'
                sh 'mkdir `pwd`/Directory5'
		sh 'cp -r `pwd`/Directory3 `pwd`/Directory5'
		sh 'cp `pwd`/index.html `pwd`/Directory5'
		sh 'echo "Jenkins Build ID: $BUILD_ID" > `pwd`/Directory5/index.html'
		script{
                    for(int i=0;i<10;i++)
                        {
                            sh 'cat `pwd`/Directory2/textFile'+i+'.txt' >> `pwd`/Directory5/index.html'
                        }
                }

               sh 'sudo docker run -p 80:80 -d -v `pwd`/Directory5:/usr/share/nginx/html mynginx'
            }
        }

}
}
