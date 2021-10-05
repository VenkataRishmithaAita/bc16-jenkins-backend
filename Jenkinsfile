import groovy.transform.Field

podTemplate(label: 'bc16', containers: [
	containerTemplate(name: 'docker', image: 'docker:19.03', command: 'cat', ttyEnabled: true),
	containerTemplate(name: 'java', image: 'adoptopenjdk/openjdk11', command: 'cat', ttyEnabled: true) ],
	volumes: [hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock')]
)
	{
		node('bc16'){
    environment {
        docker_image=""
        DOCKERHUB_CREDENTIALS= credentials('rishmitha-dockerhub')
        MY_KUBECONFIG = credentials('master2-rishmita')
    }

//      withEnv([
//         "VERSION=${env.BUILD_NUMBER}",


//     ])
// 			{

	    stage('Checkout Source') {
      
        git 'https://github.com/VenkataRishmithaAita/bc16-jenkins-backend.git'
      
    }
	    stage('Build Jar'){
	     
	        
	           container('java'){
   
	            sh 'organizationService/gradlew build'

	            sh 'jobsService/gradlew build'
	           }
	            
	    }
	    stage('Build Docker'){
            
            container('docker'){

            sh 'docker build -t rishmitha/org_jenkins organizationService/'
            sh 'docker build -t rishmitha/job_jenkins jobsService/'
            sh 'docker images'
            
        }
	    }
	    
	   stage('Push Docker'){
	        
	            container('docker'){
                withCredentials([usernamePassword(credentialsId: 'rishmitha-dockerhub', usernameVariable: 'username', passwordVariable: 'password')]) {

	          sh 'docker login -u $username -p $password'
	            //sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
	            sh 'docker push rishmitha/org_jenkins'
	            sh 'docker push rishmitha/job_jenkins'
	           
	        }
              }
	    }
             stage ('Invoking helm build') {
        	
		    build job: 'bc16-r', parameters: [string(name: 'master', value: env.BRANCH_NAME)]
	    }  
                    
				
// 	}    
    }}
