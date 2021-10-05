import groovy.transform.Field

podTemplate(label: 'bc16-be', containers: [
	containerTemplate(name: 'bc16be-docker', image: 'docker:19.03', command: 'cat', ttyEnabled: true),
	containerTemplate(name: 'bc16-java', image: 'maven:3.8.1-adoptopenjdk-11', command: 'cat', ttyEnabled: true) ],
	volumes: [hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock')]
) {

  node('bc16-be'){
  	environment {
		docker_image=""
		DOCKERHUB_CREDENTIALS= credentials('rishmitha-dockerhub')
		// MY_KUBECONFIG = credentials('master2-rishmita')
	}
	


// 		stage('Checkout Source') {
// 			steps {
//         git 'https://github.com/PDhanrajnath/fe.git'
//       }
//     }
	   stage('Checkout Source') {
      
        git 'https://github.com/VenkataRishmithaAita/bc16-jenkins-backend.git'
      
    }
	  stage('Build Jar'){
	           container('bc16-java'){
			   
	            sh 'organizationService/gradlew build'

	            sh 'jobsService/gradlew build'
	           }
	            
	    }
	  
	  

    
    stage('Build Docker'){
			
				
				container('bc16be-docker'){
                
					//sh 'docker build -t dhanrajnath/be_jenkins .'
					//sh 'docker images'
					sh 'docker build -t rishmitha/org_jenkins organizationService/'
            				sh 'docker build -t rishmitha/job_jenkins jobsService/'
            				sh 'docker images'
				}
			
		}
		
		stage('Push Docker'){
			
				container('bc16be-docker'){
					sh 'ls'
    withCredentials([usernamePassword(credentialsId: 'rishmitha-dockerhub', usernameVariable: 'username', passwordVariable: 'password')]) {
						sh 'echo $PASSWORD'
                         sh 'docker login -u $username -p $password'
						echo USERNAME
						echo "username is $USERNAME"
						//sh 'docker push dhanrajnath/be_jenkins'
						sh 'docker push rishmitha/org_jenkins'
	            				sh 'docker push rishmitha/job_jenkins'
               
				}
			}
		}

		   stage ('BC16-GC') {
        	
		    build job: 'bc16-r', parameters: [string(name: 'master', value: env.BRANCH_NAME)]
	
        }
    
	
// 	post{
// 	    always{
// 	        container('bc15-docker'){
// 	         sh 'docker logout'
// 	    }
// 	    }
// 	}
  }

  


}





// pipeline{
	
// 	agent{
			
// 			kubernetes {
		    
// 			    yaml '''
//         apiVersion: v1
//         kind: Pod
//         spec:
//           containers:
//           - name: bc15-java
//             image: maven:3.8.1-adoptopenjdk-11
//             command:
//             - cat
//             tty: true
//           - name: bc15-docker
//             image: docker:19.03
//             command:
//             - cat
//             tty: true
//             volumeMounts:
//             - name: dockersock
//               mountPath: /var/run/docker.sock
//           volumes:
//             - name: dockersock
//               hostPath:
//                 path: /var/run/docker.sock
//         '''
    
//     }
// 	}
	
		
//     environment {
//         docker_image=""
//         DOCKERHUB_CREDENTIALS= credentials('Dhanrajnath_Docker')
//         MY_KUBECONFIG = credentials('config-file')
//     }
// 	stages{
// 	    stage('Checkout Source') {
//       steps {
//         git 'https://github.com/PDhanrajnath/be.git'
//       }
//     }
// 	    stage('Build Jar'){
	     
// 	        steps{
// 	           container('bc15-java'){
   
// 	            sh 'mvn -B -DskipTests clean package'

// 	           }
	            
// 	    }}
// 	    stage('Build Docker'){
//             steps{
//             container('bc15-docker'){

//             sh 'docker build -t dhanrajnath/be_jenkins .'
//             sh 'docker images'
            
//         }}  
// 	    }
	    
// 	   stage('Push Docker'){
// 	        steps{
// 	            container('bc15-docker'){
// 	            sh 'ls'
// 	            sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
// 	            sh 'docker push dhanrajnath/be_jenkins:latest'
	           
// 	        }
// 	        }
// 	    }
               
                    
				
// 	}    
// 	post{
// 	    always{
// 	        container('bc15-docker'){
// 	         sh 'docker logout'
// 	    }}
// 	}
               
                   		
// 	}
	
