<img align="left" alt="AWS" width="60px" style="padding-right:10px;" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/docker/docker-original-wordmark.svg" />
<h1 align="left">Project 4: Docker Images and Repos</h1>
<br/>

## Docker setup:
For an M-Series Mac, go to `https://www.docker.com/products/docker-desktop/` and select `Download for Mac - Apple Silicon`
- To make sure docker is installed, first try to open the app so that the backend starts for docker to operate correctly. 
Then try to docker pull an image, ex: docker pull hello-world
  - You can run this image with: `docker run hello-world`

## Manual Docker Image Setup
- `Docker run -it -p 5001:4200 -v ~/Desktop/Repos/ceg3120-cicd-Wamski/Project4/angular-site/.:/app/ -w /app node:18-bullseye sh -c "npm install -g @angular/cli && ng serve --host 0.0.0.0"`
  - `-it`: Enables the interactive interface.
  - `-p`: Specifies first the port your host is going to use and then the port the container is using.
  - `-v`: Binds the host's `angular-site` directory to the container.
    - In the implementation above, the `.:` binds the angular-site directory to the directory named app in the container.
  - `-w`: Changes the working directory to `app` in the container
  - `node:18-bullseye`: The image to build on top of.
  - `sh -c`: Runs a list of commands in the container, these commands are seperated by `&&`.
- To view the angular site after running the command above visit [127.0.0.1:5001](http://127.0.0.1:5001)
  

## Docker File:  
Dockerfile Contents:
- This docker file uses a base image of `node:18-bullseye` and copies in an angular site. Once the 
contents of angular-site is copied into `app`, the working directory is changed to `app` and the angular 
cli is installed. Finally, there is a command that is ran only when the image is run, this command starts the 
angular app.

Building an image from the docker file :
1. Using `cd`, on your terminal navigate to the location of the docker file.
2. Run the command `docker build -t wamski/wasky-ceg3120 .`
   - `-t` is the tag the image will receive.
     - `-t [username]/[repo]`

Running the docker image:
Using the image created from the previous step run the command:
- `docker run -it -p 5001:4200 wamski/wasky-ceg3120`
  - `-it`: Enables the interactive interface.
  - `-p`: Specifies first the port your host is going to use and then the port the container is using.
- To verify that the app is running:
  1. In your CLI, you should see a line that looks similar to `✔ Compiled successfully.`
  2. In your browser, you can connect to the app by visiting: [127.0.0.1:5001](http://127.0.0.1:5001)

## Docker Repos:
To create a public/private repository:
1. Visit: `https://hub.docker.com/repositories/[UserName]`
   - Replace `[UserName]` with your username
     - My repos: https://hub.docker.com/repositories/wamski
2. Select create repository, and give it a name.
   - Select either public or private.

Creating a Personal Access Token:
1. In your docker `Account Settings` navigate to `Personal access tokens`
2. Select Generate new token
3. Give this token the permissions of: Read, and Write.
   - These are needed because this is a completely empty repo that the image create in the project will be pushed to. 
4. Click `Generate`
    - Save this to a secure location, you will not be able to view this after the window is closed.

CLI Authentication:
- `docker login -u <YOUR_USERNAME>`
  - At the password prompt, enter your Personal Access Token.

## Pushing your image to your repo
- `docker push wamski/wasky-ceg3120`

## My docker repo:
https://hub.docker.com/repository/docker/wamski/wasky-ceg3120/general

## Resources
- Running a Dockerfile: `docker run --help`
  - Also for running shell commands `sh -c ""`: https://docs.docker.com/reference/cli/docker/container/exec/
- Pushing a docker image: https://docs.docker.com/get-started/introduction/build-and-push-first-image/