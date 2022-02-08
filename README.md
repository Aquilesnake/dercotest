
<h1>DERCO TEST</h1>

<h2>Generacion microservicio con entrega de datos API: https://jsonplaceholder.typicode.com/posts</h2> 

Autor : Cristian Tapia



Creacion de Microservicio realizado con Node js y Express, muestrar datos de API.    

- Servicio EC2 con IP-publica:

Test de servicio: 
http://18.237.59.132:3000/

Muestra json : 
http://18.237.59.132:3000/products

Muestra API 
(https://jsonplaceholder.typicode.com/posts ) :
http://18.237.59.132:3000/datos

- Se genera Virtual Server -> EC2  
Generacion de instancia por defecto (Gratuita)

- Se realiza configuracion IP en Security Group 

SSH 22
HTTP 80
TCP 3000

- Se genera configuracion en IAM -> Roles 

AWS Service -> AmazonEC2roleforAWSCodePloy
Codedeploy Service -> AWSCodeDeployRole

<h2>Implementacion CICD con CodeDeploy-agent servicio de AWS</h2> 

Creacion de aplicacion - Nombre aplicacion: testderco-app, Plataforma: EC2/On-premise

Creacion de deployment group - Nombre: testderco-app-group , Rol de servicio: arn.aws.iam:::  

Deployment Type - In place  

Environment - Amazon E2 instance

Tag : Name: dercotest

Deployment Setting : allatOnce

<h2>Configuracion piepeline en la nube.</h2> 

Creacion del Pipeline: 

name: dercotest-app-pipeline

source : github 2  

nombre de coneccion a github : dercotest-app-connection

select new app -> seleccionar dercotest (proyecto)

nombre de repositorio: dercotest

nombre de rama : main 

build - skip build stage - skip

add deploy stage : seleccionar -> AWS CodeDeploy

nombre de aplicacion : dercotest-app

deployment group : derctotest-app-group


Se agrega appspec.yml con hooks: 

version: 0.0
os: linux
files:
  - source: /
    destination: /home/ec2-user/dercotest
hooks:
  ApplicationStop:
    - location: scripts/application_stop.sh
      timeout: 300
      runas: ec2-user
  BeforeInstall:
    - location: scripts/before_install.sh
      timeout: 300
      runas: ec2-user
  ApplicationStart:
    - location: scripts/application_start.sh
      timeout: 300
      runas: ec2-user

Se agrega carpeta scripts con scripts de instalacion, detencion y inicio para los nuevos despliegues (hooks).       


<h2>Implementacion de ramas en Github con metodologia Gitflow</h2> 

feature -> Develop -> Release -> Main 

===========================

<h2>Instalacion CodeDeploy:</h2> 

===========================

EC2 script on creation to install the CodeDeploy Agent:

```
#!/bin/bash
sudo yum -y update
sudo yum -y install ruby
sudo yum -y install wget
cd /home/ec2-user
wget https://aws-codedeploy-us-east-1.s3.amazonaws.com/latest/install
sudo chmod +x ./install
sudo ./install auto
```

Check if CodeDeploy agent is running:
```
sudo service codedeploy-agent status
```

Location for CodeDeploy logs:
```
/opt/codedeploy-agent/deployment-root/deployment-logs/codedeploy-agent-deployments.log
```

Uninstall CodeDeploy Agent:
```
sudo yum erase codedeploy-agent
```

===========================

<h2>Webgrafía:</h2>

===========================

Metodologia GitFlow :
https://www.juanmacivico87.com/metodologia-trabajo-git-flow/#:~:text=Por%20si%20no%20has%20escuchado,para%20gestionar%20proyectos%20m%C3%A1s%20grandes.

Creacion appspec.yml
https://docs.aws.amazon.com/codedeploy/latest/userguide/reference-appspec-file.html#appspec-reference-server

Deployment in EC2 
https://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-steps-server.html

AppSpec 'hooks' section
https://docs.aws.amazon.com/codedeploy/latest/userguide/reference-appspec-file-structure-hooks.html#appspec-hooks-server

En caso de errores - CodeDeploy : 
https://docs.aws.amazon.com/es_es/codedeploy/latest/userguide/troubleshooting.html
