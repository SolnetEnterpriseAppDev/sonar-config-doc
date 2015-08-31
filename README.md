Setup
------- 
> 1. unzip within the sonar directory `/opt/sonar`
> 2. setup a shell script - 
> `#!/bin/sh
>cd /opt/sonar/sonarqube-5.1.2/bin/linux-x86-64
>./sonar.sh $*`
>3. copy it to `/etc/init.d`
>4. start sonar up `/etc/init.d/sonar start`
>5. check the status `/etc/init.d/sonar/status`
>This should yield something like 
>`localhost:sonar user$ sonar status
SonarQube is running (7693).`
>6. verify you the administrator login
>Browse to http://localhost:9000 
>login with the default System administrator login
>( credentials are `admin`/`admin`)

>


