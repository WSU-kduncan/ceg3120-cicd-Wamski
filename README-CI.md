<img align="left" alt="AWS" width="60px" style="padding-right:10px;" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/docker/docker-original-wordmark.svg" />
<h1 align="left">Project 4: Docker Images and Repos</h1>
<br/>

# Project Objective:
<img align="right" alt="Diagram" width=50% src="/Images/CI.png">

This project sets of continuous integration of the code on the GitHub repo's main brain to build and push to 
DockerHub. Using GitHub actions and workflows this enables the automation of these tasks.
### Tools:
- GitHub actions / Workflows
  - These automate the process of building a dockerfile and pushing the resulting image to DockerHub.
    - docker/login-action@v3: Logs into DockerHub using your username and personal access token.
    - docker/build-push-action@v6: Builds the project based on the dockerfile and pushes the result to DockerHub.
- DockerHub
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
- In your CLI, you should see a line that looks similar to `✔ Compiled successfully.`
  

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

## Setting up your Github Repository Secrets
- First create a Personal access token with read and write permissions similar to how
one was created for logging into the CLI. Give this a name of `Github Actions` so that
 if you ever need to remove the key, you know which one to delete.
  - Once all of this is set, select generate and copy the key.
- Next, go to your Github repo settings and scroll down to secrets and variables, once in the Actions secrets 
you will see a green button labeled; `New Repository Secret`
  - You will need to create two named; "DOCKER_TOKEN" and "DOCKER_USERNAME"
    - In the "DOCKER_TOKEN" paste in the key that you generated with read and write.
    - In the "DOCKER_USERNAME" write in your docker username.
  - Both of these will be used in a workflows file to build and push the app within this repo to Docker Hub.

## Setting up Github Workflows to push this app image
The workflow I created runs every time a commit is pushed to the main branch. This will rebuild the image of the angular app
and build for both amd64 linux and arm64.
1. In the root directory of your repos add the following file with the corresponding PATH.
    - `.github/workflows/DockerPush.yml`
      - `.github` is a directory that Github will read for different actions and commands
      - `workflows` is a directory where you can write differnt Github actions that can be preformed based upon a 
certain event such as `push`.
      - `DockerPush.yml` is the file that will build and push your image to DockerHub.
2. In the `DockerPush.yml`
   - The work flow will use both QEMU and Buildx. Then using the previously set up `DOCKER_USERNAME` and `DOCKER_TOKEN`
 the Github action will login DockerHub. After logging in, the workflow will attempt to build the image for both AMD54 linux 
and ARM64 linux. The work flow will then push to your Dockerhub repo.
     - If this project is recreated in a different Github repo, the following highlighted portions will need to change:
       - context: `Path/to/your/app`
       - file: `Path/to/your/Dockerfile`
       -  tags: ${{ secrets.DOCKER_USERNAME }}/`Your Repo`:latest
My Workflow: https://github.com/WSU-kduncan/ceg3120-cicd-Wamski/blob/main/.github/workflows/DockerPush.yml

## Testing your workflow
Once your workflow is completed, go over to your docker hub repo and see when the last change was made. If the change is within
a few moments of the Github action completed then the github action is working properly.
- To further test this you could run `docker pull username/repo:latest` and run the image. 
  - My docker pull: `docker pull wamski/wasky-ceg3120:latest`
- Then run the pulled image `docker run -it -p 5001:4200 wamski/wasky-ceg3120`

## Resources
- Running a Dockerfile: `docker run --help`
  - Also for running shell commands `sh -c ""`: https://docs.docker.com/reference/cli/docker/container/exec/
- Pushing a docker image: https://docs.docker.com/get-started/introduction/build-and-push-first-image/
- Docker Actions Login: https://github.com/marketplace/actions/docker-login
- Docker Build and Push (GitHub Actions): https://docs.docker.com/build/ci/github-actions/secrets/
- Docker Build and Push Working directory (Context: https://stackoverflow.com/questions/69918178/is-there-a-way-to-set-path-for-docker-build-with-the-github-action-plugin-docker
- Docker image multi-platform: https://docs.docker.com/build/ci/github-actions/multi-platform/