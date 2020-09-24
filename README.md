# This project provides you the patches to enable SUPRA on Intel GPU using oneAPI
## Note: Intel oneAPI is still in Beta stage, this project based on oneAPI beta07 is for reference only. It should not be used for production.

## 1. Project Introduction
### (1) Envrionment setup 

Please refer to Intel oneAPI installation guide: https://software.intel.com/content/www/us/en/develop/articles/installation-guide-for-intel-oneapi-toolkits.html


For now, please use oneAPI beta07 version. use below command to download oneAPI beta07 basekit: 

`wget http://registrationcenter-download.intel.com/akdlm/irc_nas/16702/l_BaseKit_b_2021.1.7.1506_offline.tar.gz`

For oneAPI beat07, Below compute-runtime version should be installed:

`https://github.com/intel/compute-runtime/releases/tag/20.27.17231`


OS: Ubuntu 18.04

Hardware: Intel CPU with Gen9 or later Graphics.

This project was tested on Intel i7-8700K CPU with Intel(R) UHD Graphics 630, please refer https://ark.intel.com/content/www/us/en/ark/products/126684/intel-core-i7-8700k-processor-12m-cache-up-to-4-70-ghz.html


### 2. Patch information
    The patch 0001-* describes the Intel(R) DPC++ Compatibility Tool migrates CUDA file to DPC++ file. Apply patch 0001-*, you will see a oneapi/ folder which contains migrated DPC++ files and related header files.
    The patch 0002-* describes using DPC++ file replace original CUDA file in src/SupraLib/ folder.
    The patch 0003-* describes modification to the DPC++ files. Apply patch 0001-*, 0002-* and 0003-*, you can build and run SUPRA successfully.
    The patch 0004-* describes optimization to BeamformingNode and HilbertEnvelopeNode. Apply patch 0004-*, the BeamformingNode and HilbertEnvelopeNode performance will improve.

### 

## (2) Basic steps

Install 3rd libraries:

`apt-get install cmake cmake-gui qt5-default libtbb-dev libopenigtlink-dev git`


Download this reposity to your machine:

`git clone  https://gitlab.devtools.intel.com/wangyon1/supra_for_oneapi_patch.git`

Download SUPRA form github:

`git clone https://github.com/IFL-CAMP/supra.git`

Enter supra folder:

`cd supra`

Check supra commit log:

`git log`

it will show like below:



Find this commit message:

![avatar](https://gitlab.devtools.intel.com/wangyon1/supra-markdow-pictures/-/raw/master/Commit%20info.PNG?inline=false)

Reset the supra commit HEAD, use 

`git reset --hard 73c930a08a7b1087f5be588863876a648a1add99`
![avatar](https://gitlab.devtools.intel.com/wangyon1/supra-markdow-pictures/-/raw/master/reset%20success%20modify.png?inline=false)

Apply patches to SUPRA:

`git am ../supra_for_oneapi_patch/*.patch`

Build and Run supra demo, in supra directory:

`mkdir build`


`cd build`


Setup oneAPI eviroment:

`source /opt/intel/inteloneapi/setvars.sh`

For oneAPI beta07 version, you need change the $PATH, follow this two steps:

`echo $PATH`

it will print:

![avatar](https://gitlab.devtools.intel.com/wangyon1/supra-markdow-pictures/-/raw/master/PATH%20modify.png?inline=false)

copy the whole PATH value except the contents in <font size="3">  <font color="#dd0000"> Red Rectangle.</font> <br />

Reset PATH variable value with before copied content use this command:

`export PATH=`

it should like this:

![avatar](https://gitlab.devtools.intel.com/wangyon1/supra-markdow-pictures/-/raw/master/reset%20path.PNG?inline=false)

(use your machine to print PATH content, don't copy from here)

Use opencl as low-level library(optional):

`export SYCL_BE=PI_OPENCL`

Configure project:

`CC=clang CXX=dpcpp cmake ..`

Build:

`make -j4`

Copy sample data from supra_for_oneapi_patch/ folder to supra/build/ directory

`cp -r ../../supra_for_oneapi_patch/data .`

Run the SUPRA GUI:

`src/GraphicInterface/SUPRA_GUI -c data/configDemo.xml -a`

the SUPRA GUI show like this:

![avatar](https://gitlab.devtools.intel.com/wangyon1/supra-markdow-pictures/-/raw/master/guie.PNG?inline=false)

Check the performance, open supra.log in the build directory: 

`cat supra.log` 

it will show every node performance and the average performance in Millisecond.

## 3. Additional Note