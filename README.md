A 6-steps Caffe installation guide
=============
Here follows the 6-steps installation guide for installing Caffe on Ubuntu (16.04) with Python. This guide is meant as an get easy started guide and thus will only support CPU only performance.


Step 1 - Download Caffe
-----
Open up a terminal window (`Ctrl` + `Alt` + `t`)
```bash
cd ~ && git clone https://github.com/BVLC/caffe.git
```

Step 2 - Install dependencies
-----
Now install all the required dependencies. 
```bash
sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
sudo apt-get install --no-install-recommends libboost-all-dev libatlas-base-dev
sudo apt-get install cmake libgflags-dev libgoogle-glog-dev liblmdb-dev python-protobuf
sudo apt-get install build-essential python-dev
```

Step 3 - Install Python requirements
-----
You can skip this step if you do not need the Python interface. 
```bash
cd ~/caffe/python/
for req in $(cat requirements.txt); do sudo pip install $req; done
sudo pip install sklearn
```

Step 4 - Edit the `Makefile.config`
-----
Create the Makefile.config required for building Caffe. The git package already comes with an Makefile example and we only need to change a few things.
```bash
cd ~/caffe
cp Makefile.config.example Makefile.config 
```

Now edit the Makefile using a text editor (e.g. `gedit`, `nano`, ...)
```bash
gedit Makefile.config
```

Scroll down and find and change the following lines:
| Line | Change                                                  | To                                                                                             |
| -----| ------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| 8    | `# CPU_ONLY := 1`                                       | `CPU_ONLY := 1`                                                                                |
| 90   | `INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include`  | `INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial/`               |
| 91   | `LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib` | `LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu/hdf5/serial/` |



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
