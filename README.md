6-step Caffe installation guide
=============
Here follows the 6-step installation guide for installing the required packages for Caffe on Ubuntu (16.04). 

Step 1 - Download Caffe
-----
```bash
cd ~ && git clone https://github.com/BVLC/caffe.git
```

Step 2 - Download and install dependencies
-----
```bash
sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
sudo apt-get install --no-install-recommends libboost-all-dev libatlas-base-dev
sudo apt-get install cmake libgflags-dev libgoogle-glog-dev liblmdb-dev python-protobuf
sudo apt-get install build-essential python-dev
```

Step 3 - Python requirements
-----
```bash
cd ~/caffe/python/
for req in $(cat requirements.txt); do sudo pip install $req; done
sudo pip install sklearn
```

Step 4 - Edit Makefile.config
-----
```bash
cd ~/caffe
cp Makefile.config.example Makefile.config 
```
Now edit the Makefile using a text editor (e.g. gedit, nano, ...)
```bash
gedit Makefile.config
```

Change the corresponding lines to the following
`CPU_ONLY := 1`
`+INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial/`
`+LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu/hdf5/serial/`



Step 5 - Create symbolic links (ATTENTION)
-----
```bash
cd /usr/lib/x86_64-linux-gnu
sudo ln -s libhdf5_serial.so.10.0.2 libhdf5.so
sudo ln -s libhdf5_serial_hl.so.10.0.2 libhdf5_hl.so
```

Step 6 - Build Caffe
-----
```bash
cd ~/caffe
make clean
make all --jobs=2
make test --jobs=2
make runtest --jobs=2
make pycaffe
```

License
-----
MIT
