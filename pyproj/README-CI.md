Trevor Paddon
25 March 
Project 4

Part 1 
1. In order to download docker, I went to the docker website and downloaded docker for windows.
2. To build the image from the docker file I used the command: 
FROM nginx

COPY website/ /usr/share/html/
3. To run the container I ran docker run -p 8080:80 image-name.
4. In order to view the project running in the container I typed http://localhost:8080 into my google search bar.
Part 2
1. In order to create a public repo in Dockerhub you first have to create an account as well as login to said account. Next you create your new repository by clicking on the 'new repository' button and selecting the 'public' option.
2. To login to docker you need to use the docker login -u command. It will ask you for a password, and this is the token that I created on DockerHub. 
3. To push images to dockerhub you use the command: docker tag image-name trpaddon/3120-cicd:latest.
4. To create the secrets on github I scrolled down after selecting settings to the secrets and variables tab and selected 'New Repository Secret' and made a secret that contained my username and one that contained my password for dockerhub.
5. Github Work flow is basically a set of steps that is triggered when a specific command is entered. 
Part 3

