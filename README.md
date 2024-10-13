OpenBTS-UMTS reloaded 2019
==========================

OpenBTS-UMTS reloaded. Compatibility with latest UHD drivers, several fixes and updated install documentation for ubuntu 18.04

Original notes from Range Networks:  
For information on supported hardware, and build, install, setup and run instructions see [the wiki page](http://openbts.org/w/index.php/OpenBTS-UMTS).

Notes for this fork:  
You can run ./install_dependences.sh to get dependencies satisfied before installation.
===========================


This article tells you how to make a 3G connection by using OpenBTS-UMTS and LimeSDR-Mini. At this time connection have been realised but no data work for the moment keep you in touch…

Install the uhd driver
```bash
#git clone https://github.com/EttusResearch/uhd.git
# cd uhd
# git checkout UHD-3.10
# sudo apt-get install libboost-all-dev libusb-1.0-0-dev python-mako doxygen python-docutils cmake build-essential
# cd host
# mkdir build; cd build
# cmake ..
# make -j4
# make install
# ldconfig
```

Install Libosmocore
```bash
# cd /root
# git clone git://git.osmocom.org/libosmocore.git
# cd libosmocore
# autoreconf -i
# ./configure
# make
# make install
# ldconfig
# cd ..
```

Install osmo-trx
```bash
# cd /root
# git clone git://git.osmocom.org/osmo-trx.git
# cd osmo-trx
# autoreconf -i
# ./configure --with-lms
# make
# make install
# ldconfig
# cd ..
make -j4
# make install
# ldconfig
```

SoapySDR and SoapyUHD
```bash
# git clone https://github.com/pothosware/SoapySDR
# apt-get install cmake g++ libpython-dev python-numpy swig
# mkdir build
# cd build
# cmake ..
# make -j4
# sudo make install
# sudo ldconfig #needed on debian systems
# SoapySDRUtil --info

# git clone https://github.com/pothosware/SoapyUHD
# cd SoapyUHD
# mkdir build && cd build
# cmake ..
# make
# make install
# ldconfig
```

LimeSuite
```bash
# sudo apt-get install git g++ cmake libsqlite3-dev libi2c-dev libusb-1.0-0-dev
#install graphics dependencies
sudo apt-get install libwxgtk3.0-dev freeglut3-dev
# git clone https://github.com/myriadrf/LimeSuite
# cd LimeSuite
# cd build
# cmake ..
```

Install Apache Thrift
```bash
# apt install composer phpunit
# gem update
# cd
# git clone https://github.com/apache/thrift
# cd thrift
# ./bootstrap.sh
# ./configure
# make
# make install
# ldconfig
```

Install gnuradio
```bash
# nano /etc/apt/sources.list
add deb http://deb.debian.org/debian unstable main contrib non-free
# apt update
# apt install gcc-5 g++-5
# update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-5 15
# update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-5 15
# update-alternatives --install /usr/bin/cc cc /usr/bin/gcc 30
# update-alternatives --set cc /usr/bin/gcc
# update-alternatives --install /usr/bin/c++ c++ /usr/bin/g++ 30
# update-alternatives --set c++ /usr/bin/g++
# update-alternatives --config gcc
# update-alternatives --config g++
and choose gcc and g++ 5
# apt -y install git cmake g++ python-dev swig \
pkg-config libfftw3-dev libboost-all-dev libcppunit-dev libgsl-dev \
libusb-dev libsdl1.2-dev python-wxgtk3.0 python-numpy python-cheetah \
python-lxml doxygen libxi-dev python-sip libqt4-opengl-dev libqwt-dev \
libfontconfig1-dev libxrender-dev python-sip python-sip-dev python-qt4 \
python-sphinx libusb-1.0-0-dev libcomedi-dev libzmq3-dev python-mako
# git clone --recursive git://git.gnuradio.org/gnuradio
# cd gnuradio
# mkdir build
# cd build
# cmake ../
# make -j4
# sudo make install
Installation of gr-osmosdr
# git clone https://git.osmocom.org/gr-osmosdr
# cd gr-osmosdr
# mkdir build
# cd build
# cmake ..
# make
Installation of gr-gsm
# git clone https://git.osmocom.org/gr-gsm
# cd gr-gsm
# mkdir build
# cd build
# cmake ..
# mkdir $HOME/.grc_gnuradio/ $HOME/.gnuradio/
# make
```

OpenBTS-UMTS
```bash
apt install libreadline7 libreadline-dev libosip2-dev libortp9 libortp-dev libzmq5 libzmq3-dev libtool-bin

# pip install zmq
# git clone https://github.com/RangeNetworks/OpenBTS-UMTS
# cd OpenBTS-UMTS
# git submodule init
# git submodule update
# update-alternatives --config gcc
# update-alternatives --config g++
and choose gcc and g++ 5

# git clone https://github.com/RangeNetworks/libcoredumper
# cd libcoredumper
# ./build.sh
# cd coredumper-1.2.1
# autoreconf -i
# ./configure
# make -j4
# make install
# ldconfig

# tar zxvf asn1c-0.9.23.tar.gz
# cd vlm-asn1c-0959ffb/
# nano configure.ac

change
AM_INIT_AUTOMAKE=([-Wall -Werror foreign])
to
AM_INIT_AUTOMAKE=([-Wall foreign])

# autoreconf -i
# ./configure
# make -j4
# make install
# ldconfig

# cd ~/OpenBTS-UMTS
# git clone https://github.com/bbaranoff/OpBTS-LimeMini
# cd TransceiverUHD
# patch -p0 < ../OpBTS-LimeMini/OpBTS1.patch
# patch -p0 < ../OpBTS-LimeMini/OpBTS2.patch
$ sudo NodeManager/install_libzmq.sh
$ ./autogen.sh
$ ./configure
$ make
$ sudo make install
```

To setup OpenBTS-UMTS, we may need to modify the settings such as ARFCN, DNS, Firewall. Navigate to ~/OpenBTS-UMTS/apps folder and open OpenBTS-UMTS.example.sql file with the preferable text editor. You may need to edit following options:

‘GGSN.DNS’ set to ‘8.8.8.8’ to enable Google DNS.
‘GGSN.Firewall.Enable’ set to ‘0’ to disable Firewall.
‘UMTS.Radio.Band’ – set the band you are going to use. Select from 850, 900, 1700, 1800, 1900 or 2100.
‘UMTS.Radio.C0’ – set the UARFCN. Range of valid values depend upon the selected operating band. Please, ensure that you are using free operating band.
'UMTS.Radio.RxGain' "35" To avoid Overpower

Now, we need to create folders and working database:
```bash
$ sudo mkdir /var/log/OpenBTS-UMTS
$ cd /etc/OpenBTS
$ sudo sqlite3 /etc/OpenBTS/OpenBTS-UMTS.db “.read OpenBTS-UMTS.example.sql”
$ sudo cp TransceiverUHD/transceiver ~/OpenBTS-UMTS/
$ sudo cp TransceiverUHD/transceiver apps
```

We also need to setup forwarding in iptables to properly forward data between devices, host machine, and the Internet:

```bash
$ iptables -t nat -A POSTROUTING -j MASQUERADE -o eth0
$ echo 1 > /proc/sys/net/ipv4/ip_forward
```

If you have Internet connection through the another interface (for example, Wi-Fi), you need to change eth0 to the applicable one (i.e., wlan0). Please note, that you need to setup forwarding in iptables every time after you computer rebooted.

It’s important to install Subscriber Registry API and SIP Authentication Server to be able to launch OpenBTS-UMTS. Subscriber Registry controls database of subscriber information and in fact works as HLR (Home Location Registry):
```bash
# git clone https://github.com/RangeNetworks/subscriberRegistry.git
# cd subscriberRegistry
# git submodule init
# git submodule update
# autoreconf -i
# ./configure
# make
# make install
# mkdir /var/lib/asterisk/
# mkdir /var/lib/asterisk/sqlite3dir/
# cp apps/comp128 ~/OpenBTS-UMTS/
# cp apps/comp128 ~/OpenBTS-UMTS/apps/
# cp apps/comp128 /OpenBTS
```

RUNNING !!!
```bash
# cd ~/OpenBTS-UMTS/apps
# ./OpenBTS-UMTS
# cd subscriberRegistry/apps
# ./sipauthserve
```

To add the subscriber to the registry, you need to know IMSI and K_i value of your programmable SIM card. There are two common ways to get the values. The only way to use SIMs from another provider is to obtain the K_i through a roaming interface to the provider’s HLR/HSS. The second way is to buy test SIM-card and flash it using the programmer and pysim utility (http://cgit.osmocom.org/pysim/). 

Navigate to ~/subscriberRegistry/NodeManager/ and execute:
```bash
# git clone https://github.com/osmocom/pySim
# cd pySim
# apt-get install python-pyscard python-serial python-pip
pip install pytlv
# ./pySim-prog.py -j 0
```

> Name : Magic
> SMSP : e1ffffffffffffffffffffffff0581005155f5ffffffffffff000000
> ICCID : 8901901550000000005
> MCC/MNC : 901/55
> IMSI : 901550000000000
> Ki : bef0ba3847947b0a6fdd5bee6759931a
> OPC : 62d77be45c4cc0636a19a870f20b228b
> ACC : None

(# sudo ./nmcli.py sipauthserve subscribers create “name” imsi msisdn ki
Values imsi, msisdn and ki should be taken from your SIM-card.)
```bash
# python2.7 ./nmcli.py sipauthserve subscribers create "Magic" IMSI901550000000000 1234567 bef0ba3847947b0a6fdd5bee6759931a
```
Now, connect your phone to the network and test 3G. That’s it!
When the connection realise you should see this
https://www.youtube.com/watch?v=e6025o2T_rY

EDIT FOR AT THIS TIME PHONE CONNECT AND WITH AN APN THINGS HAPPEN BUT NO DATA AND THIS ERROR PLEASE COMMENT IF YOU HAVE A SOLUTION

OpenBTS-UMTS: TurboCoder.cpp:452: TurboInterleaver::TurboInterleaver(int): Assertion `K >= 40 && K <= 5114' failed.
Abandon
