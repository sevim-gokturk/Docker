# Handling Docker Volumes

Purpose of the this hands-on training is to teach students how to handle volumes in Docker containers.

## Learning Outcomes 

At the end of the this hands-on training, students will be able to;

- explain what Alpine container is and why it is widely used.

- list available volumes in Docker.

- create a volume in Docker.

- inspect properties of a volume in Docker.

- locate the Docker volume mount point.

- attach a volume to a Docker container.

- attach same volume to different containers.

- delete Docker volumes.

## Outline

- Part 1 - Launch a Docker Machine Instance and Connect with SSH

- Part 2 - Data Persistence in Docker Containers

- Part 3 - Managing Docker Volumes

- Part 4 - Using Same Volume with Different Containers


## Part 1 - Launch a Docker Machine Instance and Connect with SSH

- Launch a Docker machine on Amazon Linux 2 AMI with security group allowing SSH connections using the [Cloudformation Template for Docker Machine Installation](../S1A-docker-01-installing-on-ec2-linux2/docker-installation-template.yml).

- Connect to your instance with SSH.

```bash
ssh -i .ssh/call-training.pem ec2-user@ec2-3-133-106-98.us-east-2.compute.amazonaws.com
```

## Part 2 - Data Persistence in Docker Containers

- Check if the docker service is up and running.

```bash
systemctl status docker
```

- Run an `alpine` container with interactive shell open, and add command to run alpine shell. Here, explain explain what the alpine container is and why it is so popular. (Small size, Secure, Simple, Fast boot)

```bash
docker run -it alpine ash
```

- Display the os release of the alpine container.

```bash
cat /etc/os-release
```

- Create a file named `short-life.txt` under `/home` folder

```bash
cd home && touch short-life.txt && ls
```

- Exit the container and return to ec2-user bash shell.

```bash
exit
```

- Show the list of all containers available on Docker machine.

```bash
 docker image ls
docker ps -a
```

- Start the alpine container and connect to it.

```bash
docker start 737 && docker exec -it 737 ash
```

- Show that the file `short-life.txt` is still there, and explain why it is there. (Container holds it data until removed).

```bash
ls
cd home
ls /home 
```

- Exit the container and return to ec2-user bash shell.

```bash
exit
```

- Remove the alpine container.

```bash
docker rm -f 737
```

- Show the list of all containers, and the alpine container is gone with its data.

```bash
docker ps -a
```

## Part 3 - Managing Docker Volumes

- Explain why we need volumes in Docker.

- List the volumes available in Docker, since not added volume before list should be empty.

```bash
docker volume ls
```

- Create a volume named `cw-vol`.

```bash
docker volume create cw-vol
```

- List the volumes available in Docker, should see local volume `cw-vol` in the list.

```bash
docker volume ls
```

- Show details and explain the volume `cw-vol`. Note the mount point: `/var/lib/docker/volumes/cw-vol/_data`.

```bash
docker volume inspect cw-vol
```

- List all files/folders under the mount point of the volume `cw-vol`, should see nothing listed.

```bash
sudo ls -al  /var/lib/docker/volumes/cw-vol/_data
```

- Run a `alpine` container with interactive shell open, name the container as `sevim`, attach the volume `cw-vol` to `/cw` mount point in the container, and add command to run alpine shell. Here, explain `--volume` and `v` flags.

```bash
docker run -it --name sevim -v cw-vol:/cw alpine ash
```

- List files/folder in `sevim` container, show mounting point `/cw`, and explain the mounted volume `cw-vol`.

```bash
ls
```

- Create a file in `sevim` container under `/cw` folder.

```bash
cd cw && echo "This file is created in the container sevim" > i-will-persist.txt
```

- List the files in `/cw` folder, and show content of `i-will-persist.txt`.

```bash
ls && cat i-will-persist.txt
```

- Exit the `sevim` container and return to ec2-user bash shell.

```bash
exit
```

- Show the list of all containers available on Docker machine.

```bash
docker ps -a
```

- Remove the `sevim` container.

```bash
docker rm sevim
```

- Show the list of all containers, and the `sevim` container is gone.

```bash
docker ps -a
```

- List all files/folders under the volume `cw-vol`, show that the file `i-will-persist.txt` is there.

```bash
sudo ls -al  /var/lib/docker/volumes/cw-vol/_data && sudo cat /var/lib/docker/volumes/cw-vol/_data/i-will-persist.txt
```

## Part 4 - Using Same Volume with Different Containers
ayn volm frk contr kllm
- Run a `alpine` container with interactive shell open, name the container as `sevim2nd`, attach the volume `cw-vol` to `/cw2nd` mount point in the container, and add command to run alpine shell.

```bash
docker run -it --name sevim2nd -v cw-vol:/cw2nd alpine ash
```

- List the files in `/cw2nd` folder, and show that we can reach the file `i-will-persist.txt`.

```bash
ls -l /cw2nd && cat /cw2nd/i-will-persist.txt
```

- Create an another file in `sevim2nd` container under `/cw2nd` folder.

```bash
cd cw2nd && echo "This is a file of the container sevim2nd" > loadmore.txt
```

- List the files in `/cw2nd` folder, and show content of `loadmore.txt`.

```bash
ls && cat loadmore.txt
```

- Exit the `sevim2nd` container and return to ec2-user bash shell.

```bash
exit
```

- Run a `ubuntu` container with interactive shell open, name the container as `sevim3rd`, attach the volume `cw-vol` to `/cw3rd` mount point in the container, and add command to run bash shell.

```bash
docker run -it --name sevim3rd -v cw-vol:/cw3rd ubuntu bash
```

- List the files in `/cw3rd` folder, and show that we can reach the all files created earlier.

```bash
ls -l /cw3rd
```

- Create an another file in `sevim3rd` container under `/cw3rd` folder.

```bash
cd cw3rd && touch file-from-3rd.txt && ls
```

- Exit the `sevim3rd` container and return to ec2-user bash shell.

```bash
exit
docker ps -a
docker image ls
```

- Run an another `ubuntu` container with interactive shell open, name the container as `sevim4th`, attach the volume `cw-vol` as read-only to `/cw4th` mount point in the container, and add command to run bash shell.

```bash
docker run -it --name sevim4th -v cw-vol:/cw4th:ro ubuntu bash
```

- List the files in `/cw4th` folder, and show that we can reach the all files created earlier.

```bash
ls -l /cw4th
```

- Try to create an another file under `/cw4th` folder. Should see error `read-only file system`

```bash
cd cw4th && touch file-from-4th.txt
```

- Exit the `sevim4th` container and return to ec2-user bash shell.

```bash
exit
```

- List all containers.

```bash
sudo ls -al  /var/lib/docker/volumes/cw-vol/_data
docker ps -a
```

- Delete `sevim2nd`, `sevim3rd` and `sevim4th` containers.

```bash
docker rm sevim2nd sevim3rd sevim4th
```

- Delete `cw-vol` volume.

```bash
docker volume rm cw-vol
```
