# Folding@Home Docker

This is my personal brach of [yurinnick's F@H container](https://github.com/yurinnick/folding-at-home-docker). I couldn't get it to recognize my nVidia GPU so I took some steps to fix that.

Further documentation pending, I'm sharing this as-is for now. Link to public container here: https://hub.docker.com/repository/docker/thebozzcl/folding-at-home

## Usage

### requirements

Keep in mind you can still run the container with no GPU support! Still, there's other lighter containers (like the one I based this on) you could use instead.

1. This has only been tested on Pop!\_OS, which is basically Ubuntu 19.10. Other similar distros should work. No idea if this will work on Mac OS or Windows.
2. This has only been tested on nVidia GPUs. I have no idea how to set this up for AMD GPUs... but I assume you will need to use OpenCL and a different base image than mine. Feel free to branch and figure it out!
1. Update your nVidia drivers to the latest version.
2. (Optional, but should help with debugging) Install the latest version of the CUDA toolkit: https://developer.nvidia.com/cuda-downloads
3. Install the nvidia Container Toolkit: https://github.com/NVIDIA/nvidia-docker
4. Test that everything works by running `docker run --gpus all nvidia/cuda:10.0-base nvidia-smi`. If it works, the container should be able to see your GPU.

### docker

```
docker run --gpus all \
  --name folding-at-home \
  -p 7396:7396 \
  -p 36330:36330 \
  -e USER=Anonymous \
  -e TEAM=0 \
  -e ENABLE_GPU=true \
  -e ENABLE_SMP=true \
  --restart unless-stopped \
  --allow 0/0 \
  --web-allow 0/0 \
  thebozzcl/folding-at-home
```

## Parameters

- USER - Folding@home username (default: Anonymous)
- TEAM - Foldinghome team number (default: 0)
- PASSKEY - [optional] Folding@home [passkey](https://apps.foldingathome.org/getpasskey)
- ENABLE_GPU - Enable GPU compute (default: false)
- ENABLE_SMP - Enable auto-configuration of SMP slots (default: true)

Additional configuration parameters can be passed as command line arguments. To get the full list of parameters run:

```
docker run thebozzcl/folding-at-home:latest --help
```
