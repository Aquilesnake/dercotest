
<h1>DERCO TEST</h1>

Generacion MS derco entrega de datos https://jsonplaceholder.typicode.com/posts 

Creado con Node js y express 

Servicio con ip publica en :


EC2 con ip - publica activa : 

http://18.237.59.132:3000/

Implementacion CICD con Codeploy agent en AWS 


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