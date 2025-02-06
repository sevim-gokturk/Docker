# Docker Image Basic Operations


## Part 1 - Launch a Docker Machine Instance, Connect with SSH and  Install Docker on Amazon Linux 2 EC2 Instance

- Launch a Docker machine on Amazon Linux 2 AMI with security group allowing SSH connections .(SSH 22 Anywhere, HTTP 80 Anywhere & Custom TCP Kalsin 8080 Anywhere)

- Connect to your instance with SSH.

```bash
ssh -i "key.pem" ec2-user@ec2-1-234-567-89.us-east-2.compute.amazonaws.com
```

- Update the installed packages and package cache on your instance.

```bash
sudo yum update -y
```

- Install the most recent Docker Community Edition package.

```bash
sudo amazon-linux-extras install docker -y
```

- Start docker service.

```bash
sudo systemctl start docker
```

- Enable docker service so that docker service can restart automatically after reboots.

```bash
sudo systemctl enable docker
```

- Check if the docker service is up and running.

```bash
sudo systemctl status docker
```

- Add the `ec2-user` to the `docker` group to run docker commands without using `sudo`.

```bash
sudo usermod -a -G docker ec2-user
```

- Normally, the user needs to re-login into bash shell for the group `docker` to be effective, but `newgrp` command can be used activate `docker` group for `ec2-user`, not to re-login into bash shell.

```bash
newgrp docker
```


## Part 2 - Using Docker Image Commands and Docker Hub

- Sign up to Docker Hub and explain the UI.

- Create a repository with the name of `flask-app` and description of `This image repo holds Flask apps.`.

- Check if the docker service is up and running on EC2 instance.

```bash
systemctl status docker
```

- List images in Docker and explain properties of images.

```bash
docker image ls
docker images
```

- Download Docker image `ubuntu` and explain image tags (defaults to `latest`) on Docker Hub. Show `ubuntu` repo on Docker Hub and which version the `latest` tag corresponds to (`20.04`).

```bash
# Default to ubuntu:latest
docker image pull ubuntu
docker image ls
```

- Run `ubuntu` as container with interactive shell open.

```bash
docker run -it ubuntu
```

- Display the `ubuntu` os info on the container (`VERSION="24.04.1 LTS (Noble Numbat)`) and note that the `latest` tag is showing release `20.04` as in the Docker Hub. Then exit the container.

```bash
cat /etc/os-release
exit
```

- Download earlier version (`18.04`) of `ubuntu` image, which is tagged as `18.04` on Docker Hub and explain image list.

```bash
docker image pull ubuntu:18.04
docker image ls
```

- Inspect `ubuntu` image and explain properties.

```bash
# Default to ubuntu:latest
docker image inspect ubuntu
# Ubuntu with tag 18.04
docker image inspect ubuntu:18.04
```

- Search for Docker Images both on `bash` and on Docker Hub. 
  
```bash
docker search ubuntu
```

## Part 3 - Building Docker Images with Dockerfile

- Build an image of Python Flask app with Dockerfile based on `python:alpine` image and push it to the Docker Hub.

- Create a folder to hold all files necessary for creating Docker image.

```bash
mkdir sevim_web
cd sevim_web
```

- Create application code and save it to file, and name it `welcome.py`

```bash
from flask import Flask
app = Flask(__name__)
@app.route("/")
def hello():
    return "<h1>Welcome to Docker Learners</h1>"
if __name__ == "__main__":
    app.run(host="0.0.0.0", port=80)
```

- Create a Dockerfile listing necessary packages and modules, and name it `Dockerfile`.

```bash
nano Dockerfile
```

```Dockerfile
FROM ubuntu
RUN apt-get update -y
RUN apt-get install python3 -y
RUN apt-get install python3-pip -y
RUN pip3 install flask
COPY . /app
WORKDIR /app
CMD python3 ./welcome.py
```

```bash
docker build -t <dockerhubusername>/flask-app:1.0 . 
docker image ls
```

```bash
docker run -d --name welcome -p 80:80 <dockerhubusername>/flask-app:1.0
```

- Login in to Docker with credentials.

```bash
docker login
```

- Push newly built image to Docker Hub, and show the updated repo on Docker Hub.

```bash
docker push <dockerhubusername>/flask-app:1.0
```

`Dockerfile-alpine`

- This time, we reduce the size of image.

- Create a Dockerfile listing necessary packages and modules, and name it `Dockerfile-alpine`
  
```Dockerfile
FROM python:alpine
RUN pip install flask
COPY . /app
WORKDIR /app
EXPOSE 80
CMD python ./welcome.py
```

- Build Docker image from Dockerfile locally, tag it as `<Your_Docker_Hub_Account_Name>/<Your_Image_Name>:<Tag>` and explain steps of building. Note that repo name is the combination of `<Your_Docker_Hub_Account_Name>/<Your_Image_Name>`.

```bash
docker build -t "<dockerhubusername>/flask-app:2.0" -f ./Dockerfile-alpine . 
docker image ls
```

- Note that while the size of `<dockerhubusername>/flask-app:1.0` is approximately 400MB, the size of `sevim/flask-app:2.0` is 59.7MB.

- Run the newly built image as container in detached mode, connect host `port 80` to container `port 80`, and name container as `welcome`. Then list running containers and connect to EC2 instance from the browser to show the Flask app is running.

```bash
docker run -d --name welcome2 -p 8080:80 <dockerhubusername>/flask-app:2.0
```

- Stop and remove the container `welcome`.

```bash
docker stop welcome && docker rm welcome
```

- Push newly built image to Docker Hub, and show the updated repo on Docker Hub.

```bash
docker push <dockerhubusername>/flask-app:2.0
```

- We can also tag the same image with different tags.

```bash
docker image tag <dockerhubusername>/flask-app:2.0 sevim/flask-app:latest
```

- Delete image with `image id` locally.

```bash
docker image rm 497
```