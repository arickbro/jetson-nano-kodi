# jetson-nano-kodi
Kodi with HW acceleration

install Kodi Matrix
```
sudo usermod <user> -a -G docker
xhost +

docker run -it \
--net=host \
--runtime nvidia \
-e DISPLAY=$DISPLAY \
-e DBUS_SESSION_BUS_ADDRESS=$DBUS_SESSION_BUS_ADDRESS \
--device /dev/snd  \
--device /dev/nvhost-nvdec \
-v /tmp/.X11-unix/:/tmp/.X11-unix \
-v $XDG_RUNTIME_DIR:$XDG_RUNTIME_DIR \
-v /var/run/dbus/system_bus_socket:/var/run/dbus/system_bus_socket \
-v /usr/share/zoneinfo:/usr/share/zoneinfo:ro \
-v /etc/ssl/certs:/etc/ssl/certs \
-v $HOME/.kodi:/root/.kodi \
--mount type=bind,source=/media,target=/media,bind-propagation=rslave \
aliubimov/kodi-tegra:latest
```

compile inputstream.adaptive
```
docker exec -it mystifying_yalow /bin/bash
apt update
apt install git libgtest-dev cmake build-essential libexpat1-dev
```
compile gtest
```
cd /usr/src/gtest
cmake CMakeLists.txt
make

#copy or symlink libgtest.a and libgtest_main.a to your /usr/lib folder
sudo cp *.a /usr/lib
```

compile inputstream.adaptive
```
git clone -b Matrix https://github.com/peak3d/inputstream.adaptive
cd inputstream.adaptive
mkdir build && cd build
cmake -DADDONS_TO_BUILD=inputstream.adaptive -DADDON_SRC_PREFIX=../.. -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=../../kodi-build/addons -DPACKAGE_ZIP=1 ../../kodi/cmake/addons
make
  
cd ../../xbmc/addons/
  
# ls -1 ./xbmc/addons/inputstream.adaptative
addon.xml
addon.xml.in
changelog.txt
inputstream.adaptive.so -> inputstream.adaptive.so.17.6
inputstream.adaptive.so.17.6 -> inputstream.adaptive.so.2.0.19
inputstream.adaptive.so.2.0.19
libssd_wv.so
resources
 
zip -r /tmp/inputstream.adaptive.zip inputstream.adaptive
```
