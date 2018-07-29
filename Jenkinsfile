def label = "mypod"
podTemplate(label: label, containers: [
    containerTemplate(name: 'docker', image: 'docker', ttyEnabled: true, command: 'cat'),
    containerTemplate(name: 'kubectl', image: 'lachlanevenson/k8s-kubectl:v1.8.0', command: 'cat', ttyEnabled: true),
    containerTemplate(name: 'helm', image: 'lachlanevenson/k8s-helm:latest', command: 'cat', ttyEnabled: true)
  ],
  volumes: [
    hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock')
  ]) {
    node(label) {
		def repository = checkout scm
		def gitCommit = repository.GIT_COMMIT
		def gitBranch = repository.GIT_BRANCH
		def shortCommit = "${gitCommit[0..10]}"
		def previousGitCommit = sh(script: "git rev-parse ${gitCommit}~", returnStdout: true)
        
		stage('do some Docker work') {
            container('docker') {
				sh """
				docker build -t ${imageBase}:${gitBranch}-${gitCommit} .
				"""
            }
        }

        stage('do some kubectl work') {
            container('kubectl') {

            }
        }
        stage('do some helm work') {
            container('helm') {

               sh "helm ls"
            }
        }
    }
}