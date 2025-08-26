# Deepstream-5.1-on-a-Tesla-P4
Exact dependencies to install deepstream 5.1 on a Tesla P4 GPU

Nvidia Drivers 460
CUDA 11.1
cuDNN 8.1
TensorRT 7.2.3
Depstream 5.1
NVIDIA Driver Installation
Blacklisted Nouveau Driver:
sudo nano /etc/modprobe.d/blacklist-nouveau.conf
sudo update-initramfs -u
sudo reboot
Installed NVIDIA Driver 460.32.03:
chmod +x NVIDIA-Linux-x86_64-460.32.03.run
sudo apt update
sudo apt install -y build-essential # builds the required compilers gcc and cc
sudo ./NVIDIA-Linux-x86_64-460.32.03.run
Handled 32-bit dependencies: (unnecessary)
-But the installer gives you a warning suggesting it
sudo dpkg --add-architecture i386
sudo apt update
sudo apt install -y libc6:i386 libstdc++6:i386
sudo apt install -y pkg-config libglvnd-dev
Gsteamer
sudo apt update
sudo apt install -y \
 libgstreamer1.0-0 \
 gstreamer1.0-plugins-base \
 gstreamer1.0-plugins-good \
 gstreamer1.0-plugins-bad \
 gstreamer1.0-plugins-ugly \
 gstreamer1.0-libav \
 gstreamer1.0-tools \
 gstreamer1.0-x \
 gstreamer1.0-alsa \
 gstreamer1.0-gl
sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64
/3bf863cc.pub
sudo add-apt-repository "deb https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/ /"
sudo apt install -y \
gstreamer1.0-nvidia \
libgstnvvideo1.0 \
libgstnvinfer1.0
CUDA 11.1 Installation
Set up CUDA repository:
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-ubuntu1804.pin
sudo mv cuda-ubuntu1804.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/11.1.1/local_installers/cuda-repo-ubuntu1804-11-1-
local_11.1.1-455.32.00-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu1804-11-1-local_11.1.1-455.32.00-1_amd64.deb
sudo apt-key add /var/cuda-repo-ubuntu1804-11-1-local/7fa2af80.pub
sudo apt-get update
sudo apt-get -y install cuda
Fixed CUDA PATH:
Added to ~/.bashrc:
export PATH=/usr/local/cuda-11.1/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-11.1/lib64:$LD_LIBRARY_PATH
cuDNN Installation
 - I Installed 8.0.5 at the start but 8.1 was required by deepstream.
Manual installation from .tgz (https://developer.nvidia.com/rdp/cudnn-archive):
tar -xzvf cudnn-11.1-linux-x64-v8.0.5.39.tgz
sudo cp cuda/include/cudnn*.h /usr/local/cuda-11.1/include/
sudo cp cuda/lib64/libcudnn* /usr/local/cuda-11.1/lib64/
sudo chmod a+r /usr/local/cuda-11.1/include/cudnn*.h /usr/local/cuda-11.1/lib64/libcudnn*
ORRRRRRRRR
Deb package installation (https://developer.nvidia.com/rdp/cudnn-archive):
sudo dpkg -i libcudnn8_8.0.5.39-1+cuda11.1_amd64.deb
sudo dpkg -i libcudnn8-dev_8.0.5.39-1+cuda11.1_amd64.deb
TensorRT Installation
Added TensorRT repository (https://developer.nvidia.com/tensorrt/download) 7.2.X:
sudo dpkg -i nv-tensorrt-repo-ubuntu1804-cuda11.1-trt7.2.3.4-ga-20210226_1-1_amd64.deb
sudo apt-key add /var/nv-tensorrt-repo-${tag}/7fa2af80.pub
sudo apt-get update
sudo apt-get install tensorrt
sudo apt-get install python3-libnvinfer-dev
sudo apt-get install onnx-graphsurgeon
librdkafka Installation
Compiled from source:
git clone https://github.com/edenhill/librdkafka.git
cd librdkafka
git reset --hard 7101c2310341ab3f4675fc565f64f0967e135a6a
./configure --prefix=/usr/local
make
sudo make install
sudo cp -v /usr/local/lib/librdkafka* /opt/nvidia/deepstream/deepstream-5.1/lib/
DeepStream 5.1 Installation
Installed DeepStream:
sudo apt-get install ./deepstream-5.1_5.1.0-1_amd64.deb
Troubleshooting:
Reinstalled (multiple times)
Fixed GStreamer plugins:
sudo apt install --reinstall
gstreamer1.0-plugins-bad
gstreamer1.0-plugins-good
gstreamer1.0-plugins-ugly
Tested sample configurations:
deepstream-app -c /opt/nvidia/deepstream/deepstream-5.1/samples/configs/deepstream-app/source4_1080p_dec_inferresnet_tracker_sgie_tiled_display_int8.txt
use sudo nano to tweak parameters
ex: sudo nano source4_1080p_dec_infer-resnet_tracker_sgie_tiled_display_int8.txt
Driver Reinstalls:
Multiple purges and reinstalls of NVIDIA drivers (Kept changing from 460 to 470 or 455):
sudo apt purge 'nvidia-*'
sudo apt install nvidia-driver-460
Verification Commands (Tip: verify often in case dependencies don't get installed properly or get corrupted)
CUDA/cuDNN:
nvcc --version
Nvidia Drivers:
nvidia-smi
TensorRT:
python3 -c "import tensorrt; print(tensorrt.__version__)"
DeepStream:
deepstream-app --version
