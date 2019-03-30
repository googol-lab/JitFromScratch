# JitFromScratch

[![Build Status](https://travis-ci.org/weliveindetail/JitFromScratch.svg?branch=master)](https://travis-ci.org/weliveindetail/JitFromScratch/branches/)

Collection of examples from my talks in the [LLVM Social Berlin](https://www.meetup.com/de-DE/LLVM-Social-Berlin/) and [C++ User Group Berlin](https://www.meetup.com/de-DE/berlincplusplus/) that implement various aspects of a JIT compiler based on the LLVM Orc libraries.

## Contribute

The repository follows a perfect history policy to foster traceability and understanding. If you find a bug or want to submit an improvement, please send a pull request against the [respective step](
https://github.com/weliveindetail/JitFromScratch/branches/all?query=step).

## Structure

The examples are organized around a nonsense command line program that compiles the code for a simple function at runtime:

```
template <size_t sizeOfArray>
int *integerDistances(const int (&x)[sizeOfArray], int *y) {
  int items = arrayElements(x);
  int *results = customIntAllocator(items);

  for (int i = 0; i < items; i++) {
    results[i] = abs(x[i] - y[i]);
  }

  return results;
}
```

## Steps

The example project is built in a series of self-contained steps:

0. [Add sources with original integerDistances function](https://github.com/weliveindetail/JitFromScratch/commit/1e459e7170f723b60d12b304194186eb5daf2153)
1. [Add lit tests](https://github.com/weliveindetail/JitFromScratch/commit/822acce4b4ad05828f138ae5a4b613ef4042fa4d)
2. [Add Dockerfiles to run typical use-cases on Ubuntu 18.04](https://github.com/weliveindetail/JitFromScratch/commit/bafa17e0ed6f547085625a8c275bd6141e8cd79f)
3. [Add TravisCI config for test-debug docker](https://github.com/weliveindetail/JitFromScratch/commit/81add37d0d08fee039adfc19f24b716e520515a1)
4. [Initialize LLVM for native target codegen](https://github.com/weliveindetail/JitFromScratch/commit/a57937b3e31a633a65510bc70cca5f66f5966b35)
5. [Add minimal JIT compiler based on LLJIT](https://github.com/weliveindetail/JitFromScratch/commit/a61a3a92dbe951db4cea9f14503e70d122609ee0)
6. [Create empty module and pass it to the JIT](https://github.com/weliveindetail/JitFromScratch/commit/8c8318b466f1b82064d5689f6950214a45514fe6)
7. [In debug mode dump extra info when passing -debug -debug-only=jitfromscratch](https://github.com/weliveindetail/JitFromScratch/commit/0199b52770f12e2c9ab8d9c7bca074d6bf023fd1)
8. [Generate function that takes two ints and returns zero](https://github.com/weliveindetail/JitFromScratch/commit/2396b70cc0fcfe0daa04b2b3a7e67d0f7019368e)
9. [Add basic sanity checks for IR code](https://github.com/weliveindetail/JitFromScratch/commit/cdf832a7f6182db415db1cc790daba707e989bff)
10. [Request and run trivial function](https://github.com/weliveindetail/JitFromScratch/commit/c1ad3e507049b3cab5c7e01074b30fc3e8ed5a58)
11. [Emit IR code for substraction and use it in the integerDistances function](https://github.com/weliveindetail/JitFromScratch/commit/8611f681bb17d4f34c71c2596b02f02ddadc06b2)
12. [Emit variable names to make IR more readable](https://github.com/weliveindetail/JitFromScratch/commit/9c106215c8bcfd0f1bde89607f89e523c566a60c)
13. [Allow symbol resolution from the host process](https://github.com/weliveindetail/JitFromScratch/commit/a87601e433e7127a1d1e91955f5af1e0ab7fcef5)
14. [Emit call to stdlib function abs in JITed code](https://github.com/weliveindetail/JitFromScratch/commit/7a87c5c1577f48d8466e3fcf3ca52fa1ba10b64e)
15. [Emit unrolled loop in JITed code](https://github.com/weliveindetail/JitFromScratch/commit/ce361962fa5e32550f6f0ad1eb20fc0036d80e1e)
16. [Emit call to allocator in JITed code](https://github.com/weliveindetail/JitFromScratch/commit/fc86ecb9a73a345f6a25ad67e79190843a8b2fb4)
17. [Remove wrapper function and call JITed code directly](https://github.com/weliveindetail/JitFromScratch/commit/44a67bbf139db7a86495e790c836b61339c9c9f1)
18. [Break free from LLJIT](https://github.com/weliveindetail/JitFromScratch/commit/b5bcb5eb43df859f7e4ed29c496c2a872a7749c2)
19. [Implement GDB JIT interface](https://github.com/weliveindetail/JitFromScratch/commit/09bb4cc311c3dd55b15a75cc406ac0e788d170a4)
20. [Add optimization passes controlled via -Ox command line flag](https://github.com/weliveindetail/JitFromScratch/commit/377e50a5d8c029e1433e8397fff9185e9d17e9d1)
21. [Simple name-based object cache](https://github.com/weliveindetail/JitFromScratch/commit/1fc39b30d9342066344349fbdce130c2fffc0a1a)

## Build and run locally

```
$ git clone https://github.com/weliveindetail/JitFromScratch jitfromscratch
$ mkdir build && cd build
$ cmake -GNinja -DLLVM_DIR=/path/to/llvm-build/lib/cmake/llvm ../jitfromscratch
$ ninja JitFromScratch
$ ./JitFromScratch -debug -debug-only=jitfromscratch
```
The project was tested on Ubuntu 18.04, macOS 10.14 and Windows 10. Please find real-world examples for commands and expected output on [Travis CI](https://travis-ci.org/weliveindetail/JitFromScratch).

## Build and run in docker

Docker images with prebuilt variants of LLVM can be pulled from [dockerhub](https://cloud.docker.com/u/weliveindetail/repository/docker/weliveindetail/jitfromscratch) or created manually based on the Dockerfiles in [docker/](https://github.com/weliveindetail/JitFromScratch/tree/master/docker).

## Build and debug from vscode

Install the [C/C++](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools), [
CMake Tools](https://marketplace.visualstudio.com/items?itemName=vector-of-bool.cmake-tools) and [Native Debug](https://marketplace.visualstudio.com/items?itemName=webfreak.debug) extensions and apply the configuration files in [docs/vscode/](https://github.com/weliveindetail/JitFromScratch/tree/master/docs/vscode).

## Previous Versions

I try to keep modifications on the examples minimal when porting to a newer version of LLVM. This allows to [diff for API changes](https://github.com/weliveindetail/JitFromScratch/tree/master/llvm50#previous-versions) and see how they affect individual steps.

In LLVM 8, however, the Orc library was almost entirely rewritten and I used the opportunity to also refine my examples.

You can still find the old examples here:

* [LLVM 5.0](https://github.com/weliveindetail/JitFromScratch/tree/master/llvm50)
* [LLVM 4.0](https://github.com/weliveindetail/JitFromScratch/tree/master/llvm40)
