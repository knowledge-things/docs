# PyTorch Container for Jetson and JetPack

`sudo apt-get install nvidia-jetpack`

## Running the Container

First pull one of the `l4t-pytorch` container tags from above, corresponding to the version of JetPack-L4T that you have installed on your Jetson. For example, if you are running the latest JetPack 5.1 (L4T R35.2.1) release:

```
sudo docker pull nvcr.io/nvidia/l4t-pytorch:r35.2.1-pth2.0-py3
```

Then to start an interactive session in the container, run the following command:

```
sudo docker run -it --rm --runtime nvidia --network host nvcr.io/nvidia/l4t-pytorch:r35.2.1-pth2.0-py3
```

You should then be able to start a Python3 interpreter and `import torch` and `import torchvision`.

## Mounting Directories from the Host Device

To mount scripts, data, ect. from your Jetson's filesystem to run inside the container, use Docker's [`-v` flag](https://docs.docker.com/storage/bind-mounts/) when starting your Docker instance:

```bash
sudo docker run -it --rm --runtime nvidia --network host -v /home/user/project:/location/in/container nvcr.io/nvidia/l4t-pytorch:r35.2.1-pth2.0-py3
```

Note that if you want to use dual CSI cameras inside of container, you should include the following flags in your `docker run`command:

```bash
--volume /tmp/argus_socket:/tmp/argus_socket \
--device /dev/video0 \
--device /dev/video1
```