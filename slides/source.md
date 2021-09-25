
class: center, middle

# DevOps 101
### Simple workflow for small teams

Ilya Mochalov 2021/09

---

# Agenda

1. What is DevOps and CI/CD?  10:30
2. Project Acme 
3. Docker 10:45
4. GitHub Actions and Registry 12:00
5. Next 
6. Q and A

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
<img src="https://wiki.aquasec.com/download/attachments/2854889/Container_VM_Implementation.png?version=1&modificationDate=1520172703952&api=v2" alt="components" style="width: 70%;">

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
const port = 3000
app.get('/', (req, res) => {
  res.send('Hello World!')
})
app.listen(port, () => {
  console.log(`Example app listening at http://localhost:${port}`)
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
- Features and pricing: https://github.com/features/actions
- Syntax: https://docs.github.com/en/actions/learn-github-actions/workflow-syntax-for-github-actions
- Find actions: https://github.com/marketplace?type=actions

You can:
- run tests
- build/package your code
- release 

---

# 4.1 Building your first action

1. Add a file `.github/workflows/build.yml`

```yaml
on: [push]
name: build
jobs:
  build:
    name: Build docker image
    runs-on: ubuntu-latest
    steps:
      - name: Determine Docker image tag and name
        id: determine_tag
        run: |
          ref="${GITHUB_REF##*/}"
          echo "::set-output name=ref::$(echo $ref)"
          echo "::set-output name=image_full::$(echo ghcr.io/${GITHUB_REPOSITORY}:$ref)"
      - name: Checkout repo
        uses: actions/checkout@v2
        with:
          fetch-depth: "0"
      - name: Build docker image
        run: docker build -t ${{ steps.determine_tag.outputs.image_full }} .
        working-directory: ./app
```

---

2. Add push to a GitHub Registry

```yaml

      - name: Push docker image
        run: |
          echo ${{ secrets.GITHUB_TOKEN }} | docker login ghcr.io -u ilyamochalov --password-stdin
          docker push ${{ steps.determine_tag.outputs.image_full }} 


```

---

# 4.2 GitHub Actions Homework

1. Add simple unit test file to our `app`
2. Create a new workflow at  `.github/workflows/test.yml`
  - what triggers do you want to use?
  - how can we stop merging our code if test fails? 

---

# 5. Next

1. How to deploy and where?
  - ali/tencent/qing cloud
2. What tools to use?
  - Ansible
  - Docker
  - GitHub actions

---

# 6. Q and A