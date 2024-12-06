pipeline{
	agent any
	environment{
		PROJECT_ID = 'opensou₩rcesw-438913'
		CLUSTER_NAME = 'opensource-sw-k8s'
		LOCATION = 'asia-northeast3-a'
		CREDENTIALS_ID = '6c611442-f665-4da6-8b51-9f00afed66c1'
	}
	stages{
		stage("Checkout code"){
			steps{
				checkout scm
			}
		}
		stage("Build image"){
			steps{
				script{
					myapp = docker.build("imNaNye/yorijori:${env.BUILD_ID}")
					}
				}
			}
		stage("Push image"){
			steps{
				script{
					docker.withRegistry('https://registry.hub.docker.com', 'dockerhub'){
						myapp.push("latest")
						myapp.push("${env.BUILD_ID}")
						}
					}
				}
			}
		stage("Deploy to GKE"){
			when{
				branch 'master'
			}
			steps{
				sh "sed -i 's/yorijori:latest/yorijori:${env.BUILD_ID}/g' deployment.yaml"
			step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
				}
			}
		
		}
	}
