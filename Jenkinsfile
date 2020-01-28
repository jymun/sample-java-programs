def nlabel = "jenkins-project-${UUID.randomUUID().toString()}"

podTemplate(label: nlabel, 
	containers: [ 
		containerTemplate(name: 'maven', image: 'maven', ttyEnabled: true, command: 'cat' ),
		containerTemplate(name: 'docker', image: 'docker', ttyEnabled: true, command: 'cat'),
		containerTemplate(name: 'kubectl', image: 'lachlanevenson/k8s-kubectl', ttyEnabled: true, command: 'cat')
	], 
	volumes: [
        hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock'),
        persistentVolumeClaim(mountPath: '/root/.m2', claimName: 'jenkins-maven-pvc')
    ]
) 
{
	node (nlabel) { 
		
		stage ('Checkout') {
			checkout scm
		}
			
		stage ('maven build') {
			container('maven') { 
				sh ("mvn -DskipTests package" -Dsonar.host.url=http://192.168.0.224:9000)
			}
		}
		

	} 
} 
