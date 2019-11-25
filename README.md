# Ready To Innovate

## Requirements
*  PHP
*  MySQL
* Apache

### Login & create ocp project
```shell
 oc login your-ocp-server
 oc new-project your-rti-project
```

### Deploy MySQL
```shell
#create your db
oc new-app \
 -e MYSQL_USER=rti \
 -e MYSQL_PASSWORD=rtipass \
 -e MYSQL_DATABASE=rtidb \
 --name rti-mysql -l app=rti \
 mysql:5.7
# wait until the deployment is done
# Get name of pod running MySQL
mpod=$(oc get po --selector="app=rti" --output=name|cut -d'/' -f2)

# Copy setup files to pod
oc cp ./database.sql ${mpod}:/tmp/database.sql
# exec sql
oc exec $mpod -- bash -c "mysql --user=root < /tmp/database.sql"
```

### Build & Deploy App
```shell
#build & deploy php app
oc new-app  https://github.com/serhat-dirik/rti  --name rti -l app=rti php
```

## Setup
This app uses a captcha to ensure no robot creation of users.

Securimage: A PHP class dealing with CAPTCHA images, audio, and validation
https://www.phpcaptcha.org/documentation/quickstart-guide/

To register with recaptcha, visit: https://www.google.com/recaptcha/intro/v3.html
