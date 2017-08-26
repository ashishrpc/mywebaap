jenkin user/password : admin/10c576c5ea5f494baeb92ef26cb5e908
C:\Program Files (x86)\Jenkins\secrets\initialAdminPassword


=============== build clean and install on tomcat server from cmd prompt using : mvn tomcat7:deploy
1) C:\Java\Apache Software Foundation\apache-tomcat-7.0.75\conf\tomcat-users.xml update role:

<role rolename="manager-gui"/>
  <role rolename="manager-script"/>
  <role rolename="manager-jmx"/>
  <role rolename="manager-status"/>
  <role rolename="admin-gui"/>
  <role rolename="admin-script"/>
  <user username="admin" password="admin" roles="manager-gui,manager-script,manager-jmx,manager-status,admin-gui,admin-script"/>
  
2) start tomcat from CMD prompt: start.bat
C:\Java\Apache Software Foundation\apache-tomcat-7.0.75\bin\

3)install plugin for tomcat7 by dependency tag in pom.xml (build or clean) but not work from STS
 <!-- https://mvnrepository.com/artifact/org.apache.tomcat.maven/tomcat7-maven-plugin -->
	<dependency>
	    <groupId>org.apache.tomcat.maven</groupId>
	    <artifactId>tomcat7-maven-plugin</artifactId>
	    <version>2.2</version>
	</dependency>
	
4) add also in pom.xml (main plugin: tomcat7-maven-plugin)
<plugins>
      <plugin>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.1</version>
        <configuration>
          <source>1.7</source>
          <target>1.7</target>
        </configuration>
      </plugin>
      <plugin>
        <artifactId>maven-war-plugin</artifactId>
        <version>2.4</version>
        <configuration>
          <warSourceDirectory>WebContent</warSourceDirectory>
          <failOnMissingWebXml>false</failOnMissingWebXml>
        </configuration>
      </plugin>

		<plugin>
			<groupId>org.apache.tomcat.maven</groupId>
			<artifactId>tomcat7-maven-plugin</artifactId>
			<version>2.2</version>
			<configuration>
				<url>http://localhost:8082/manager/text</url>
				<server>TomcatServer</server>
				<path>/mywebaap</path>
			</configuration>
		</plugin>
    </plugins>
5)on CMD prompt, go in your project directory where pom.xml exist and type "mvn tomcat7:deploy" and press enter
its automaticaly install in manager. view it in http://localhost:8082/manager/text

============================================== Jenkins ================================
https://www.jdev.it/deploying-your-war-file-from-jenkins-to-tomcat/
6) "Deploy to container" plugin and install
7)go to your job and select “Configure”. Next, scroll down to the bottom of the page to the “Post-build Actions”. Select the option “Deploy war/ear to a container” from the “Add post-build action” dropdown button.
a) WAR/EAR files : **/*.war
b) Context path : mywebaap
c) Containers : tomcat 7.x -> admin/admin -> http://localhost:8082
