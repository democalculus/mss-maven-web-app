# maven-web-application
#walmart-dev-mss branch added   Dec_91_22
#configure nexus details in jenkins
#Path
#/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/demo-maven_3.8.6/conf
<server>
      <id>deploymentRepo</id>
      <username>repouser</username>
      <password>repopwd</password>
    </server>
#where to past it
-->
    <server>
      <id>nexus</id>
      <username>admin</username>
      <password>passw0rd</password>
    </server>
  </servers>

  <!-- mirrors
##################poll scm trigger by change in commit ID##########
Build #5 (Dec 19, 2022, 11:08:06 AM)
Add description
Changes
push (details / githubweb)
Started by an SCM change

	Revision: 05e7d9b25bb5202def325e884c64c7d253d08f9c
Repository: https://github.com/democalculus/maven-web-application.git
refs/remotes/origin/walmart-dev-mss

##################BUILD periodically this everything you have schedule build will trigger periodically###
#this is used for ongoing development
Build #6 (Dec 19, 2022, 11:11:00 AM)
Add description
No changes.
Started by timer

	Revision: 05e7d9b25bb5202def325e884c64c7d253d08f9c
Repository: https://github.com/democalculus/maven-web-application.git
refs/remotes/origin/walmart-dev-mss


#add webhook update

http://34.125.180.153:8080/github-webhook/
Build #17 (Dec 19, 2022, 11:25:17 AM)
Progress:
[cancel]
Add description
Changes
push (details / githubweb)
push (details / githubweb)
Started by GitHub push by democalculus

	Revision: 606be053b2c227d35ae69979fac4ff22efe09a53
Repository: https://github.com/democalculus/maven-web-application.git
refs/remotes/origin/walmart-dev-mss

######slack
https://slack.com  
###update
### audit trail logs ################
[root@mss-jenkins jenkins]# ls
audit_trails.log.0

Dec 19, 2022 5:10:39,644 PMFreestyle_projects » Maven_Folder » wallmart-dev-mss #12 Started by GitHub push by democalculus, Parameters:[] on node Jenkins started at 2022-12-19T17:10:08Z completed in 30917ms completed: SUCCESS
Dec 19, 2022 5:10:42,299 PM'Freestyle_projects/Maven_Folder/wallmart-qa-mss' (class hudson.model.FreeStyleProject) used credentials 'democalculus-Tom-box_creds_ID' (class com.cloudbees.plugins.credentials.impl.UsernamePasswordCredentialsImpl).
Dec 19, 2022 5:10:43,974 PMFreestyle_projects » Maven_Folder » wallmart-qa-mss #4 Started by GitHub push by democalculus, Parameters:[] on node Jenkins started at 2022-12-19T17:10:13Z completed in 30338ms completed: SUCCESS


#####SonarQube
#squ_1bd82d92f55afca8d9fff16fdb00ec42f786bd39
