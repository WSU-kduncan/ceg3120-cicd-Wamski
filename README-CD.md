<img align="left" alt="AWS" width="60px" style="padding-right:10px;" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/docker/docker-original-wordmark.svg" />
<h1 align="left">Project 5: Docker Continuous Integration with tags</h1>
<br/>

## GitHub Tags
In your Github repository there is a drop down that typically shows the different
 branches on your repo. This also shows the different tags, in this dropdown, the second 
header shows the different tags associated with the repository.
- You can create a tag with a commit by doing the following:
  1. Create a commit
        ```shell
        git add file/path
        git commit -m "your commit message"
        git tag -a v#.#.# -m "Your tag description"    
        ```
     This will tag your commit, an example tag would look like: `v1.0.0`
  2. Pushing your changes and tag to GitHub is similar to how you would push a branch.
        ```shell
        git push origin v#.#.#
        ```
     
## GitHub workflow for Semantic Versioning
My workflow builds my image and then pushes to DockerHub under 3 different tags `latest, major, minor`.
 This is only done when a new tag to commited to the repo.  
EX:
```
:latest
:1 
:1.0
```
- The 1 is the major version with the 1.0 being the minor version.

### Workflow steps:
1. The workflow sets up QEMO and Buildx so that the image can be built for both arm64 and amd64.
2. The workflow uses my username and personal access token stored in my repos secrets to login to dockerhub.
3. The workflow builds and pushes the 3 different tags for both amd64 and arm64.

If this workflow was to be used in a different repository the images field under `Grab MetaData` would need to change
```yaml
# From:
with:
  images: ${{ secrets.DOCKER_USERNAME }}/wasky-ceg3120

#To:
with:
  images: ${{ secrets.DOCKER_USERNAME }}/repo-name-here
```
My workflow: https://github.com/WSU-kduncan/ceg3120-cicd-Wamski/blob/main/.github/workflows/DockerPush.yml.old

### Checking if the workflow completed:
1. Go to your Docker Hub repo and see if there are now 3 tags instead of just latest.
2. Try to `docker pull` the different tags
```shell
docker pull wamski/wasky-ceg3120:latest
docker pull wamski/wasky-ceg3120:1
docker pull wamski/wasky-ceg3120:1.0
```
3. Then make sure each image is able to start up.
```shell
docker run -it -p 5001:4200 wamski/wasky-ceg3120:latest
docker run -it -p 5001:4200 wamski/wasky-ceg3120:1
docker run -it -p 5001:4200 wamski/wasky-ceg3120:1.0
```

## Resources
- Adding tags to workflow: https://docs.docker.com/build/ci/github-actions/manage-tags-labels/
- Git Tag: https://graphite.dev/guides/git-push-tag
- Major/Minor: https://github.com/docker/metadata-action
- Trigger Rule: In class demonstration of webhooks (Lecture Date: 04/25/2025)