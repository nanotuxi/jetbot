# Docker

???+ caution
    If you are using a 3rd party JetBot kit, depending on the kit, it may require a customized software setup specific to the kit.

    Please check the manufacture's set up instruction.

In addition to the pre-built SD card image, we also provide a docker container
in case you want to install JetBot on an existing Jetson Nano SD card.

???+ hint
    For this, we'll assume you've set up your Jetson Nano using the **online Getting Started guide**.
        
     - [Getting Started With Jetson Nano Developer Kit](https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit)

### Step 1 - Configure System

First, call the [``scripts/configure_jetson.sh``](https://github.com/NVIDIA-AI-IOT/jetbot/blob/master/scripts/configure_jetson.sh) script to configure Jetson to opitimal configuration for Jetson operation.

```bash
cd jetbot
./scripts/configure_jetson.sh
```

???+ info
    To bring back the GUI, run `./scripts/re_enable_gui.sh`.

Optionally, if you have not set up swap, you can create swap with this script.<br>
We recommend having 4GB swap.

```bash
./script/enable_swap.sh
```


Next, source the [``docker/configure.sh``](https://github.com/NVIDIA-AI-IOT/jetbot/blob/master/docker/configure.sh) script to configure various environment variables related to JetBot docker.

```bash
cd docker
source configure.sh
```

Finally, if you haven't already, set the default docker runtime to NVIDIA using [``docker/set_nvidia_runtime.sh``](https://github.com/NVIDIA-AI-IOT/jetbot/blob/master/docker/set_nvidia_runtime.sh).  This is needed to use
CUDA related components with the containers.

```bash
./set_nvidia_runtime.sh
```

If needed, you can also set memory limits on the Jupyter container.

```bash
export JETBOT_JUPYTER_MEMORY=500m
export JETBOT_JUPYTER_MEMORY_SWAP=3G
```

### Step 2 - Enable all containers

Call the following to enable the JetBot docker containers 

```bash
sudo systemctl enable docker   # enable docker daemon at boot
./enable.sh $HOME   # we'll use home directory as working directory, set this as you please.
```

Now you can go to ``https://<jetbot_ip>:8888`` from a web browser and start programming JetBot!
You can do this from any machine on your local network.  The password to log in is ``jetbot``.

![](https://user-images.githubusercontent.com/25759564/92091965-51ae4f00-ed86-11ea-93d5-09d291ccfa95.png)


???+ note
    The directory you specify to ``./enable.sh`` will be mounted as a volume in the jupyter container at the location ``/workspace``.  This means the work you in the ``/workspace`` folder inside container is saved.  This is set to the root directory of Jupyter Lab.  Please note, if you work outside of that directory it will be lost when the container shuts down.
