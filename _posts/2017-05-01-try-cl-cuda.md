---
layout: post
title: cl-cudaをつかってみる
date:   2017-05-01
categories: common lisp
---
# cl-cudaをつかってみる

[cl-cuda](https://github.com/takagi/cl-cuda)

私の環境
- OSX 10.12.4 (macOS Sierra) (MacBookPro)
- GeForce GT 750M
- CUDA 8.0.81
- SBCL 1.3.17

## sbclのインストール

```
$ brew install sbcl
```

## quick lispのインストール

1. インストール・スクリプトをダウンロードして、

```
$ sbcl --load quicklisp.lisp
* (quicklisp-quickstart:install)
* (ql:add-to-init-file)
```

## cl-cudaのインストール
```
$ sbcl
* (ql:quickload :cl-cuda)
```
ここでエラーが出る。(想定どおり???)

```
* (ql:quickload :cffi)
```
そして、リトライ
```
* (ql:quickload :cl-cuda)
To load "cl-cuda":
  Load 1 ASDF system:
    cl-cuda
; Loading "cl-cuda"
.....; cc -m64 -I /opt/local/include/ -o /Users/ichise/.cache/common-lisp/sbcl-1.3.17-macosx-x64/Users/ichise/quicklisp/dists/quicklisp/software/cl-cuda-20170403-git/src/driver-api/type-grovel__grovel-tmpGHU3ALSV -I/Users/ichise/quicklisp/dists/quicklisp/software/cffi_0.18.0/ /Users/ichise/.cache/common-lisp/sbcl-1.3.17-macosx-x64/Users/ichise/quicklisp/dists/quicklisp/software/cl-cuda-20170403-git/src/driver-api/type-grovel__grovel.c
/Users/ichise/.cache/common-lisp/sbcl-1.3.17-macosx-x64/Users/ichise/quicklisp/dists/quicklisp/software/cl-cuda-20170403-git/src/driver-api/type-grovel__grovel.c:6:10: fatal error: 
      'cuda/cuda.h' file not found
#include <cuda/cuda.h>
         ^
1 error generated.

debugger invoked on a CFFI-GROVEL:GROVEL-ERROR in thread #<THREAD "main thread" RUNNING {1001F1EA93}>: Subprocess (:PROCESS #<SB-IMPL::PROCESS :EXITED 1>)
 with command ("cc" "-m64" "-I" "/opt/local/include/" "-o" "/Users/ichise/.cache/common-lisp/sbcl-1.3.17-macosx-x64/Users/ichise/quicklisp/dists/quicklisp/software/cl-cuda-20170403-git/src/driver-api/type-grovel__grovel-tmpGHU3ALSV" "-I/Users/ichise/quicklisp/dists/quicklisp/software/cffi_0.18.0/" "/Users/ichise/.cache/common-lisp/sbcl-1.3.17-macosx-x64/Users/ichise/quicklisp/dists/quicklisp/software/cl-cuda-20170403-git/src/driver-api/type-grovel__grovel.c")
 exited with error code 1

Type HELP for debugger help, or (SB-EXT:EXIT) to exit from SBCL.

restarts (invokable by number or by possibly-abbreviated name):
  0: [RETRY                        ] Retry PROCESS-OP on #<CUDA-GROVEL-FILE "cl-cuda" "src" "driver-api" "type-grovel">.
  1: [ACCEPT                       ] Continue, treating PROCESS-OP on #<CUDA-GROVEL-FILE "cl-cuda" "src" "driver-api" "type-grovel"> as having been successful.
  2:                                 Retry ASDF operation.
  3: [CLEAR-CONFIGURATION-AND-RETRY] Retry ASDF operation after resetting the configuration.
  4: [ABORT                        ] Give up on "cl-cuda"
  5:                                 Exit debugger, returning to top level.

(CFFI-GROVEL:GROVEL-ERROR "~a" #<UIOP/RUN-PROGRAM:SUBPROCESS-ERROR {1002514C73}>)
   source: (ERROR (QUOTE GROVEL-ERROR) :FORMAT-CONTROL FORMAT-CONTROL :FORMAT-ARGUMENTS FORMAT-ARGUMENTS)
0] 5
; 
; compilation unit aborted
;   caught 2 fatal ERROR conditions

* (exit)

```

ヘッダファイル「`cuda/cuda.h`」がみつからない。

```
$ cd /Developer/NVIDIA/CUDA-8.0/include
$ grep cuda/cuda.h *lisp
type-grovel.lisp:#+darwin (include "cuda/cuda.h")
```
たぶん、この行で生成するC言語のソースファイルがおかしい。ファイルを編集してみる
```
$ diff type-grovel.lisp.org type-grovel.lisp
14,15c14,15
< #+darwin (include "cuda/cuda.h")
< #-darwin (include "cuda.h")
---
> #+darwin (include "cuda.h")
> ;; #-darwin (include "cuda.h")
```

cl-cudaのインストールをリトライ
```
$ sbcl
This is SBCL 1.3.17, an implementation of ANSI Common Lisp.
More information about SBCL is available at <http://www.sbcl.org/>.

SBCL is free software, provided as is, with absolutely no warranty.
It is mostly in the public domain; some portions are provided under
BSD-style licenses.  See the CREDITS and COPYING files in the
distribution for more information.
* (ql:quickload :cl-cuda)
To load "cl-cuda":
  Load 1 ASDF system:
    cl-cuda
; Loading "cl-cuda"
.....; cc -m64 -I /opt/local/include/ -o /Users/ichise/.cache/common-lisp/sbcl-1.3.17-macosx-x64/Users/ichise/quicklisp/dists/quicklisp/software/cl-cuda-20170403-git/src/driver-api/type-grovel__grovel-tmpGHU3ALSV -I/Users/ichise/quicklisp/dists/quicklisp/software/cffi_0.18.0/ /Users/ichise/.cache/common-lisp/sbcl-1.3.17-macosx-x64/Users/ichise/quicklisp/dists/quicklisp/software/cl-cuda-20170403-git/src/driver-api/type-grovel__grovel.c
; /Users/ichise/.cache/common-lisp/sbcl-1.3.17-macosx-x64/Users/ichise/quicklisp/dists/quicklisp/software/cl-cuda-20170403-git/src/driver-api/type-grovel__grovel /Users/ichise/.cache/common-lisp/sbcl-1.3.17-macosx-x64/Users/ichise/quicklisp/dists/quicklisp/software/cl-cuda-20170403-git/src/driver-api/type-grovel__grovel.grovel-tmp.lisp
.............................................
[package cl-cuda.lang.util].......................
[package cl-cuda.lang.data].......................
[package cl-cuda.lang.type].......................
[package cl-cuda.lang.syntax].....................
[package cl-cuda.lang.environment]................
[package cl-cuda.lang.built-in]...................
[package cl-cuda.lang.kernel].....................
[package cl-cuda.lang.compiler.compile-data]......
[package cl-cuda.lang.compiler.compile-type]......
[package cl-cuda.lang.compiler.type-of-expression]
[package cl-cuda.lang.compiler.compile-expression].
..................................................
[package cl-cuda.lang.compiler.compile-statement].
..................................................
[package cl-cuda.lang.compiler.compile-kernel]....
[package cl-cuda.lang]............................
[package cl-cuda.api.nvcc]........................
[package cl-cuda.api.kernel-manager]..............
[package cl-cuda.api.memory]......................
[package cl-cuda.api.context].....................
[package cl-cuda.api.defkernel]...................
[package cl-cuda.api.macro].......................
[package cl-cuda.api.timer].......................
[package cl-cuda.api].............................
[package cl-cuda]
(:CL-CUDA)
```

成功しました。

## cl-cudaを動かしてみる
cl-cuda/examples/vector-add.lispを実行してみる
```
* (main)
Invoking CU-INIT succeded.
Invoking CU-DEVICE-GET succeded.
Invoking CU-CTX-CREATE succeded.
Invoking CU-DEVICE-COMPUTE-CAPABILITY succeded.
Invoking CU-MEM-ALLOC succeded.
Invoking CU-MEM-ALLOC succeded.
Invoking CU-MEM-ALLOC succeded.
Invoking CU-MEMCPY-HOST-TO-DEVICE succeded.
Invoking CU-MEMCPY-HOST-TO-DEVICE succeded.
nvcc -arch=sm_30 -I /Users/ichise/quicklisp/dists/quicklisp/software/cl-cuda-20170403-git/include -ptx -o /tmp/cl-cuda.0KEWca.ptx /tmp/cl-cuda.0KEWca.cu

debugger invoked on a SIMPLE-ERROR in thread
#<THREAD "main thread" RUNNING {1001F1F053}>:
  nvcc exits with code: 1
nvcc fatal   : The version ('80100') of the host compiler ('Apple clang') is not supported


Type HELP for debugger help, or (SB-EXT:EXIT) to exit from SBCL.

restarts (invokable by number or by possibly-abbreviated name):
  0: [ABORT] Exit debugger, returning to top level.

(CL-CUDA.API.NVCC::RUN-NVCC-COMMAND #P"/tmp/cl-cuda.0KEWca.cu" #P"/tmp/cl-cuda.0KEWca.ptx")
   source: (ERROR "nvcc exits with code: ~A~%~A" EXIT-CODE
                  (GET-OUTPUT-STREAM-STRING OUT))
0] 0
```

システム上のコンパイラがnvccでサポートしていないらしい。

[NVCC does not support Apple Clang version 8.x](https://github.com/arrayfire/arrayfire/issues/1384)
- Appleの開発者向け[サイト](https://developer.apple.com/downloads/)にログイン
- Xcode Command Line Tools 7.3をダウンロード&インストール
- `sudo xcode-select --switch /Library/Developer/CommandLineTools`

もう一度、cl-cudaを動かしてみる。
```
$ sbcl
* (ql:quickload :cl-cuda)
* (load "vector-add.lisp")
* (cl-cuda-examples.vector-add::main)
Invoking CU-INIT succeded.
Invoking CU-DEVICE-GET succeded.
Invoking CU-CTX-CREATE succeded.
Invoking CU-DEVICE-COMPUTE-CAPABILITY succeded.
Invoking CU-MEM-ALLOC succeded.
Invoking CU-MEM-ALLOC succeded.
Invoking CU-MEM-ALLOC succeded.
Invoking CU-MEMCPY-HOST-TO-DEVICE succeded.
Invoking CU-MEMCPY-HOST-TO-DEVICE succeded.
nvcc -arch=sm_30 -I /Users/ichise/quicklisp/dists/quicklisp/software/cl-cuda-20170403-git/include -ptx -o /tmp/cl-cuda.g9K4wp.ptx /tmp/cl-cuda.g9K4wp.cu
Invoking CU-MODULE-LOAD succeded.
Invoking CU-MODULE-GET-FUNCTION succeded.
Invoking CU-LAUNCH-KERNEL succeded.
Invoking CU-MEMCPY-DEVICE-TO-HOST succeded.
verification succeed.
Invoking CU-MEM-FREE succeded.
Invoking CU-MEM-FREE succeded.
Invoking CU-MEM-FREE succeded.
Invoking CU-MODULE-UNLOAD succeded.
Invoking CU-CTX-DESTROY succeded.
NIL
```

ログでは「succeded」と表示されているので、成功したみたいです。

このあと、System updateが起動して、Command Line Toolsが元バージョンに戻りました。

