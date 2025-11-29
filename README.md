# TFHEpp
This repository contains the stable code for the TFHE library based on [TFHEpp](https://github.com/virtualsecureplatform/TFHEpp) (Version 10). Below is the introduction for TFHEpp.

TFHEpp is a full scratched pure C++ version of TFHE. TFHEpp is slightly(about 10%) faster than the original [TFHE implementation](https://github.com/tfhe/tfhe). In addition to that, THFEpp supports [Circuit Bootstrapping](https://eprint.iacr.org/2018/421), [Programable Boootstrapping many LUT](https://eprint.iacr.org/2021/729), Partial Key([Klemsa's version](https://eprint.iacr.org/2021/634), [Zama's version](https://eprint.iacr.org/2023/979) . Named as subset key in the code), and [Modified Chen's Packing](https://eprint.iacr.org/2024/1318) (We call it as annihilate key switching in our code), [mean compensation in rounding noises](https://eprint.iacr.org/2025/809). 
We also include partial support for B/FV, written in include/bfv++.hpp. For the implementation of the Modified Chen's Packing, we also used the idea of [dividing TRLWE at each recursion](https://eprint.iacr.org/2025/1088), though I developed that independently. 
TFHEpp depends on AVX2 because we use SPQLIOS FMA. If you want to run TFHEpp without AVX2, see the spqlios++ branch. It includes a pure C++ implementation of SPQLIOS as a header-only library, but it is slow. 

# Supported Compiler

This code includes UTF-8 identifiers like α, using `extern template`, and std::make_unique_for_overwrite. Therefore, GCC11 or later and Clang16 or later are primarily supported compilers. 

# Parameter
The default parameter (128bit.hpp) is 128-bit security. Please add -DUSE_80BIT_SECURITY=ON to use a faster but less secure parameter.
The fastest parameter with 128-bit security is concrete.hpp. Please add -DUSE_CONCRETE=ON to use a faster parameter. 
If you need your own parameter, please modify the files under include/params/ or add your own hpp and add change the macro definition in include/param.hpp. 

## Ternary Key
As you will see in the files under include/params, we use a ternary key for lvl1 or higher-level parameters. 

# FFTW3 Support
Some environments that do not support AVX2 cannot use spqlios. Instead of spqlios, TFHEpp can use fftw3.
To use fftw3,  install `libfftw3-dev` and add `-DUSE_FFTW3=ON` to the compile option.

# Setup
The following are my build settings for TFHEpp, running on a server with an Intel Xeon Platinum 8370C.
```bash
git clone https://github.com/ThuryW/TFHEpp.git --recursive
cd TFHEpp
mkdir build
cd build
cmake .. -DENABLE_TEST=ON -DUSE_BLAKE3=OFF -DUSE_RANDEN=ON -DUSE_AVX512=ON -DCMAKE_EXPORT_COMPILE_COMMANDS=ON
make
sudo make install # to install in /usr/local/lib
./test/nand
```

# Citation
For the people who want to cite this library directly (may be in addition to [VSP paper](https://www.usenix.org/conference/usenixsecurity21/presentation/matsuoka)), I give a below example of bibtex citation.

```
@misc{TFHEpp,
	author = {Kotaro Matsuoka},
	title = {{TFHEpp: pure C++ implementation of TFHE cryptosystem}},
  	year = {2020},
	howpublished = {\url{https://github.com/virtualsecureplatform/TFHEpp}}
}
```
