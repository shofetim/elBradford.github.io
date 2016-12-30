---
published: true
title: 'How To: GPU (NVENC) Accelerated FFMPEG in an LXC Container'
permalink: /2016/GPU-FFMPEG-in-LXC
author: bradford
layout: post
categories:
  - Technology
  - Tutorials
tags:
  - FFMPEG
  - NVENC
  - LXC
  - Proxmox
comments: true
---
_This project has been on my home media server to-do list since I rebuilt my media server this summer. I put off the problem, which I knew would take some work, until this week while I had the house to myself and no school or work to attend to. It was prickly enough that I decided to post about it - in case I ever need to do this again and forget how (likely), or that this shows up high enough in the search results to help someone else who`s struggling._

BLUF: If you`re looking for the guide, look down at part 3. Parts 1 and 2 are where I detail my journey.

When I built my home media server, I planned it to be a multi-purpose machine. I built it on Proxmox 4 (currently 4.4, based on Debian Jessie 8.6) so that I could either use virtualization or its built-in lxc-based container system to segregate the various roles this server will have.

It`s primary roles are as a file system (Raid-Z based redundant storage array) and a media server using Emby, a superior mono-based (.net) Plex-alternative that I switched to a couple years ago.

I`ve been running the media server continuously, and it was the main reason I built the server. The hardware I built it on, in order to be power-saving as a 24/7 appliance, struggled when it was forced to transcode some of the high-resolution video streams (1080p was around 17 fps, less than realtime). This on an 8-core atom processor (C2758-based A1SRM-2758F). I think part of my problem was how Proxmox handled CPU scheduling - it seemed to be reserving some of the CPU for another container, despite that container not needing it.

Since I already had purchased a GT 710 which, despite its naming convention, is a recently-released, passively cooled 28nm Kepler chip that draws around 19 watts. Unfortunately it doesn`t support HEVC/x265, but hopefully in the next few years Nvidia will come out with an updated low-power card that supports it.

Emby came out with experimental GPU encoding support [as early as Januay 2015](https://emby.media/community/index.php?/topic/17078-some-great-happenings-around-the-community/?hl=gpu). So my mission was to 1) Get my containerized Emby server the ability to access the GPU and 2) get a copy of FFMPEG that supports NVENC (Nvidia`s GPU-accellerated encoding engine).




## Part 1 ##

### Pass GPU to LXC Container ###

In order to get a LXC container to have access to the NVidia GPU, you need to pass the device through to the container in the lxc config file (more on that later). But in order to do that, the devices need to appear in the ``/dev/`` folder, specificially ``/dev/nvidia0` and its brothers.

My breakthrough came when I found [this guide](http://sqream.com/setting-cuda-linux-containers-2/) on giving CUDA access to Linux containers. You have a choice - you can download the drivers from a repository (in my case nvidia proprietary driver version `367.48` was available from jessie-backports). I tried this - several locations suggested it, and it`s an easier solution if it works. In addition, 367 was recent enough to give my hardware NVENC, the holy grail (at least for me).

It didn't work. I never got the desired nodes in `/dev`, namely `/dev/nvidia0`, `/dev/nvidiactl`, and `/dev/nvidia-uvm`, nor did `nvidia-smi` ever run correctly. In the end, I settled on downloading the most recent .run package from Nvidia directly, version `375.26`. Once I did this, and configured `nvidia-persistenced` to run at startup, I got my `/dev` nodes to appear consistently at startup.

My next job was to get them to also appear in the container.

I added the necessary lines to my lxc `.config` file, and indeed they did show up in the container. However they didn't work. I tested it by running Nvidia System Management Interface (`nvidia-smi`) in the container, and it failed every time.

