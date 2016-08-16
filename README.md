# A 6-step Caffe installation guide
Here follows the 6-steps installation guide for installing Caffe on Ubuntu (16.04) with Python. This guide is meant as an "get easy started" guide and thus will be CPU runtime only. The guide is separated into the following steps:

1. <a href="https://github.com/aaskov/6-steps-Caffe-install#1-download-caffe">Download Caffe</a>
2. <a href="https://github.com/aaskov/6-steps-Caffe-install#2-install-dependencies">Install dependencies</a>
3. <a href="https://github.com/aaskov/6-steps-Caffe-install#3-install-python-requirements">Install Python requirements</a>
4. <a href="https://github.com/aaskov/6-steps-Caffe-install#4-edit-the-makefile">Edit the Makefile</a>
5. <a href="https://github.com/aaskov/6-steps-Caffe-install#5-create-symbolic-links">Create symbolic links</a>
6. <a href="https://github.com/aaskov/6-steps-Caffe-install#6-build-caffe">Build Caffe</a>


## 1. Download Caffe
Open up a terminal window (<kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>t</kbd>) and run:
```bash
cd ~ && git clone https://github.com/BVLC/caffe.git
```


## 2. Install dependencies
```bash
sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
sudo apt-get install --no-install-recommends libboost-all-dev libatlas-base-dev
sudo apt-get install cmake libgflags-dev libgoogle-glog-dev liblmdb-dev python-protobuf
sudo apt-get install build-essential python-dev
```


## 3. Install Python requirements
I highly recommend the Python interface for Caffe (known as PyCaffe), however you can skip this step if you do not whish to use Python with Caffe.
```bash
cd ~/caffe/python/
for req in $(cat requirements.txt); do sudo pip install $req; done
sudo pip install sklearn
```


## 4. Edit the Makefile
We need to create the `Makefile.config` and edit it to our needs. The git package already comes with an example so we only need to change a couple of lines.
```bash
cd ~/caffe && cp Makefile.config.example Makefile.config 
```

Now edit the Makefile using a text editor (e.g. `gedit`, `nano`, ...) and change the lines as listed in the table below.
```bash
gedit Makefile.config
```

| Line | Change from  | Change to |
| ---- | ------------ | --------- |
| 8    | `# CPU_ONLY := 1`  | `CPU_ONLY := 1` |
| 90   | `INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include` | `INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial/` |
| 91   | `LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib` | `LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu/hdf5/serial/` |



## 5. Create symbolic links
On Ubuntu we need to create symbolic links to the `libhdf5` package. Be carefull with the verision numbers.
```bash
cd /usr/lib/x86_64-linux-gnu
sudo ln -s libhdf5_serial.so.10.0.2 libhdf5.so
sudo ln -s libhdf5_serial_hl.so.10.0.2 libhdf5_hl.so
```


## 6. Build Caffe
Finally we need to build Caffe with the following commands. You can speed up this process by changing `--jobs=2` to eg. `--jobs=4` if you have a quad-core processor.
```bash
cd ~/caffe
make clean
make all --jobs=2
make test --jobs=2
make runtest --jobs=2
make pycaffe
```
You're done!


#### License
MIT
