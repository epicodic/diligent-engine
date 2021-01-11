

Fork from https://github.com/DiligentGraphics/DiligentEngine with minor modifications:

- to work on older compilers
- generate shared libraries instead of static ones

# Getting started

## Clone

Clone repository and all submodules:

```shell
git clone  https://github.com/epicodic/diligent-engine
cd diligent-engine
git submodule update --init  --recursive
```

## Build

```shell
mkdir build && cd build
cmake .. -DCMAKE_BUILD_TYPE=Release
make
```

