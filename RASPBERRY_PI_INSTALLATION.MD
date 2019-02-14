# Raspbian Stretch Lite

## Install and compile gst-mmal

sudo apt-get -y install git \ # In order to clone gst-mmal
                        libgstreamer-plugins-base1.0-dev # Dependency of gst-mmal
git clone https://github.com/reinzor/gst-mmal.git
cd gst-mmal
LDFLAGS='-L/opt/vc/lib' CPPFLAGS='-I/opt/vc/include -I/opt/vc/include/interface/vcos/pthreads -I/opt/vc/include/interface/vmcs_host/linux' ./autogen.sh --disable-gtk-doc
make
sudo make install

export GST_PLUGIN_PATH=/usr/local/lib/gstreamer-1.0:/usr/lib/arm-linux-gnueabihf/gstreamer-1.0;
export LD_LIBRARY_PATH=/usr/local/lib/gstreamer-1.0:/usr/local/lib:/usr/local/lib64:/usr/lib64:/lib64:/opt/vc/lib:$LD_LIBRARY_PATH;
export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig:/usr/lib64/pkgconfig:/opt/vc/lib/pkgconfig:$PKG_CONFIG_PATH;
sudo ldconfig

## Install required gstreamer tools and plugins

sudo apt-get -y install gstreamer1.0-plugins-base-apps \ # gst-launch
                        gstreamer1.0-tools \ # gst-inspect
                        gstreamer1.0-plugins-good \ # qtdemux
                        gstreamer1.0-plugins-bad # h264parse plugin


## Test installation

wget https://d2qguwbxlx1sbt.cloudfront.net/TextInMotion-VideoSample-1080p.mp4 -O $HOME/sample_1080p.mp4
gst-launch-1.0 filesrc location= /home/pi/sample_1080p.mp4 ! decodebin ! mmalvideosink
