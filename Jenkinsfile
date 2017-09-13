node {
    def gitUrl = 'https://github.com/VasiliyNesterovich/training.git'
    def gitShortUrl = '@github.com/VasiliyNesterovich/training.git'
    def task = 'task03'
    def loginGit="vasiliy.nesterovich"
    def emailGit="outcubus@yandex.ru"
    def artifactName = 'TASK.WAR'
    def gradleProp = 'gradle.properties'

        stage("Clone git") {
            git branch: task, url: gitUrl
            }

        stage("Building artifacts"){
            sh('chmod +x ./gradlew && ./gradlew clean build')
            }


    def propFile = readFile gradleProp
	def version = propFile.version
          
        stage("Copy war nexus"){
            withCredentials([usernamePassword(credentialsId: 'NEXUS', passwordVariable: 'NEXUS_PASS', usernameVariable: 'NEXUS_USER')]) {
			sh "curl -Lv -u ${NEXUS_USER}:${NEXUS_PASS} --upload-file ${WAR} \"http://172.20.20.12:8081/content/repositories/snapshots/${task}/${version}/${artifactName}\"

		stage("Deploy war"){
			def temp = "http://172.20.20.12/jk-status?cmd=update&from=list&w=loadbalancer&sw=172.20.20.10&vwa=1"
			httpRequest httpMode: 'POST', url: temp
			withCredentials([usernamePassword(credentialsId: 'TOM', passwordVariable: 'TOM_PASS', usernameVariable: 'TOM_USER')]) {
			sh "curl -Lv -u ${NEXUS_USER}:${NEXUS_PASS} \"http://172.20.20.12:8081/content/repositories/snapshots/${task}/${version}/${artifactName}\" | curl -T - -u ${TOM_USER}:${TOM_PASS} 		\"http://172.20.20.10:8080/manager/text/deploy?path=/tests&update=true\" && curl -vv -u ${TOM_USER}:${TOM_PASS} \"http://172.20.20.10:8080/manager/text/reload?path=/tests\""
			sh('sleep 60')
			http://172.20.20.12/jk-status?cmd=update&from=list&w=loadbalancer&sw=172.20.20.10&vwa=0

			def temp = "http://172.20.20.12/jk-status?cmd=update&from=list&w=loadbalancer&sw=172.20.20.11&vwa=1"
			httpRequest httpMode: 'POST', url: temp
			withCredentials([usernamePassword(credentialsId: 'TOM', passwordVariable: 'TOM_PASS', usernameVariable: 'TOM_USER')]) {
			sh "curl -Lv -u ${NEXUS_USER}:${NEXUS_PASS} \"http://172.20.20.12:8081/content/repositories/snapshots/${task}/${version}/${artifactName}\" | curl -T - -u ${TOM_USER}:${TOM_PASS} 		\"http://172.20.20.11:8080/manager/text/deploy?path=/tests&update=true\" && curl -vv -u ${TOM_USER}:${TOM_PASS} \"http://172.20.20.10:8080/manager/text/reload?path=/tests\""
			sh('sleep 60')
			http://172.20.20.12/jk-status?cmd=update&from=list&w=loadbalancer&sw=172.20.20.11&vwa=0
			}

													
            stage("Push git"){
            withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'GIT', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASS']]) {
            sh("git config --global user.name \"${loginGit}\"")
            sh("git config --global user.email \"${emailGit}\"")
            sh("git add ${gradleProp}")
            sh("git push https://${GIT_USER}:${GIT_PASS}${gitShortUrl} ${task}")
            }
            }
                    

}