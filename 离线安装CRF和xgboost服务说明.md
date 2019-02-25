# 离线安装CRF和xgboost服务说明

## gcc的问题

第一步，检查gcc版本是否不低于4.8，低于则按以下升级。

```
wget http://gcc.skazkaforyou.com/releases/gcc-4.8.2/gcc-4.8.2.tar.gz
tar -xvf gcc-4.8.2.tar.gz
cd gcc-4.8.2/
./contrib/download_prerequisites
mkdir gcc-build-4.8.2
cd gcc-build-4.8.2
../configure --enable-checking=release --enable-languages=c,c++ --disable-multilib
sudo make
sudo make  install
gcc -v

sudo mv /usr/bin/gcc /usr/bin/gcc4.4.7
sudo ln -s /usr/local/gcc/bin/gcc /usr/bin/gcc
sudo mv /usr/bin/g++ /usr/bin/g++4.4.7
sudo ln -s /usr/local/gcc/bin/g++ /usr/bin/g++
sudo mv /usr/bin/c++ /usr/bin/c++4.4.7
sudo ln -s /usr/local/gcc/bin/c++ /usr/bin/c++
```

## 装Python3.6
第二布，安装python3.6，注意，先安装 'Development Tools'和 zlib-devel 这些基础的开发软件包（这些基础开发软件包没准备离线安装包）。

```
yum groupinstall 'Development Tools'
yum install zlib-devel bzip2-devel openssl-devel ncurese-devel

wget https://www.python.org/ftp/python/3.6.3/Python-3.6.3.tar.xz
tar Jxvf Python-3.6.3.tar.xz
cd Python-3.6.3
./configure --prefix=/usr/local/python3.6.3
sudo make
sudo make install
cd ../
sudo rm -r Python-3.6.3
sudo ln /usr/local/python3.6.3/bin/python3.6 /usr/bin/python3.6
sudo ln /usr/local/python3.6.3/bin/pip3.6 /usr/bin/pip3.6
```

## 安装 virtualenv

第三步，pip安装依赖的第三方库，这一步可以使用virtualenv，也可以直接使用python3.6安装

```
pip install virtualenv-15.1.0-py2.py3-none-any.whl
cd ~
mkdir virtualenv
cd virtualenv
mkdir default
virtualenv --python=/usr/local/python3.6.3/bin/python3.6 default
cd ../
source virtualenv/default/bin/activate


pip install docutils-0.14-py3-none-any.whl
pip install jmespath-0.9.3-py2.py3-none-any.whl
pip install six-1.11.0-py2.py3-none-any.whl
pip install python_dateutil-2.6.1-py2.py3-none-any.whl
pip install botocore-1.8.17-py2.py3-none-any.whl
pip install boto-2.48.0-py2.py3-none-any.whl
pip install s3transfer-0.1.12-py2.py3-none-any.whl
pip install boto3-1.5.3-py2.py3-none-any.whl
pip install bz2file-0.98.tar.gz
pip install certifi-2017.11.5-py2.py3-none-any.whl
pip install chardet-3.0.4-py2.py3-none-any.whl 
pip install idna-2.6-py2.py3-none-any.whl 
pip install JPype1-0.6.2.tar.gz
pip install numpy-1.13.3-cp36-cp36m-manylinux1_x86_64.whl
pip install pyahocorasick-1.1.6.tar.gz
pip install pymongo-3.6.0-cp36-cp36m-manylinux1_x86_64.whl
pip install PyMySQL-0.7.11-py2.py3-none-any.whl
pip install urllib3-1.22-py2.py3-none-any.whl
pip install requests-2.18.4-py2.py3-none-any.whl
pip install scikit_learn-0.19.1-cp36-cp36m-manylinux1_x86_64.whl
pip install scipy-1.0.0-cp36-cp36m-manylinux1_x86_64.whl
pip install smart_open-1.5.5.tar.gz
pip install thrift-0.10.0.zip
pip install tornado-4.5.2.tar.gz
pip install gensim-3.2.0-cp36-cp36m-manylinux1_x86_64.whl
pip install PyYAML-3.12.tar.gz
pip install pathtools-0.1.2.tar.gz
pip install argh-0.26.2-py2.py3-none-any.whl
pip install watchdog-0.8.3.tar.gz
pip install funcsigs-1.0.2-py2.py3-none-any.whl
pip install pytz-2017.3-py2.py3-none-any.whl
pip install tzlocal-1.5.1.tar.gz
pip install APScheduler-3.5.0-py2.py3-none-any.whl
```

## 安装 CRF

第四步，安装CRF

```
tar -xvf CRF++-0.58.tar.gz
cd CRF++-0.58
./configure 
make
sudo make install
sudo ln -s /usr/local/lib/libcrfpp.so.* /usr/lib64/
cd python
python setup.py build 
python setup.py install
cd ../../
sudo rm -r CRF++-0.58
```

## 安装 xgboost
第四步，安装XGBoost，首先先检查GLIBC的版本问题，然后再执行下面的指令安装XGBoost

