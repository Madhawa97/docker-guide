# Docker Guide

## Building the App's Container Image
In order to build the application, we need to use a `Dockerfile`. A Dockerfile is simply a text-based script of instructions that is used to create a container image. If you've created Dockerfiles before, you might see a few flaws in the `Dockerfile` below. 

Create a file named Dockerfile in the same folder as the file `package.json` with the following contents.

```docker
FROM node:12-alpine
# Adding build tools to make yarn install work on Apple silicon / arm64 machines
RUN apk add --no-cache python2 g++ make
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
```

Now build the container image using the docker build command.

```shell
docker build -t getting-started .
```
The . at the end of the docker build command tells that Docker should look for the Dockerfile in the current directory.

## Starting an App Container
Now that we have an image, let's run the application.
```shell
docker run -dp 3000:3000 getting-started
```

## Rebuilding the app after updating code

We have to replace our Old Container.
To remove a container, it first needs to be stopped. Once it has stopped, it can be removed. We have two ways that we can remove the old container. Feel free to choose the path that you're most comfortable with.

### Removing a container using the CLI
- Get the ID of the container by using the docker ps command.
```shell
docker ps
```
- Use the docker stop command to stop the container.
Swap out <the-container-id> with the ID from docker ps
```shell
docker stop <the-container-id>
```
- Once the container has stopped, you can remove it by using the docker rm command.
```shell
docker rm <the-container-id>
```
Now we can start the updated app using `docker run -dp 3000:3000 getting-started` command.

## Sharing the App
For this we have to use a Docker registry. The default registry is Docker Hub.

### Create a Repo
To push an image, we first need to create a repo on Docker Hub.

Go to Docker Hub and Click the Create Repository button. For the repo name, use `getting-started`. Make sure the Visibility is Public.

If you look on the right-side of the page, you'll see a section named Docker commands. This gives an example command that you will need to run to push to this repo.

## Docker command with push example

### Pushing our Image
In the command line, try running the push command you see on Docker Hub. Note that your command will be using your namespace, not "docker".

**We need to "tag" our existing image we've built to give it another name.**

Use the docker tag command to give the getting-started image a new name. Be sure to swap out YOUR-USER-NAME with your Docker ID.
```shell
docker tag getting-started YOUR-USER-NAME/getting-started
```
Now try your push command. If you're copying the value from Docker Hub, you can drop the tagname portion, as we didn't add a tag to the image name. If you don't specify a tag, Docker wi use a tag called latest.
```shell
docker push YOUR-USER-NAME/getting-started
```

## Running our Image on a New Instance

Open your browser to Play with Docker.

Once you're logged in, click on the "+ ADD NEW INSTANCE" link in the left side bar. (If you don't see it, make your browser a little wider.) After a few seconds, a terminal window will be opened in your browser.

In the terminal, start your freshly pushed app.
```shell
docker run -dp 3000:3000 YOUR-USER-NAME/getting-started
```

You should see the image get pulled down and eventually start up!

Click on the 3000 badge when it comes up and you should see the app with your modifications! Hooray! If the 3000 badge doesn't show up, you can click on the "Open Port" button and type in 3000.
