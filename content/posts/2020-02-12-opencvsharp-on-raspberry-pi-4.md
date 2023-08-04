+++ 
date = 2020-02-12T17:00:00+00:00
title = "OpenCVSharp on Raspberry Pi 4"
aliases = ['/2020/02/12/opencvsharp-on-raspberry-pi-4', '/2020/02/12/opencvsharp-on-raspberry-pi-4.html']
+++

Using Raspbian Buster on a Raspberry 4, we are going to build OpenCV 4.2.0 from source and OpenCVSharp4.

(This is a copy paste from a microsoft forum, will change later to address in more detail, source page in the end of the article)

```
#RPI Config
sudo raspi-config
Advanced Optioins > Memory Split > 128
Interface Options > Pi Camera Enable


#Dependencies
sudo apt-get -y install build-essential cmake git unzip pkg-config libjpeg-dev libpng-dev libtiff-dev libavcodec-dev libavformat-dev libswscale-dev libgtk2.0-dev libcanberra-gtk* libxvidcore-dev libx264-dev libgtk-3-dev python3-dev python3-numpy python-dev python3-pip python-numpy libtbb2 libtbb-dev libdc1394-22-dev libv4l-dev v4l-utils libjasper-dev libopenblas-dev libatlas-base-dev libblas-dev liblapack-dev gfortran gcc-arm* protobuf-compiler


#OpenCV
wget -O opencv.zip https://github.com/opencv/opencv/archive/4.2.0.zip
wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/4.2.0.zip
unzip opencv.zip
unzip opencv_contrib.zip
mv opencv-4.2.0 opencv
mv opencv_contrib-4.2.0 opencv_contrib


#Build
cd ~/opencv/
mkdir build
cd build
cmake -D CMAKE_BUILD_TYPE=RELEASE \
        -D CMAKE_INSTALL_PREFIX=/usr/local \
        -D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib/modules \
        -D ENABLE_NEON=ON \
        -D ENABLE_VFPV3=ON \
        -D WITH_OPENMP=ON \
        -D BUILD_TIFF=ON \
        -D WITH_FFMPEG=ON \
        -D WITH_GSTREAMER=ON \
        -D WITH_TBB=ON \
        -D BUILD_TBB=ON \
        -D BUILD_TESTS=OFF \
        -D WITH_EIGEN=OFF \
        -D WITH_V4L=ON \
        -D WITH_LIBV4L=ON \
        -D WITH_VTK=OFF \
        -D OPENCV_EXTRA_EXE_LINKER_FLAGS=-latomic \
        -D OPENCV_ENABLE_NONFREE=ON \
        -D INSTALL_C_EXAMPLES=OFF \
        -D INSTALL_PYTHON_EXAMPLES=OFF \
        -D BUILD_NEW_PYTHON_SUPPORT=ON \
        -D BUILD_opencv_python3=TRUE \
        -D OPENCV_GENERATE_PKGCONFIG=ON \
        -D BUILD_EXAMPLES=OFF ..


#Swap change to CONF_SWAPSIZE=4096
sudo nano /etc/dphys-swapfile
sudo /etc/init.d/dphys-swapfile stop
sudo /etc/init.d/dphys-swapfile start


#Make OpenCV
make -j4
sudo make install
sudo ldconfig
sudo apt-get update


#Mono
sudo apt-get -y install apt-transport-https dirmngr gnupg ca-certificates
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
echo "deb https://download.mono-project.com/repo/debian stable-raspbianstretch main" | sudo tee /etc/apt/sources.list.d/mono-official-stable.list
sudo apt-get update
sudo apt-get install mono-complete


#.NET Core 3.1
sudo apt-get -y install libunwind8 gettext
wget https://download.visualstudio.microsoft.com/download/pr/67766a96-eb8c-4cd2-bca4-ea63d2cc115c/7bf13840aa2ed88793b7315d5e0d74e6/dotnet-sdk-3.1.100-linux-arm.tar.gz
sudo mkdir -p /usr/share/dotnet
sudo tar -xvf dotnet-sdk-3.1.100-linux-arm.tar.gz -C /usr/share/dotnet
sudo ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet


#OpenCVSharp https://github.com/shimat/opencvsharp
cd ~
git clone https://github.com/shimat/opencvsharp.git
cd opencvsharp
git fetch --all --tags --prune && git checkout ${OPENCVSHARP_VERSION}
cd src
mkdir build
cd build
cmake -D CMAKE_INSTALL_PREFIX=${YOUR_OPENCV_INSTALL_PATH} ..
make -j4
sudo make install


#Swap change to CONF_SWAPSIZE=100
sudo nano /etc/dphys-swapfile
sudo reboot
```

Sources

- [https://github.com/shimat/opencvsharp/issues/388](https://github.com/shimat/opencvsharp/issues/388)

- [https://social.msdn.microsoft.com/Forums/de-DE/66739526-fbaf-4d74-8da5-f3ddfb01fbe9/aspnet-core-auf-dem-raspberry-pi-mit-linux-gpio-pi-camera-uvm?forum=aspnetajaxmvcde](https://social.msdn.microsoft.com/Forums/de-DE/66739526-fbaf-4d74-8da5-f3ddfb01fbe9/aspnet-core-auf-dem-raspberry-pi-mit-linux-gpio-pi-camera-uvm?forum=aspnetajaxmvcde)
- [https://qengineering.eu/install-opencv-4.2-on-raspberry-pi-4.html](https://qengineering.eu/install-opencv-4.2-on-raspberry-pi-4.html)
- [https://github.com/shimat/opencvsharp/wiki/Tutorial-for-Unix](https://github.com/shimat/opencvsharp/wiki/Tutorial-for-Unix)