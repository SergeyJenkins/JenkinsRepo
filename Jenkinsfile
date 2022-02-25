pipeline {
    agent { label 'JenkinsSlave' }
        stages {
            stage('Create files') {
                steps {
                    sh 'rm -rf `pwd`/Directory1'
                    sh 'mkdir `pwd`/Directory1'
                    script {
                        for(int i=0;i<10;i++) {
                            sh 'echo "some text for file - "'+i+' > `pwd`/Directory1/textFile'+i+'.txt'
                        }
                    }
                }
        }
            stage('Copy files and insert data') {
                steps {
                    sh 'rm -rf `pwd`/Directory2'
                    sh 'mkdir `pwd`/Directory2'
                    script {
                        for(int i=0;i<10;i++) {
                            sh 'cp `pwd`/Directory1/textFile'+i+'.txt `pwd`/Directory2/textFile'+i+'.txt'
                            sh 'date >> `pwd`/Directory2/textFile'+i+'.txt'
                        }
                    }
            }
        }
            stage('Copy files and make them read-only') {
                steps {
                    sh 'id ${USER}'
		    sh 'sudo usermod -aG docker ${USER}'
                    sh 'rm -rf `pwd`/Directory3'
                    sh 'mkdir `pwd`/Directory3'
                    script {
                        for(int i=0;i<10;i++) {
                            sh 'cp `pwd`/Directory2/textFile'+i+'.txt `pwd`/Directory3/textFile'+i+'.txt'
                            sh 'chmod 444 `pwd`/Directory3/textFile'+i+'.txt'
                        }
                    }
            }
        }
            stage('Build nginx image') {
	        agent { 
	            dockerfile {
	                filename 'Dockerfile'
		        additionalBuildArgs '-t mynginx'
		        reuseNode true			
	            }
	        }
                steps {
		    sh 'id ${USER}'
                    sh 'rm -rf `pwd`/Directory4'
                    sh 'mkdir `pwd`/Directory4'
                }
            }
            stage('Run container') {
                steps {
		    sh 'rm -rf `pwd`/Directory5'
                    sh 'mkdir `pwd`/Directory5'
		    sh 'cp -r `pwd`/Directory3 `pwd`/Directory5'
		    sh 'cp `pwd`/index.html `pwd`/Directory5'
		    sh 'echo "Jenkins Build Tag : $BUILD_TAG" > `pwd`/Directory5/index.html'
		    script {
                        for(int i=0;i<10;i++)
                            {
                                sh 'cat `pwd`/Directory2/textFile'+i+'.txt >> `pwd`/Directory5/index.html'
                            }
                    }
                    sh 'docker run -p 80:80 -d -v `pwd`/Directory5:/usr/share/nginx/html mynginx'
                }
           }

        }
}
