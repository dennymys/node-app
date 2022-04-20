# Step by Step Make Node.js Image use Docker
### Denny Manuel Yeremia Sinurat(185150707111004)

*in this exercise i am using ubuntu os on aws server, make server in AWS ec2 first and connect it use ssh 
<br>


#### 1. Install Docker:

```yaml
-	curl -fsSL https://get.docker.com -o get-docker.sh
-	sh get-docker.sh
```

#### 2. Add usermode docker:
```yaml
sudo usermod -aG docker ubuntu
```
<br>

#### 3. To see images in docker:

```yaml
docker images
```

#### 4. Pull ubuntu images to docker:

```yaml
docker pull ubuntu:focal
```

#### 5. Pull node.js images to docker:

```yaml
docker pull node:latest
```

#### 6. Make new container in docker:

```yaml
docker run -it ubuntu:focal /bin/bash
```

#### 7. check the running image in docker:

```yaml
docker ps-a
```

#### 8. create a new directory with the name node-app and change directory to node-app:

```yaml
- mkdir node-app
- cd node-app
```

#### 9. install node.js to docker  :

```yaml
- docker run -it -v $(pwd):/app node:latest /bin/bash
- cd /app
- npm init
- npm install express
```

#### 10. Make index.js file:

```yaml
nano index.js
```
####copy and paste this script to index.js file:

```yaml

const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => {
  res.send('Hello World!')
})

app.listen(port, () => {
  console.log(`Example app listening on port ${port}`)
})

```

#### 11. Run this to accsess index.js in port 3000:

```yaml
docker run -it -v $(pwd):/app -w /app -p 3000:3000 node:latest index.js
```
File executed successfully on port 3000

#### 12. Make docker image:

```yaml
nano Dockerfile
```
copy & paste this script:

```yaml
FROM node:latest

WORKDIR /app

RUN npm install -g nodemon

CMD nodemon
```

#### 13. Build docker :

```yaml
docker build -t node-app
```

#### 14. run the docker and access server public ip address use 3000 port:

```yaml
docker run -d -p 3000:3000 -v $(pwd):/app node-app
```

#### 15. Make new directory name Dockerfile.prod:

```yaml
nano Dockerfile.prod
```
copy & paste this script:

```yaml
FROM node:latest

ADD . /app

WORKDIR /app

RUN npm install -g pm2

RUN npm install

CMD pm2-runtime index.js
```

#### 16. build docker:

```yaml
docker build -t node-app-prod -f Dockerfile.prod .
```

#### 17. kill docker which is using port 3000:

```yaml
docker kill {container}
```

#### 18. Access server use ip_address_public:3000 

#### 19. Push image to docker hub:

```yaml
- docker login (need username and password)
- docker tag node-app-prod dennys27/node-app-prod
- docker push dennys27/node-app-prod

```

#### 20. Open docker hub and make sure the image is successfully pushed:

#### 21. Make new server in AWS EC2:

#### 22. use ssh to remote ec2-server:

```yaml
sudo ssh -i MyKey.pem(ssh key) ubuntu(ec2-user)@ip_address_public
```


#### 23. Install docker in new server and give superuser to docker ubuntu:

```yaml
-	curl -fsSL https://get.docker.com -o get-docker.sh
-	sh get-docker.sh
- sudo usermod -aG docker ubuntu
```


#### 24. Take your node.js images from your docker hub :

```yaml
docker run -d -p 3000:3000 dennys27/node-app-prod
```


