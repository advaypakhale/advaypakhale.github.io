---
date:
  created: 2025-09-21
readtime: 5
# pin: true
tags:
  - setup
---

# NVIDIA development environment setup on Ubuntu

Because NVIDIA likes to nuke its own drivers on my system every few months.
<!-- more -->

## Nuking everything NVIDIA from existence
```bash
dpkg -l | grep nvidia | awk '{print $2}' | xargs sudo dpkg --force-all -P; sudo apt --fix-broken install
```

## Installing the drivers
```bash
sudo ubuntu-drivers install
```

## Installing CUDA Toolkit
Follow the instructions [here](https://developer.nvidia.com/cuda-downloads?target_os=Linux).

## Installing `nvidia-container-toolkit`
The instructions from [here](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#installing-with-apt) are replicated below.

```bash
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
  && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
# optionally, run:
# sed -i -e '/experimental/ s/^#//g' /etc/apt/sources.list.d/nvidia-container-toolkit.list
sudo apt-get update
export NVIDIA_CONTAINER_TOOLKIT_VERSION=1.17.8-1
  sudo apt-get install -y \
      nvidia-container-toolkit=${NVIDIA_CONTAINER_TOOLKIT_VERSION} \
      nvidia-container-toolkit-base=${NVIDIA_CONTAINER_TOOLKIT_VERSION} \
      libnvidia-container-tools=${NVIDIA_CONTAINER_TOOLKIT_VERSION} \
      libnvidia-container1=${NVIDIA_CONTAINER_TOOLKIT_VERSION}
```

## Configuring `nvidia-container-toolkit` for docker
```bash
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker
```

And we are hopefully done.

I hope the day comes that I never have to do this again. Till then, [this](https://youtu.be/iYWzMvlj2RQ?si=u8nVWI6sDA28pZA0).
