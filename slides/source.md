
class: center, middle

# DevOps 101
### Simple workflow for small teams

Ilya Mochalov 2021/09

---

# Agenda

1. What is DevOps and CI/CD?
2. Project Acme 
3. Docker
2. GitHub Actions and Registry
4. Workshop 
5. Next

---

# 1. What is DevOps and CI/CD?
<img src="https://user-images.githubusercontent.com/194400/28494977-ce74a632-6f36-11e7-9f86-f48abde49479.png" alt="components" style="width: 80%;">

Developer Operations help to easily/reliably deploy your App and keep it live! (read more: https://github.com/dwyl/learn-devops#why)

CI === build, test and package  ✅

CD === deploy/release

---

# 2. Project Acme
- E-COMMERCE application
- A front end (native mobile application or WeChat mini app)
- Backend is an API that does users, products and payments; Deployed on a single server on AliCloud

By the end of this workshop you will have:
- Sample Express API project that is Dockerized
- Configured CI with GitHub Actions and GitHub Container Registry

---

# 3. Docker 🚢
<img src="https://cdn.guru99.com/images/1/101818_0504_DockerTutor1.png" alt="components" style="width: 70%;">

- Allows to easily package an application and run it very easy on numerous platforms (Container Service, k8s, virtual servers)
- Documentation: https://docs.docker.com/get-started/overview/#docker-architecture

---

# 3. GitHub Actions and Container Registry
- CI/CD that is integrated right into your code repository

Read More: https://github.com/features/actions

---

