Trevor Paddon 
9 April 2023
Projectt5

Part 1
```
Project 5 Overview: For this project we are starting to practice semantic versioning. What this does is you add a tag whenever you push a new version so that
you can keep track of whatever the latest version is. We are also working on deployment. Deployment allows you to build and release your projects to your 
destination of choice. It also allows for an easy rollback if you need to revert to a previous version. 

Git Tag: to generate a git tag you will use the command 'git tag version' so for me it was git tag 1.0.0

GitHub Workflow: In my workflow I added a tags section as well as docker/metadata-action. This will trigger the workflow when a new tag is pushed as well as
generating a set of tags based on the tag name, commit hash, and the latest tag. My workflow now looks like this:

name: Docker Image CD

on:
  push:
    tags:
      - '*'
  pull_request:
    branches: [ "master" ]

jobs:

  build-and-push:

    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Get Git tag name
        id: git-tag
        run: echo ::set-output name=TAG_NAME::$(git describe --tags --abbrev=0)

      - name: Get Git commit hash
        id: git-commit
        run: echo ::set-output name=COMMIT_HASH::$(git rev-parse HEAD)

      - name: Generate Docker tags
        id: docker-tags
        uses: docker/metadata-action@v3
        with:
          images: docker.io/<trevorpaddon>/<project5>
          tags: |
            latest
            ${{ steps.git-tag.outputs.TAG_NAME }}
            ${{ steps.git-commit.outputs.COMMIT_HASH }}

      - name: Build and push Docker image
        uses: docker/build-push-action@v2
        with:
          context: .
          file: ./Dockerfile
          push: true
          tags: ${{ steps.docker-tags.outputs.tags }}

The link to my repository is https://hub.docker.com/repository/docker/trevorpaddon/project5/general
```

Part 2 

```
Downloading Docker: To download docker on my instance, I did the following:
1. sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg
2. sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
3. echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
4. sudo apt-get update
5. sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
6. sudo docker run hello-world. 

Container Restart: I added code for an automatic container restart because this will keep the important applications running even if my script fails. To do 
this I created a file called restartcontainer.sh and added the following code: 

#!/bin/bash
container_name="my-container"
docker ps -a | grep $mycontainer | awk '{print $1}' | xargs docker restart

I then made the file executable with the command chmod +x restartcontainer.sh.


Installing adnanh Webhook: To install this I ran sudo apt-get install webhook. I then made a file called webhook.json with the following code:
{
"id": "my-webhook",
"execute-command": "/usr/local/bin/restart-container.sh",
"command-working-directory": "/usr/local/bin/",
"response-message": "Container restarted successfully"
}
To restart I ran the command sudo service webhook.service restart.

Webhook Task File: The webhook task specification file explains how to transfer data to another application when an event or trigger happens in one 
application. This can be used for a number of things, like automating processes between programs or informing users of significant events. If someone wanted 
to access your file on your system, I would put it in my etc/systemd/system directory.
```

Part 3 
```
![image](https://user-images.githubusercontent.com/77801382/231638675-a40e27b3-db0f-45d2-b8a2-d45d501cb350.png)

```
