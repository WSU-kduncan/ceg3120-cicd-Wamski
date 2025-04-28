<img align="left" alt="AWS" width="60px" style="padding-right:10px;" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/docker/docker-original-wordmark.svg" />
<h1 align="left">Project 4-5: CICD</h1>
<br/>

## Overview
This repository focuses on setting up both continuous integration and continuous 
deployment. The continuous integration portion uses GitHub actions and workflows to
 build and push an image to DockerHub when ever there is a push to the main branch. 
The Continuous Deployment portion changes the workflow slightly by listening for tags, 
it still builds and pushes and image to DockerHub. But, it uses webhooks to send a payload to
 an AWS instance to pull and serve the new image. 

## Continuous Integration
[README](README-CI.md)  
- Using GitHub actions and workflows this project will build and push an image to DockerHub.
 This project will use things like repository secrets, and containers.

## Continuous Deployment
[README](README-CD.md)
- Using Webhooks and AWS, this project will focus on listening for a payload from DockerHub.
 Once the payload is received, the webhook definition file will make sure that the payload is a 
 push to DockerHub and that the payload came from my DockerHub repo.

## Links
- [My Docker Repo](https://hub.docker.com/repository/docker/wamski/wasky-ceg3120/general)
- [My App](http://3.230.88.186/)