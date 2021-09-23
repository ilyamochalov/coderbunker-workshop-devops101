
class: center, middle

# DevOps 101
### Simple workflow for small teams

Ilya Mochalov 2021/09

---

# Agenda

1. What is DevOps and CI/CD?
2. Project Acme 
3. Docker
4. GitHub Actions and Registry
4. Workshop 
5. Next

---

# 1. What is DevOps and CI/CD?
<img src="https://user-images.githubusercontent.com/194400/28494977-ce74a632-6f36-11e7-9f86-f48abde49479.png" alt="components" style="width: 80%;">

Developer Operations help to easily/reliably deploy your App and keep it live! (read more: https://github.com/dwyl/learn-devops#why)

CI === build, test and package

CD === deploy/release

---

# 2. Project Acme
Description:
- E-COMMERCE application
- A front end (native mobile application or WeChat mini app)
- Backend is an API that does users, products and payments management

Backend can de deployed:
- Services like Heroku, Elastic Beanstalk (AWS), Back4App
- Elastic container registry (AWS, Alicloud), K8S, HC Nomad
- Virtual machine (EC2, ECS, etc..) + Dockerized app ✅

By the end of this workshop you will have:
- Sample Express API project that is Dockerized
- Configured CI with GitHub Actions and GitHub Container Registry

---

# 3. Docker 🚢
<img src="https://cdn.guru99.com/images/1/101818_0504_DockerTutor1.png" alt="components" style="width: 70%;">

- Allows to easily package an application and run it very easy on numerous platforms (Container Service, k8s, virtual machine)
- Documentation: https://docs.docker.com/get-started/overview/#docker-architecture

---

# 3.1 Creating Express app

1. Create GitHub repo
2. Run the following
```sh
$ mkdir app
$ cd app
$ docker run -exec -it -v $(pwd):/app -p 3000:3000 node:alpine sh
/ # cd /app and npm init
/app # npm install express --save
```
3. Create `index.js`
```nodejs
const express = require('express')
const app = express()
app.get('/', (req, res) => {
  res.send('Hello World!')
})
app.listen(port, () => {
  console.log(`Example app listening at http://localhost:3000`)
})
```
4. Run the app
```sh
/app # node index.js 
Example app listening at http://localhost:3000
```
---
# 3.2 Build and run docker image locally
1. Add a docker file `Dockerfile`
```Dockerfile
FROM node:alpine
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 3000
ENTRYPOINT ["node", "index.js"]
```
2. Build an image
```sh
$ docker build -t devops101:latest .
```

3. Run an image
```sh
$ docker run -p 3000:3000 devops101:latest
```
---
# 3.3 Docker Homework

Pass a configurations to your application as environmental variables

1. Edit `index.js` to return a content of env var `ENV_VAR`
2. Rebuild an image 
3. Update docker run command command from 3.2 so it injects an env var into running container (see [reference](https://docs.docker.com/engine/reference/run/))

---

# 4. GitHub Actions and Container Registry
- CI/CD that is integrated right into your code repository

Features and pricing: https://github.com/features/actions
Syntax: https://docs.github.com/en/actions/learn-github-actions/workflow-syntax-for-github-actions
Find actions: https://github.com/marketplace?type=actions

You can:
- run tests
- build/package your code
- release 

---

# 4.1 Building your first action

1. 