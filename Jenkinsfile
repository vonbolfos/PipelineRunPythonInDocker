pipeline{
    agent {
        docker{ image 'centos:7'
                args '-u root'
                label 'Jenkinsy'
        }
    }
	
   triggers {
        gitlab(triggerOnPush: true, triggerOnMergeRequest: true, branchFilterType: 'All')
   }


   stages{
        stage('InstallDependencies'){
            steps{
                sh 'yum -y install python3 python3-pip zlib-devel gcc git pip3'
            }
        }
        stage('CloneRepository'){
            steps{
                sh 'git clone https://github.com/linuxacademy/content-pipelines-cje-labs.git'
            }
        }
	stage('AddDogClassRequirements'){
            steps{
                sh 'pip install -r content-pipelines-cje-labs/lab3_lab4/dog_pics_downloader/requirements.txt'
            }
        }
	stage('ExecuteDogClassScript'){
            steps{
                sh 'python content-pipelines-cje-labs/lab3_lab4/dog_pics_downloader/dog_pic_get_class.py'
            }
        }
        stage('AddDogClassWatermarksRequirements'){
            steps{
                sh 'pip3 install -r content-pipelines-cje-labs/lab3_lab4/image_watermarker/requirements.txt'
            }
        }
        stage('ExecuteDogClassWatermarksScript'){
            steps{
                sh 'python3 content-pipelines-cje-labs/lab3_lab4/image_watermarker/watermark.py'
            }
        }
    }
    post{
        success{
            archiveArtifacts artifacts: '*.jpg, *watermarked.jpg'	
	    echo "Pipeline has been successfully built"
        }
	failure {
	       echo "Pipeline build has failed"
	}
        cleanup{
            sh 'rm -rf content-pipelines-cje-labs'
        }
    }
}
