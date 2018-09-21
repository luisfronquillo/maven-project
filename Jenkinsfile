pipeline{
agent 'master'
properties([parameters([string(defaultValue: '6', description: '', name: 'MAX_ERROR', trim: false)])])
	stages{
		stage('compilar') {
			steps{
				node(label:'master') {
					checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'git-luisf', url: 'https://github.com/luisfronquillo/maven-project']]])
					sh 'mvn clean package'
					archiveArtifacts artifacts: '**/*.war', onlyIfSuccessful: true
				}
			}
		}
		stage('scancode') {
			steps{
				node(label:'master') {
					sh 'mvn checkstyle:checkstyle'  
					checkstyle canComputeNew: false, canRunOnFailed: true, defaultEncoding: '', failedTotalAll: params.MAX_ERROR, healthy: '', pattern: '', unHealthy: ''
				}
			}
		}
		stage('deploy') {
			steps{
				node(label:'Windows') {
					copyArtifacts filter: '**/*.war', fingerprintArtifacts: true, flatten: true, projectName: 'codepipe', selector: specific(env.BUILD_NUMBER), target: env.TOMCAT_HOME

				}
			}
		}	
	}
}




