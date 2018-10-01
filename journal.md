## 2018-04-10

- Buy [CanaKit Raspberry Pi 3 Complete Starter Kit - 32 GB Edition](https://www.canakit.com/raspberry-pi-3-starter-kit.html) ($70) and [Camera Module V2](https://www.amazon.com/gp/product/B01ER2SKFS) ($26)
- Connect camera to Raspberry (blue side of ribbon faces audio out as seen [here](https://projects.raspberrypi.org/en/projects/getting-started-with-picamera/4) )
- Put NOOBS microSD card in slot underneath Pi 3 board, glue on heatsinks
- Leave top of case open since even with heatsinks using all 4 cores hit ~80C in temperature gauge
- Install Raspbian (~45mins over Wi-Fi w/ updates)
- Enable Camera software via `Preferences > Raspberry Pi Configuration > Interfaces`
- Update firmware (`sudo rpi-update`)
- Reboot, test camera with `raspistill -o output.jpg`
- Install development software (~20 minutes):  
`sudo apt-get install build-essential git cmake pkg-config`  
`sudo apt-get install libjpeg-dev libtiff5-dev libjasper-dev libpng12-dev`  
`sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev`  
`sudo apt-get install libxvidcore-dev libx264-dev`  
`sudo apt-get install libgtk2.0-dev`  
`sudo apt-get install libatlas-base-dev gfortran`  

- Install OpenCV (~2 hrs with 4 cores, overnight with 1)  
`git clone https://github.com/Itseez/opencv.git`  
`git clone https://github.com/Itseez/opencv_contrib.git`  
`cd opencv`  
`mkdir build`  
`cd build`  
`cmake -D CMAKE_BUILD_TYPE=RELEASE \`  
`    -D CMAKE_INSTALL_PREFIX=/usr/local \`  
`    -D INSTALL_C_EXAMPLES=OFF \`  
`    -D INSTALL_PYTHON_EXAMPLES=ON \`  
`    -D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib/modules \`  
`    -D BUILD_EXAMPLES=ON ..`  
`make -j4` (remove `-j4` to use 1 core, likely safer...)  
`sudo make install`  
`sudo ldconfig`  

## 2018-04-11
- Found some useful notes on optimizing OpenCV on Raspberry Pi: [here](https://www.pyimagesearch.com/2017/10/09/optimizing-opencv-on-the-raspberry-pi/). Most notably, that adding `CONF_SWAPSIZE=1024` to ```/etc/dphys-swapfile``` then rebooting will increase swap to 1GB and allow compiling with 4 cores (see note previous day) . Unfortunately, this wears out the microSD card so should change back `CONF_SWAPSIZE=100` after compiling is done.
- Wrote a bit of code to fetch video stream from camera in Python and call openCV functions 

## 2018-04-18
- Switched to an off-line/asynchronous approach since Raspberry Pi doesn't have power for real-time processing, nor do we need real-time processing for a count
