# nvidia-gpu-monitoring
repository for nvidia gpu monitoring environment

## nvidia dcgm(Data Center GPU Manager) build
```
# get dcgm git src
git clone https://github.com/NVIDIA/dcgm-exporter.git

# move directory
cd ./dcgmbuild

# run build script
./build.sh
```

## nvidia gpu monitoring in k8s
https://docs.kakaocloud.com/tutorial/observability/nvidia-gpu-monitoring

## nvidia-docker
https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/index.html

# How to Install nvidia container toolkit and Run dcgm-exporter as docker
```
# preliquisite
nvidia-smi

# 1. Add Repository for nvidia container toolkit 
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | \
sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg
curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list | \
sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list

# 2. Install nvidia container toolkit
sudo apt-get update
sudo apt-get install -y nvidia-container-toolkit

# 3. Set nvidia runtime as docker
sudo nvidia-ctk runtime configure --runtime=docker

# 4. Docker restart
sudo systemctl restart docker

# 5. Check gpu runtime
docker info | grep -i runtime
# expected result
# Runtimes: nvidia runc
# Default Runtime: runc

# 6. Run dcgm-exporter as docker
docker run -d --gpus all -p 9400:9400 nvcr.io/nvidia/k8s/dcgm-exporter:4.4.1-4.6.0-ubuntu22.04
```
