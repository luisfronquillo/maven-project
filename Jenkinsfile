pipeline{
agent 'master'
	stages{
		stage('compilar') {
			node(label:'master') {
				checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'git-luisf', url: 'https://github.com/luisfronquillo/maven-project']]])
				sh 'mvn clean package'
				archiveArtifacts artifacts: '**/*.war', onlyIfSuccessful: true
			}
		}
		stage('scancode') {
			node(label:'master') {
				sh 'mvn checkstyle:checkstyle'  
				checkstyle canComputeNew: false, canRunOnFailed: true, defaultEncoding: '', failedTotalAll: params.MAX_ERROR, healthy: '', pattern: '', unHealthy: ''
			}
		}
		stage('deploy') {
			node(label:'Windows') {
				copyArtifacts filter: '**/*.war', fingerprintArtifacts: true, flatten: true, projectName: 'codepipe', selector: specific(env.BUILD_NUMBER), target: env.TOMCAT_HOME

			}
		}	
	}
}




