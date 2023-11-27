# Building Katago on NVIDIA Jetson Xavier NX

## Option 0: Yocto 
Long term goal will be cross compile Katago on a more powerful host to the embedded target. [Ridgerun](https://developer.ridgerun.com/wiki/index.php/Yocto_Support_for_NVIDIA_Jetson_Platforms_-_Setting_up_Yocto) claims to supportthis but I haven't verified.

## Option 1: Comiple Katago on Jetson Xavier NX
We will be using Katago v1.13.2, which requires [cmake 3.18.2](https://github.com/lightvector/KataGo/blob/ec6f2ad176e001ee7f2a51e7e0add6695bfb640b/cpp/CMakeLists.txt#L1)

### Build cmake v3.28.0
```
mkdir cmake-src
cd cmake-src
wget https://github.com/Kitware/CMake/releases/download/v3.28.0-rc5/cmake-3.28.0-rc5.tar.gz
tar vxzf cmake-3.28.0-rc5.tar.gz
cd cmake-3.28.0-rc5
./configure
make -j 4
```

### Build Katago v1.13.2
```
git clone https://github.com/lightvector/KataGo
cd KataGo
git checkout v1.13.2
cmake .
make -j 4
```

### Caveats
* If you don't have latest cmake, [download here](https://github.com/Kitware/CMake/releases/download/v3.28.0-rc5/cmake-3.28.0-rc5.tar.gz)
* Before Katago v1.8.0, Katago had problems compiling on the NX, because the processor (ARMv8/aarch64) doesn't support sse instructions (an Intel feature). [This commit}(https://github.com/lightvector/KataGo/commit/0e6ca47368b190f0b84e4f9dd0f63c2717ecc96d) fixed the bug.

## References
* https://zhuanlan.zhihu.com/p/183193381
* https://developer.ridgerun.com/wiki/index.php/Yocto_Support_for_NVIDIA_Jetson_Platforms_-_Setting_up_Yocto
* https://cmake.org/download/

