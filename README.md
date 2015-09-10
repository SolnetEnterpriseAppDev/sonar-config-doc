##Server
###Setup
> 1. unzip within the sonar directory `/opt/sonar`
> 2. setup a shell script - 
> `#!/bin/sh
>cd /opt/sonar/sonarqube-5.1.2/bin/linux-x86-64
>./sonar.sh $*`
>3. copy it to `/etc/init.d`
>4. start sonar up `/etc/init.d/sonar start`
>5. check the status `/etc/init.d/sonar/status`
>>This should yield something like :
>>`localhost:sonar user$ sonar status
>>SonarQube is running (7693).`

>6. verify you the administrator login
>Browse to http://localhost:9000 
>login with the default System administrator login
>( default credentials are `admin`/`admin`)
###Plugins
Where an enterprise environment doesn't provide the sonar box with access to the internet, simply download the plugins and copy them to _`<SONAR_HOME>/extensions/plugins`_ directory.
##Client
Sonar can be run across source code using _maven_ or by using _sonar-runner_.
###Maven _config_
To setup maven add the following section to your maven settings file
The following example has the sonar server deployed on _sonarhost_ and a _mysql_ db installed on _sonardbhost_.
>`<profiles>
.<profile>
..<id>sonarRemote</id>
..<activation>
...<activeByDefault>true</activeByDefault>
..</activation>
..<properties>
...<sonar.jdbc.url>jdbc:mysql://sonardbhost:3306/sonar</sonar.jdbc.url>
...<sonar.jdbc.driver>com.mysql.jdbc.Driver</sonar.jdbc.driver>
...<sonar.jdbc.username>sonar</sonar.jdbc.username> 
...<sonar.jdbc.password>sonar</sonar.jdbc.password>
...<sonar.host.url>http://sonarhost:9000/</sonar.host.url>
..</properties>
.</profile>
</profiles>`
###Maven _invocation_
The sonar maven plugin is invoked as follows:
`maven sonar:sonar`
The following example shows invocation using a select profile in this case `sonarLocal` and passing the test source directory as a `-D` parameter:
`mvn -P sonarLocal sonar:sonar -Dsonar.tests=src/test/java`
###Sonar-runner _config_
Sonar-runner requires runtime properties - these can be supplied by a properties file in the project
>`# must be unique in a given SonarQube instance`
`sonar.projectKey=hard:amu`
`# this is the name displayed in the SonarQube UI`
`sonar.projectName=EMU`
`sonar.projectVersion=1.0`
`# Path is relative to the sonar-project.properties file. Replace "\" by "/" on Windows.`
`# Since SonarQube 4.2, this property is optional if sonar.modules is set. `
`# If not set, SonarQube starts looking for source code from the directory containing `
`# the sonar-project.properties file.`
`sonar.sources=.`
 `# Encoding of the source code. Default is default system encoding`
`#sonar.sourceEncoding=UTF-8`
`sonar.exclusions=CAS-1.3.0/**/*,cool-php-captcha-0.3/**/*,Smarty-3.1.15/**/*,templates_c/*,js/*,themes/**/*`
###Sonar-runner _invocation_
Sonar-runner is invoked from the directory where the properties file is located.
Simply by entering the command line:
`sonar-runner`
You can pass extra parameters - the following example passes the `sonar.exclusions` in using the `-D` parameter as follows:
`$ sonar-runner.sh -Dsonar.exclusions=CAS-1.3.0/**/*,cool-php-captcha-0.3/**/*,Smarty-3.1.15/**/*,templates_c/*,js/*,themes/**/*`
