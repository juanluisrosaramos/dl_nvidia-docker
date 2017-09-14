
# Deep Learning Docker image
## Cloned and modified from https://github.com/floydhub/dl-docker
My intention is to build a docker image (in this case a nvidia-docker) that contains all the software necessary for my thesis project. I found this super complete image from FloydHub butt I want to modify it with just what I need, as I am not comfortable with 

### How to build my image?
git clone https://github.com/juanluisrosaramos/dl_nvidia-docker.git
cd dl_nvidia-docker

**GPU Version**
docker build -t floydhub/caffe-docker:gpu -f Dockerfile.gpu .

### How to launch my container?
nvidia-docker run -it -p 8888:8888 -p 6006:6006 -v /sharedfolder:/root/sharedfolder floydhub/caffe-docker:gpu bash
>>>>>>> f0e59f656c830892bacf128dbc84067e9a0d716a
Note the use of `nvidia-docker` rather than just `docker`

| Parameter      | Explanation |
|----------------|-------------|
|`-it`             | This creates an interactive terminal you can use to iteract with your container |
|`-p 8888:8888 -p 6006:6006`    | This exposes the ports inside the container so they can be accessed from the host. The format is `-p <host-port>:<container-port>`. The default iPython Notebook runs on port 8888 and Tensorboard on 6006 |
|`-v /sharedfolder:/root/sharedfolder/` | This shares the folder `/sharedfolder` on your host machine to `/root/sharedfolder/` inside your container. Any data written to this folder by the container will be persistent. You can modify this to anything of the format `-v /local/shared/folder:/shared/folder/in/container/`. See [Docker container persistence](#docker-container-persistence)
|`floydhub/dl-docker:cpu`   | This the image that you want to run. The format is `image:tag`. In our case, we use the image `dl-docker` and tag `gpu` or `cpu` to spin up the appropriate image |
|`bash`       | This provides the default command when the container is started. Even if this was not provided, bash is the default command and just starts a Bash session. You can modify this to be whatever you'd like to be executed when your container starts. For example, you can execute `docker run -it -p 8888:8888 -p 6006:6006 floydhub/dl-docker:cpu jupyter notebook`. This will execute the command `jupyter notebook` and starts your Jupyter Notebook for you when the container starts

### How do I update/install new libraries?
You can do one of:

1. Modify the `Dockerfile` directly to install new or update your existing libraries. You will need to do a `docker build` after you do this. If you just want to update to a newer version of the DL framework(s), you can pass them as CLI parameter using the --build-arg tag ([see](-v /sharedfolder:/root/sharedfolder) for details). The framework versions are defined at the top of the `Dockerfile`. For example, `docker build -t floydhub/dl-docker:cpu -f Dockerfile.cpu --build-arg TENSORFLOW_VERSION=0.9.0rc0 .`

2. You can log in to a container and install the frameworks interactively using the terminal. After you've made sure everything looks good, you can commit the new contains and store it as an image
