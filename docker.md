# DOCKER NOTES


### images
- list images on a host `docker images`
- build an image `docker build ./path/to/dir/containing/Dockerfile`
- build an image with a specific name `docker build -t my_image_name .`
- build an image with a specific name and tag `docker build -t my_image_name:my_tag .` to confirm creation you can do `docker images`
- remove an image `docker image rm my_image_name:my_tag`


### containers
- list containers _running_ on a host `docker ps`
- list all containers on a host `docker ps -a`
- run a docker image as a container `docker run --name my_cont_name my_img_name:my_tag` (assumes `latest` tag by default)
- remove a container `docker rm my_cont_name`
- check the logs of a container 'docker logs my_cont_name'
- check the logs of a container as a stream 'docker logs -f my_cont_name'
- start a container `docker start mycontainer`
- stop a container `docker stop mycontainer`
- for a django project `docker run -p 8000:8000 -d mycontname` (note the `-d` is to detach the process from the command line)
- open a shell into a container: `docker exec -it MY_CONT_NAME [bash|sh]` 

