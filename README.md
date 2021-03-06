# Introduction

The architecture of this project is briefly described as following:

- Folder `programs/` is a `Rust` project (https://github.com/image-rs/image). It takes `png` image and decode
- Foder `fuzz/` contains test inputs
- Folder `verifier/` is a simplified version of AFL fuzzer. It reads all test inputs from a folder and generates reports (e.g. edge coverage, number of paths, etc...)
- File `fuzzer.py` contains the main logic of our fuzzer:
  - **Step1**: Read test inputs, bitmap to create train data
  - **Step2**: Training to detect hot bytes and brute-force them to generate test inputs (under `tmp/topk/`)
  - **Step3**: Ask `verifier` to execute newly generated test inputs (under `tmp/topk`)
  - **Step4**: Jump to **Step1**
  
# Requirements

To run this, you have to install the following packages:

- pytorch: ```pip install torch```
- numpy: ```pip install numpy```
- tqdm: ```pip install tqdm```

Note that: rust-fuzzer requires GPU


# How to run

Fuzz `png` project with our rust-fuzzer:
```javascript
make instrument && make png
```

This project is ready to fuzz. If you want to re-run the entire process. Plz, do the following steps:

```bash
make clean
make instrument # instrument Rust project
make png-afl # run AFL to generate training data. Whenever you think data is sufficient, stop AFL
make png # run our rust-fuzzer

```

# Sample
![](https://s8.gifyu.com/images/ezgif-6-28948025d975.gif)