I struggled with this for far too long, until I had an eureka moment after reading [a forum post](https://forum.openvz.org/index.php?t=tree&goto=52646&S=8435426c35e817f11e7afea78c459581#msg_52646): I need the same OS in the container that`s in the host, Debian 8.6. My current container was Ubuntu 16.04.

I spun up a new Debian 8.6-based container, made it as similar to the old except I made it unpriviliged, confident it would work based on much of my research.

Voila, I was able to see the devices and `nvidia-smi` could query the device successfully. One down, one to go.




## Part 2 ##

### Get a NVENC-compatible FFMPEG ###

I tried the most obvious approach, which is using a static build of FFMPEG for my architecture. Emby devs suggested that other users use [these builds](https://www.johnvansickle.com/ffmpeg/), so I grabbed it too. A quick `ffmpeg -encoders | grep nvenc` showed that the they were built with NVENC. I tried it out, and every time I tried using the h264_nvenc encoder it would segmentation fault.

I decided to try and build it myself, and I used two sources - one, [an older pdf guide](http://developer.download.nvidia.com/compute/redist/ffmpeg/1511-patch/FFMPEG-with-NVIDIA-Acceleration-on-Ubuntu_UG_v01.pdf) from Nvidia, and the second was Nvidia`s [ffmpeg page](https://developer.nvidia.com/ffmpeg). They were both valuable, but I leaned more heavily on the newer web page for my final product.

After a few false starts, I got a working build that was able to utilize the GPU to encode video. After some more work turning my ffmpeg binary into a more multi-purpose encoder (setting the appropriate flags), it was finished. After a trial run, it worked flawlessly.




## Part 3 ##

### How to do all the stuff ###

#### Step 0: Requirements ####

Start with a working LXC host with an Nvidia-based GPU that supports NVENC

#### Step 1: Download and install Nvidia proprietary drivers on the host ####

Go to http://www.nvidia.com/object/unix.html to download the most recent driver for your architecture. You may need to install gcc and make from your package manager. Make sure you remove any previously-installed nvidia drivers before you do this.

```bash
# wget http://us.download.nvidia.com/XFree86/Linux-x86_64/375.26/NVIDIA-Linux-x86_64-375.26.run
# chmod +x NVIDIA-Linux-x86_64-375.26.run
# ./NVIDIA-Linux-x86_64-375.26.run
```

The installer will run - it's pretty painless. Once it is installed, you'll see some Nvidia binaries were installed and are part of your path - type `nvid` and tab to see them all. You _should_ have at least some of the `/dev` nodes present, but I had to tell `nvidia-persistenced` to run at startup

```bash
# nvidia-persistenced --persistence-mode
```

Test you can access the driver by running `nvidia-smi`

```bash
# nvidia-smi
```

#### Step 2: Passthrough GPU /dev nodes to LXC ####

_Important:_ Make sure your container is the same version as your host - if your host is Ubuntu 16.10, your container must be Ubuntu 16.10.

Edit your lxc container `.conf` file to pass through the devices - Proxmox places the files in `/etc/pve/lxc/###.conf` - edit it like so:

```lxc.mount.entry: /dev/nvidia0 dev/nvidia0 none bind,optional,create=file,uid=65534,gid=65534
lxc.mount.entry: /dev/nvidiactl dev/nvidiactl none bind,optional,create=file,uid=65534,gid=65534
lxc.mount.entry: /dev/nvidia-uvm dev/nvidia-uvm none bind,optional,create=file,uid=65534,gid=65534```

#### Step 3:  Download and install Nvidia proprietary drivers in the container ####

Log into your Container and install the driver, but don't install the kernel modules - let's use `--no-kernel-module` to make sure the .run package doesn't try to install them, since we`re using the host kernel that already has the module loaded:

```bash
# lxc-attach -n ###
# wget http://us.download.nvidia.com/XFree86/Linux-x86_64/375.26/NVIDIA-Linux-x86_64-375.26.run
# chmod +x NVIDIA-Linux-x86_64-375.26.run
# ./NVIDIA-Linux-x86_64-375.26.run --no-kernel-module
```

Test that `nvidia-smi` works:

```bash
# nvidia-smi
```

#### Step 4: Install the CUDA and NVENC SDKs ####

We need to download the current CUDA SDK, which we`ll use to build FFMPEG later. Go to https://developer.nvidia.com/cuda-downloads to download the appropriate runfile.

When you install the CUDA SDK, be sure **not** to install the driver again - you already have. Install the toolkit and the samples in the default locations.

```bash
# wget https://developer.nvidia.com/compute/cuda/8.0/prod/local_installers/cuda_8.0.44_linux-run
# chmod +x cuda_8.0.44_linux-run
# ./cuda_8.0.44_linux-run
```

We also need to copy the cuda.h file to our local includes to make it easier to build FFMPEG:

```bash
# cp /usr/local/cuda/include/cuda.h /usr/include/
```

Next, download the NVENC SDK from the developer`s site: https://developer.nvidia.com/nvidia-video-codec-sdk. You will need to create an account to gain access to the most current version of the SDK. Once you have it, extract it to a development folder and move the header files to your local include directory:

```bash
# mkdir ~/development
# cd ~/development
# wget https://developer.nvidia.com/designworks/video_codec_sdk/downloads/v7.1?accept_eula=yes
# unzip Video_Codec_SDK_7.1.9.zip
# cp Video_Codec_SDK_7.1.9/Samples/common/inc/*.h /usr/local/include
```

#### Step 5: Grab other FFMPEG dependencies ####

Based on your FFMPEG configuration, you may need different libraries. I opted to grab as many as I could from apt in order to simplify, but you may find you need to search for others. My configuration is based on the [general-purpose static build recommended by Emby devs](https://www.johnvansickle.com/ffmpeg/), and called for the following libraries and packages to be installed:

```bash
# apt install libvpx4 frei0r-plugins-dev libgnutls28-dev libass-dev libmp3lame-dev libopencore-amrnb-dev libopencore-amrwb-dev libopenjpeg-dev libopus-dev librtmp-dev libsoxr-dev libspeex-dev libtheora-dev libvo-amrwbenc-dev libvorbis-dev libvpx-dev libwebp-dev libx264-dev libx265-dev libxvidcore-dev
```

Later on, during the configure stage, you may find you need more or less of these libraries.

#### Step 6: Get and Build FFMPEG ####

Download ffmpeg [from git](https://git.ffmpeg.org/gitweb/ffmpeg.git) and select your desired branch. I chose 3.1, since 3.2.2 had issues with Emby (based on a flaw in ffmpeg):

```bash
# cd ~/development
# git clone https://git.ffmpeg.org/ffmpeg.git
# cd ffmpeg
```

View and select a remote branch

```bash
# git branch -a
# git checkout release/3.1
```

Now, let`s configure our ffmpeg build

```bash
# mkdir ../ffmpegbuild
# cd ../ffmpegbuild
# ../ffmpeg/configure \
                        --enable-nonfree --disable-shared --enable-nvenc --enable-cuda --enable-cuvid --enable-libnpp --extra-cflags=-Ilocal/include --enable-gpl --enable-version3 --disable-debug --disable-ffplay --disable-indev=sndio --disable-outdev=sndio --enable-fontconfig --enable-frei0r --enable-gnutls --enable-gray --enable-libass --enable-libfreetype --enable-libfribidi --enable-libmp3lame --enable-libopencore-amrnb --enable-libopencore-amrwb --enable-libopenjpeg --enable-libopus --enable-librtmp --enable-libsoxr --enable-libspeex --enable-libtheora --enable-libvo-amrwbenc --enable-libvorbis --enable-libvpx --enable-libwebp --enable-libx264 --enable-libx265 --enable-libxvid --extra-cflags=-I/usr/local/cuda/include --extra-ldflags=-L/usr/local/cuda/lib64
```

The last line tells ffmpeg where to find the cuda libraries it needs from the CUDA SDK installed earlier. If all looks good, let`s build it:

```bash
# make -j 10
```

That's it! Take FFMPEG for a spin - try some encodes and see how fast it goes. If it works, let`s install it! If it doesn't, take a look at the error and start your troubleshooting there.

```bash
# make install
```




## Impressions ##

### The Good ###

I told Emby to use the system-installed ffmpeg and to use NVENC for transcoding. I was _immediately_ blown away at how fast the transcoded video stream started, and how quickly it proceeded. I had a 23Mbit source file that I forced to transcode to 1Mbit, and later at 21Mbit. According to my benchmarks, the 21Mbit transcode of a 23Mbit source was giving me about 112 fps. More than fast enough - in fact, if I can get the quality higher with a slower transcode, I'd be happy, because...

### The Bad ###

Frankly, the NVENC encoded video looks much worse than the original. Even when it was transcoding to 21Mbit, it had a lot of compression artifacts, and the beautiful grain of the source was entirely lost. It was apparent on my top-end 4k TV and on my low end 1080p LCD.

Ultimately, transcoding was unwatchable before, so this is definitely an improvement. I will work with the Emby devs to see if there`s a way to pass a quality setting to NVENC so it more closely matches the source material.