```
git clone --recursive https://github.com/dmlc/xgboost
cd xgboost
sudo make -j4
cd python-package
python setup.py install
cd ../../
sudo rm -r xgboost
```

## GLIBC的问题：

### version `GLIBC_2.18' not found

解决办法
```
sudo wget http://ftp.gnu.org/pub/gnu/glibc/glibc-2.18.tar.xz
sudo xz -d glibc-2.18.tar.xz
sudo tar -xvf glibc-2.18.tar
cd glibc-2.18
sudo mkdir build
cd build
sudo ../configure --prefix=/usr --disable-profile --enable-add-ons --with-headers=/usr/include --with-binutils=/usr/bin
sudo make && sudo make install
```

验证 
```
# 输入 `strings /lib64/libc.so.6|grep GLIBC` 看是否更新
GLIBC_2.2.5
GLIBC_2.2.6
GLIBC_2.3
GLIBC_2.3.2
GLIBC_2.3.3
GLIBC_2.3.4
GLIBC_2.4
GLIBC_2.5
GLIBC_2.6
GLIBC_2.7
GLIBC_2.8
GLIBC_2.9
GLIBC_2.10
GLIBC_2.11
GLIBC_2.12
GLIBC_2.13
GLIBC_2.14
GLIBC_2.15
GLIBC_2.16
GLIBC_2.17
GLIBC_PRIVATE
```

### version `GLIBCXX_3.4.14' not found

解决办法
```
sudo wget http://ftp.de.debian.org/debian/pool/main/g/gcc-4.7/libstdc++6_4.7.2-5_amd64.deb
sudo ar -x libstdc++6_4.7.2-5_amd64.deb&&sudo tar xvf data.tar.gz
cd usr/lib/x86_64-linux-gnu
sudo cp libstdc++.so.6.0.17 /usr/lib64/
cd /usr/lib64/
sudo chmod +x libstdc++.so.6.0.17
sudo rm -rf libstdc++.so.6
sudo ln -s libstdc++.so.6.0.17 libstdc++.so.6
```

验证
```
# 输入 `strings /usr/lib64/libstdc++.so.6 | grep GLIBCXX` 看是否更新 
GLIBCXX_3.4
GLIBCXX_3.4.1
GLIBCXX_3.4.2
GLIBCXX_3.4.3
GLIBCXX_3.4.4
GLIBCXX_3.4.5
GLIBCXX_3.4.6
GLIBCXX_3.4.7
GLIBCXX_3.4.8
GLIBCXX_3.4.9
GLIBCXX_3.4.10
GLIBCXX_3.4.11
GLIBCXX_3.4.12
GLIBCXX_3.4.13
GLIBCXX_3.4.14
GLIBCXX_3.4.15
GLIBCXX_3.4.16
GLIBCXX_3.4.17
GLIBCXX_DEBUG_MESSAGE_LENGTH
```

### version `GLIBCXX_3.4.19' not found

解决办法
```
sudo wget http://ftp.de.debian.org/debian/pool/main/g/gcc-4.9/libstdc++6_4.9.2-10_amd64.deb
sudo ar -x libstdc++6_4.9.2-10_amd64.deb &&sudo tar xvf data.tar.xz
cd usr/lib/x86_64-linux-gnu/
sudo cp libstdc++.so.6.0.20 /usr/lib64/
cd /usr/lib64/
sudo chmod +x libstdc++.so.6.0.20
sudo rm -rf libstdc++.so.6
sudo ln -s libstdc++.so.6.0.20  ./libstdc++.so.6
```

验证
```
# 输入strings /usr/lib64/libstdc++.so.6 | grep GLIBCXX
GLIBCXX_3.4
GLIBCXX_3.4.1
GLIBCXX_3.4.2
GLIBCXX_3.4.3
GLIBCXX_3.4.4
GLIBCXX_3.4.5
GLIBCXX_3.4.6
GLIBCXX_3.4.7
GLIBCXX_3.4.8
GLIBCXX_3.4.9
GLIBCXX_3.4.10
GLIBCXX_3.4.11
GLIBCXX_3.4.12
GLIBCXX_3.4.13
GLIBCXX_3.4.14
GLIBCXX_3.4.15
GLIBCXX_3.4.16
GLIBCXX_3.4.17
GLIBCXX_3.4.18
GLIBCXX_3.4.19
GLIBCXX_3.4.20
GLIBCXX_FORCE_NEW
GLIBCXX_DEBUG_MESSAGE_LENGTH
```

## 配置Hanlp

第五步，在特定目录下安装好Hanlp的相关东西，然后配置路径文件，包括hanlp目录下的property文件和代码的path.conf文件

## 启动服务

将Service文件夹拷贝至安装目录，进入该目录，执行以下指令启动Service，注意前面的python路径，如果使用了Hanlp，则进行相应更改

sudo nohup /usr/local/bin/python3.6 service.py --port=1999 --log_file_prefix=tornado_1999.log &


