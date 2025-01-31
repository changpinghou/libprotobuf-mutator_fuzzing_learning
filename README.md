# libprotobuf-mutator_fuzzing_learning
Learn how to combine libprotobuf-mutator with libfuzzer &amp; AFL++  

## Environment Settings 
* Ubuntu Linux 20.04 64 bit  
* Clang 11.0.1  

### Install Clang/LLVM & libfuzzer  
* Follow the step in [this article](https://linuxhint.com/install-llvm-ubuntu/) and add the toolchain's apt repository in Ubuntu.  
* `sudo apt-get install clang-11 libfuzzer-11-dev`  

### Install libprotobuf-mutator  
Follow the step in [libprotobuf-mutator's readme](https://github.com/google/libprotobuf-mutator/blob/master/README.md)  

#### Install dependencies
```shell
sudo apt-get update
sudo apt-get install protobuf-compiler libprotobuf-dev binutils cmake \
  ninja-build liblzma-dev libz-dev pkg-config autoconf libtool
```

#### Compile and test everything:

```shell
cd libprotobuf-mutator
mkdir build
cd build
( A cmake command, check the below section )
ninja check # test, might took very long time
ninja # just build, use this if you don't want to wait too long
sudo ninja install # install
```
> **Notice**  
> Use the following cmake command to build `libprotobuf-mutator-libfuzzer.so.0` and `libprotobuf-mutator.so.0` shared library

```shell
 cmake .. -GNinja -DCMAKE_C_COMPILER=clang-11 \ 
 -DCMAKE_CXX_COMPILER=clang++-11 \ 
 -DCMAKE_BUILD_TYPE=Debug \ 
 -DLIB_PROTO_MUTATOR_DOWNLOAD_PROTOBUF=ON \ 
 -DBUILD_SHARED_LIBS=ON
```

> To build static libraries, use the following `cmake` command:  
> ( **This will generate libraries that can be linked into shared libraries / normal program** )

```shell
cmake .. -GNinja -DCMAKE_C_COMPILER=clang-11 \
-DCMAKE_CXX_COMPILER=clang++-11 \
-DCMAKE_BUILD_TYPE=Debug \
-DLIB_PROTO_MUTATOR_DOWNLOAD_PROTOBUF=ON \
-DCMAKE_C_FLAGS="-fPIC" -DCMAKE_CXX_FLAGS="-fPIC"
```
## How to upgrade the environment  
* Upgrade Clang/LLVM & libfuzzer ( install a new version ) 
* Upgrade AFL++ ( git pull & rebuild )  
* Upgrade libprotobuf-mutator ( git pull & rebuild )  
    - Rebuild and re-install `libprotobuf-mutator-libfuzzer.so.0` and `libprotobuf-mutator.so.0`.  
    - Rebuild `libprotobuf-mutator-libfuzzer.a` and `libprotobuf-mutator.a`.  
* **Re-compile the protobuf with newer `protoc` and replace those `*.cc` & `*.h` with new ones.**

## FAQ  
Q : I ran into this error message while building the binary : `This file was generated by an old version of protoc.`  
A : The `test.pb.cc` and `test.pb.h` in this repo is generated with protoc `v3.13.0.0`, so if your protoc's version is newer, make sure to re-generate those two files with the original protobuf source code `test.proto` ( source code and steps to generate `*.cc` & `*.h` are all in [1_simple_protobuf](https://github.com/bruce30262/libprotobuf-mutator_fuzzing_learning/tree/master/1_simple_protobuf) ).

## Learning  
* [Simple protobuf example](https://github.com/bruce30262/libprotobuf-mutator_fuzzing_learning/tree/master/1_simple_protobuf)  
* [libprotobuf + libfuzzer](https://github.com/bruce30262/libprotobuf-mutator_fuzzing_learning/tree/master/2_libprotobuf_libfuzzer)  
* [libprotobuf + libfuzzer ( custom mutator )](https://github.com/bruce30262/libprotobuf-mutator_fuzzing_learning/tree/master/3_libprotobuf_libfuzzer_custom_mutator)  
* [How to combine libprotobuf-mutator and AFL++](https://github.com/bruce30262/libprotobuf-mutator_fuzzing_learning/tree/master/4_libprotobuf_aflpp_custom_mutator)  
* [Handling input samples from AFL++ in custom mutator](https://github.com/bruce30262/libprotobuf-mutator_fuzzing_learning/tree/master/5_libprotobuf_aflpp_custom_mutator_input)

## Reference  
* [libprotobuf-mutator](https://github.com/google/libprotobuf-mutator)  
* [Deconstructing LibProtobuf/Mutator Fuzzing](https://bshastry.github.io/2019/01/18/Deconstructing-LPM.html)  
* [Custom Proto Mutation](https://bshastry.github.io/2019/12/27/Custom-Proto-Mutation.html)  
* [AFL++ custom mutator](https://github.com/vanhauser-thc/AFLplusplus/blob/master/docs/custom_mutator.md)  
* [afl-libprotobuf-mutator](https://github.com/thebabush/afl-libprotobuf-mutator/)

## LICENSE  
[![License: CC BY-NC-SA 4.0](https://licensebuttons.net/l/by-nc-sa/4.0/88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/)
