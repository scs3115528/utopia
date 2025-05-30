# 用法
## 1.以项目snappy为例
1.环境搭建
```
git clone https://github.com/UTopia-fuzzing/UTopia.git
cd UTopia
git submodule update --init --recursive
docker buildx build -f docker/Dockerfile -t utopia .
docker run -it utopia bash
cd /UTopia
```
2.编译utopia
```
cmake -B build -S .
cmake --build build -j$(nproc)
```
3.运行
```
python3 -m helper.make snappy
python3 -m helper.build snappy
cd /UTopia/exp/snappy/output/fuzzers
./Snappy_FourByteOffset_Test
```
## 2.cJSON
helper/make.yml
```
cJSON:
    repo:
      dir: cJSON
      cmd:
        - apt update && apt install -y cmake
        - git clone  https://github.com/DaveGamble/cJSON.git --config user.name=autofuzz --config user.email=autofuzz@autofuzz.com
        - cd cJSON && git checkout acc76239bee01d8e9c858ae2cab296704e52d916 -b autofuzz_base
    compile:
      org:
        - rm -rf build && mkdir -p build
        - cd build &&  cmake -DBUILD_SHARED_LIBS=OFF -DENABLE_CJSON_TEST=ON k#:cmake_org_flags:# ..
        - rm -f output/build.log
        - cd build && export COMPILE_LOG="$(realpath ../output/build.log)" && make V=1 -j8
        - cp build/libcjson.a output/
      fuzzer:
        - rm -rf build && mkdir -p build
        - cd build && cmake -DBUILD_SHARED_LIBS=OFF -DENABLE_CJSON_TEST=ON  k#:cmake_fuzzer_flags:# ..
        - cd build && make -j8 > /dev/null 2>&1
        - cp build/libcjson.a output/libcjson_fuzzer.a
      profile:
        - rm -rf build && mkdir -p build
        - cd build && cmake -DBUILD_SHARED_LIBS=OFF -DENABLE_CJSON_TEST=ON  k#:cmake_profile_flags:# ..
        - cd build && make -j8 > /dev/null 2>&1
        - cp build/libcjson.a output/libcjson_profile.a

```
helper/build.yml
```
cJSON:
  name: cJSON
  prjdir: cJSON
  builddir: build
  libs:
    libcjson.a:
  uts:
    cJSON_test:
      libalias: libcjson.a
      buildkey: cJSON_test
```
运行
```
python3 -m helper.make cJSON
python3 -m helper.build cJSON
```
